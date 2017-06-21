### socket.io
* web socket 是基于http的，以轮询方式处理事件。
    * 必然有的确定就是性能低，系统开销大。
    * 但是简单快捷，不在需要处理socket黏包之类的恶心东西。

* socket.io 服务器大致结构(*express v4.14.0, socket.io v1.4.6 ,node V7.6.0*)

```javascript
var app = require('express')();
var port =15151
var io = require('socket.io')(port);
exports.start = function(config){
    io.sockets.on('connection',function(sockey){
        //连接操作
        socket.on('disconnet',function(data){
            //远程客户端关闭事件
        });
        socket.on('selfping',function(data){
            //自定义心跳事件(socket.io有内置的ping事件)
        });
        socket.on('selfevent',function(data){
            //自定义事件
        });
    });
};

//心跳
function tick(){
    //....
}

setInterval(tick,5000);
```
* 客户端大致结构
```javascript
socket_a.on('connect',function(data){
    //连接事件(内置事件)
});

socket_a.on("disconnect",function(){
    //服务器断开连接事件(内置事件)
});

socket_a.on("selfping",function(data){
    //自定义心跳事件
});

socket_a.on('selfevent',function(data){
    //自定义事件
});

//心跳
function tick(){

}

setInterval(tick,10000);
```
* *遇到过一个坑，客户端的网络特殊断开后(比如拔掉网线)，并不会触发disconnect给服务器，如果服务器自己没心跳处理，那服务器将在socket超时后自己抛出一个disconnect事件，如果在这期间客户端重新连接上了，触发disconent事件后直接去关掉客户端socket，那么会触发一个诡异的情况，客户端可以一直发ping包，但是发不出去任何其它消息，所以在disconnect事件的时候，最好判断触发的socket.id是不是当前socket(这个是当时服务器框架的一个漏洞)*
* [一个握手建立加密通道的例子](https://github.com/ZhHong/zh_example/tree/master/node/socket.io)