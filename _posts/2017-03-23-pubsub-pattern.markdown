---
layout:     keynote
title:      "浅谈Javascript中publish/subscribe模式"
navcolor:   "invert"
date:       2017-3-23
author:     "Mill"
header-img: img/post-bg-pubsub-pattern.jpg
tags:
    - 前端开发
    - JavaScript
    - 设计模式
---

## 为什么要使用Publish/Subscribe设计模式
解耦，解耦，解耦！
我们都希望每个component做到独立，方便复用。
但同时又希望它们之间存在联系。
这时候就需要publish/subscribe模式。
以MC*框架为例，通常Model层会处理很多业务相关的逻辑，它可能需要和很多个View模块进行交互。
这时候如果采用p/s模式，model层就只需要publish一个event，相关的View模块就会接收到，进行更新。
我们也可以把这种模式认为是Observer模式。
## Publish/Subscribe模式的实现
#### 实现一：利用jQuery的callbacks
```javascript
var topics = {};
jQuery.Topic = function(id){
    var callbacks, topic = topics[id];
    if (!topic) {
        callbacks = jQuery.Callbacks();
        topic = {
            publish: callbacks.fire,
            subscribe: callbacks.add,
            unsubscribe: callbacks.remove
        };
        topics[id] = topic;
    }
    return topic;
}
$.Topic("message").subscribe(function(data) {
    console.log("a publish has occurred, get data:" + data);
});
$.Topic("message").publish('send');
```
#### 实现二：原生js
```javascript
var pubsub = {};
(function(myObject){
    //Storage for topic which can be broadcast or listened to.
    var topics = {};
    var uid = -1;
    myObject.publish = function(topic, args) {
        if (topics[topic] && topics[topic].length) {
            topics[topic].forEach(function(t){
                t.fn(topic, args);
            });
        }
    };
    myObject.subscribe = function(topic, fn) {
        if (!topics[topic]) {
            topics[topic] = [];
        }
        var token = (++uid).toString();
        topics[topic].push({
            token: token,
            fn: fn
        });
        return token;
    };
    myObject.unsubscribe = function(token) {
        for (var i in topics) {
            if (topics.hasOwnProperty(i)) {
                for (var len = topics[i].length; len--;){
                    if (topics[i][len].token === token) {
                        topics[i].splice(len, 1);
                        return token;
                    }
                }
            }
        }
        return this;
    };

})(pubsub);
var token = pubsub.subscribe('newMessage', function(topic, a){console.log("Log1:"+"Topic" + topic+ ",Message" + a)});
var token2 = pubsub.subscribe('newMessage', function(topic, a){console.log("Log2:"+"Topic" + topic+ ",Message" + a)});
pubsub.publish('newMessage', 'Hello');
pubsub.unsubscribe(token2);
pubsub.publish('newMessage', 'Hello');
```
