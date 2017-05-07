---
layout:     keynote
title:      "如何部署vue+webpack项目到heroku"
navcolor:   "invert"
date:       2017-5-7
author:     "Mill"
header-img: img/post-bg-stonehenge.jpg
tags:
    - vue
    - webpack
    - heroku
---
# 部署vue+webpack项目到heroku平台上
### 构建vue+webpack项目
```bash
npm run build
```
该命令会将静态文件打包到dist目录下，并且生成可被用于服务端的index.html 
### dist中创建构建文件
由于要把dist目录下的文件放到Heroku上，服务端需要知道如何处理那些静态文件 
进入dist文件夹，创建package.json 
```javascript
{
  "name": "vue-example",
  "version": "1.0.0",
  "description": "Vue project",
  "author": "xxx",
  "private": true,
  "dependencies": {
    "express": "^4.15.2",
    "http-proxy-middleware": "^0.17.4"
  }
}
```
这里用express作为server端框架，并采用http-proxy-middleware来代理特定请求 
### dist中创建server.js
```javascript
var express = require("express");
var proxy = require('http-proxy-middleware');

var options = {
  target: 'http://api.example.com',
  changeOrigin: true,
  pathRewrite: {
    '^/api' : ''
  }
}

app = express();
app.use(express.static(__dirname));
//pathrewrite会把目标url的路径作替换，localhost:5000/api/getFoods => http://api.example.com/getFoods
app.use('/api', proxy(options));

var port = process.env.PORT || 5000;
app.listen(port);
```
Heroku会调用```npm start```来运行我们部署上去的应用，这个文件就会被使用到 
我们可以在本地先测试是否能成功运行 

### 创建Heroku远程目录
首先需要安装Heroku-cli，根据官网 [Set up教程](https://devcenter.heroku.com/articles/getting-started-with-nodejs#set-up) 完成安装
创建Heroku远程目录，名字为example
```bash
heroku create example
```
确认远程目录已创建
```bash
git remote -v
# heroku  https://git.heroku.com/example.git (fetch)
# heroku  https://git.heroku.com/example.git (push)
```
添加远程目录,这样才能push到这个git目录
```bash
heroku git:remote -a example
```
接下来删除.gitignore文件中'dist/'，然后commit当前的改动
```bash
git add .
git commit -m "Adding dist folder"
```

### 把dist文件夹部署到heroku
只需要把dist文件夹中的内容部署到heroku上就能够运行
这里利用```git subtree```命令
```bash
git subtree push --prefix dist heroku master
```
接下来heroku就会帮我们部署express，启动整个app

### 运行app
当heroku构建成功后，就可以打开```https://example.herokuapp.com```，部署成功。

### 参考
[Quick-n-clean way to deploy Vue + Webpack apps on Heroku](https://medium.com/@sagarjauhari/quick-n-clean-way-to-deploy-vue-webpack-apps-on-heroku-b522d3904bc8)









