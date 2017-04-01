---
layout:     keynote
title:      "CSS3 Flex布局详解"
navcolor:   "invert"
date:       2017-4-1
author:     "Mill"
header-img: img/post-bg-kaiser-wilhelm.jpg
tags:
    - CSS3
    - layout
---
# CSS3 Flex布局详解

Flex布局是CSS3中一个特别重要的布局方式，它也将成为未来前端页面布局的首选。  
这篇文章的内容主要参考
[A Complete Guide to Flexbox](https://css-tricks.com/snippets/css/a-guide-to-flexbox/#flexbox-background)
和
[Flex 布局教程：语法篇](http://www.ruanyifeng.com/blog/2015/07/flex-grammar.html?utm_source=tuicool)
## Flex布局的基本概念
![Flex basic concept](http://onppapi6x.bkt.clouddn.com/flex-background.png)  
Flex布局是一整套属性，而不是单一属性。它需要有个flex container容器，这个容器的所有子元素都成为flex item。  
容器有两条轴，一条是主轴，一条是交叉轴，默认情况下，主轴是水平轴，而交叉轴是垂直轴。  
主轴开始的位置是```main start```, 结束位置是```main end```,交叉轴同理。每个item占据的主轴空间是```main size```, 交叉轴空间是```cross size```。
## 容器的属性
### 1. flex-direction
该属性决定主轴的方向，有4个值：  
 -**row**： 主轴为水平方向，且从左端开始  
 -**row-reverse** 主轴为水平方向，且从右端开始  
 -**column**： 主轴为垂直方向，且从上端开始  
 -**column-reverse**： 主轴为垂直方向，且从下端开始  

### 2. flex-wrap
该属性定义了主轴上排不下该如何换行  
 -**nowrap**： 不换行  
 -**wrap**： 换行，第一行在上方  
 -**wrap-reverse**： 换行，第一行在下方  
### 3. flex-flow
该属性是```flex-direction```和```flex-wrap```的简写模式。  
### 4. justify-content
该属性定义了item在主轴上的对齐方式  
 -**flex-start**： 左对齐  
 -**flex-end**： 右对齐  
 -**center**： 居中  
 -**space-between**： 两端对齐，item的中间的间隔相等，两端无空间  
 -**space-around**： 每个item两侧的间隔均相等  
### 5. align-items
该属性定义了item在交叉轴上的对齐方式  
**flex-start**， **flex-end**，**center**， **baseline**， **stretch**  
### 6. align-content
该属性定义了多根轴线的对齐方式，如果只有一根轴线，则该属性不起作用  
**flex-start**， **flex-end**，**center**， **space-between**， **space-around**, **stretch** 

### 实例
Ex1. **align-items**: ```center```,  **justify-content**: ```flex-end```  

![Container example1](http://onppapi6x.bkt.clouddn.com/justify-content-flex-end-align-items-center.png)
交叉轴也就是垂直方向居中，水平轴上右对齐，这里的每个item是带有蓝色背景的图片。  

Ex2. **align-content**: ```center```, **justify-content**: ```flex-end```  

![Container example2](http://onppapi6x.bkt.clouddn.com/justify-content-flex-end-align-content-center.png)
这里把container的高度调成了400px，而每个图片的高度为100px。  
自动wrap容不下的那张图，align-content设为center使两条主轴在container里垂直居中。

## Item的属性
### 1. order
该属性定义Item的排列顺序，数值越小，排列越靠前。
### 2. flex-grow
该属性定义Item的放大比例，默认是0，也就是不放大。  
加入所有item的属性都是1，那它们会等分剩余空间。
### 3. flex-shrink
该属性是Item的缩小比例，默认是1，也就是空间不足时会缩小。
如果某个item的属性是0，那空间不足时，它不缩小。
### 4. flex-basis
该属性为Item占据主轴的空间，默认为auto，可以设成某个px值,或者百分比等等。
### 5. flex
该属性是```flex-grow```, ```flex-shrink```, ```flex-basis```的简写。
### 6. align-self
该属性允许单个项目有与其他item不一样的对齐方式，可覆盖```align-items```
### 实例
Ex1. ```flex-grow```：三个div的属性值分别为1，2，1  

![Item example2](http://onppapi6x.bkt.clouddn.com/flex-grow-1-2-1.png)
可以看出三个item等分了Container的剩余空间。
## 综合实例
### 1. 导航菜单
这里以响应式的导航菜单为例。  
<p data-height="265" data-theme-id="0" data-slug-hash="qrLNzO" data-default-tab="result" data-user="mewwind" data-embed-version="2" data-pen-title="Demo Flexbox 2" class="codepen">See the Pen <a href="http://codepen.io/mewwind/pen/qrLNzO/">Demo Flexbox 2</a> by Mewwind (<a href="http://codepen.io/mewwind">@mewwind</a>) on <a href="http://codepen.io">CodePen</a>.</p>
<script async src="https://production-assets.codepen.io/assets/embed/ei.js"></script>

浏览器宽度 > 800px  
 **flex-flow**: ```row wrap```, **justify-content** ```flex-end```;
![Nav menu > 800px](http://onppapi6x.bkt.clouddn.com/navmenu-gt800px.png)  

浏览器宽度在600px和800px之间  
 **flex**: ```1 0 auto```; flex-grow被设为1，各个item等分整个container。
![Nav menu > 800px](http://onppapi6x.bkt.clouddn.com/navmenu-800px.png)

浏览器宽度小于600px  
 **flex-flow**: ```column wrap```
![Nav menu > 800px](http://onppapi6x.bkt.clouddn.com/navmenu-600px.png)

### 2. 圣杯布局
这里以经典的圣杯布局为例。
<p data-height="265" data-theme-id="0" data-slug-hash="jqzNZq" data-default-tab="result" data-user="css-tricks" data-embed-version="2" data-pen-title="Demo Flexbox 3" class="codepen">See the Pen <a href="http://codepen.io/team/css-tricks/pen/jqzNZq/">Demo Flexbox 3</a> by CSS-Tricks (<a href="http://codepen.io/css-tricks">@css-tricks</a>) on <a href="http://codepen.io">CodePen</a>.</p>
<script async src="https://production-assets.codepen.io/assets/embed/ei.js"></script>

浏览器宽度 > 800px  
蓝色的```flex-grow```设为3，黄色和粉色都为1
蓝色的```order```为2也就是排在黄色后边， 

浏览器宽度在600px到800px之间  
蓝色红色绿色的```flex-basis```为100%，而黄色和粉色仍是auto

浏览器宽度小于600px  
所有的```flex-basis```都为100%

