# 【webrtc】webrtc中音视频数据采集（10）

![](https://img-blog.csdnimg.cn/20190919174435159.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM0MjczMDU5,size_16,color_FFFFFF,t_70)
![](https://img-blog.csdnimg.cn/20190920094538667.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM0MjczMDU5,size_16,color_FFFFFF,t_70)
```shell
//client.js部分代码

'use strict';

var videoplay = document.querySelector('video#player')

function gotMediaStream (stream){
    videoplay.srcObject = stream;
}

function handleError (err){
    console.log(err);
}

if( !navigator.mediaDevices || !navigator.mediaDevices.getUserMedia ){
    console.log('getUserMedia is not support!')
}else{
    var constraints = {
        video : true,
        audio : true
    }
    navigator.mediaDevices.getUserMedia(constraints)
    .then(gotMediaStream)
    .catch(handleError);
}
```
> ##### [示例github地址](https://github.com/smileyqp/webrtc/tree/master/webserver/public/mediastream)