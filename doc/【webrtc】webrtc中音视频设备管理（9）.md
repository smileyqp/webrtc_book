# 【webrtc】webrtc中音视频设备管理（9）

![](https://img-blog.csdnimg.cn/20190919161221864.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM0MjczMDU5,size_16,color_FFFFFF,t_70)
![](https://img-blog.csdnimg.cn/20190919161353272.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM0MjczMDU5,size_16,color_FFFFFF,t_70)


```shell
//index.html中
<!DOCTYPE html>
<html>
<head>
  <title>webrtc test</title>
</head>
<body>
  <script src="./js/client.js"></script>  
</body>
</html>



//client.js中
'use strict';

if( !navigator.mediaDevices || !navigator.mediaDevices.enumerateDevices ){
    console.log('enumerateDevices is not support!')
}else{
    navigator.mediaDevices.enumerateDevices()
    .then(gotDevices)
    .catch(handleError);
}

function gotDevices(deviceInfos) {
    deviceInfos.forEach((deviceInfo) => {
        console.log(deviceInfo.kind + ':label = ' + deviceInfo.label + ':id = ' + deviceInfo.deviceId + ':groupId = ' + deviceInfo.groupId);
    });
}

function handleError (err){
    console.log(err);
}
```
![](https://img-blog.csdnimg.cn/20190919164415102.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM0MjczMDU5,size_16,color_FFFFFF,t_70)
> 目录结构 
> ![](https://img-blog.csdnimg.cn/20190919164652519.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM0MjczMDU5,size_16,color_FFFFFF,t_70)
> ![](https://img-blog.csdnimg.cn/20190919174034147.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM0MjczMDU5,size_16,color_FFFFFF,t_70)

> ##### [示例github地址](https://github.com/smileyqp/webrtc/tree/master/webserver/public/device)