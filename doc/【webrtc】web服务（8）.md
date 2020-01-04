# 【webrtc】web服务（8）

![](https://img-blog.csdnimg.cn/20190919150149229.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM0MjczMDU5,size_16,color_FFFFFF,t_70)
```shell
'use strict';

var http = require('http');
var https = require('https');
var fs = require('fs');//读取证书

var express = require('express');
var serveIndex = require('serve-index')


var app = express();//实例化express模块

app.use(serveIndex('./public'));//浏览目录
app.use(express.static('./public'));//发布静态目录的位置；发布路径

//http server服务创建
var http_server = http.createServer(app);
http_server.listen(8080, '0.0.0.0');


//创建http server
var options = {
	key : fs.readFileSync('./cert/1557605_www.learningrtc.cn.key'),
	cert: fs.readFileSync('./cert/1557605_www.learningrtc.cn.pem')
}

var https_server = https.createServer(options, app);
https_server.listen(9999, '0.0.0.0');
```

![](https://img-blog.csdnimg.cn/20190919155936814.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM0MjczMDU5,size_16,color_FFFFFF,t_70)
![](https://img-blog.csdnimg.cn/20190919155959275.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM0MjczMDU5,size_16,color_FFFFFF,t_70)

> ##### [示例github地址](https://github.com/smileyqp/webrtc/tree/master/http)