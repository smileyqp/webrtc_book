# 【webrtc】获取设备权限（12）

 > 注意：之前直接获取音视频设备的时候，设备的label数据并没有；但是，在获取音视频流的时候，浏览器会弹窗提醒权限允许，之后再去获取音视频设备的label等是有数据的

 ```shell
 'use strict';

var videoplay = document.querySelector('video#player')


//设备的展示与选择
var audioSource = document.querySelector("select#audioSource");
var audioOutput = document.querySelector("select#audioOutput");
var videoSource = document.querySelector("select#videoSource");
function gotDevices(deviceInfos) {      //参数deviceInfos是设备信息的数组
    deviceInfos.forEach((deviceInfo) => {
        console.log(deviceInfo.kind + ':label = ' + deviceInfo.label + ':id = ' + deviceInfo.deviceId + ':groupId = ' + deviceInfo.groupId);
        var option = document.createElement('option');
        option.value = deviceInfo.deviceId;
        option.text = deviceInfo.label;
        if(deviceInfo.kind === 'audioinput'){       //deviceInfo.kind来判断种类;音频
            audioSource.appendChild(option);
        }else if(deviceInfo.kind === 'audiooutput'){        //音频输出
            audioOutput.appendChild(option);
        }else if(deviceInfo.kind === 'videoinput' ){           //视频
            videoSource.appendChild(option);
        }

    });
}

//获取到流
function gotMediaStream (stream){
    videoplay.srcObject = stream;
    return navigator.mediaDevices.enumerateDevices();   //成功获取流；并返回一个promise；用于后边对设备的判断
}

//错误处理
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
    .then(gotMediaStream)   //获取流
    .then(gotDevices)       //设备获取处理
    .catch(handleError);
}
 ```

 > 效果图：
 > ![](https://img-blog.csdnimg.cn/20190920103857991.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM0MjczMDU5,size_16,color_FFFFFF,t_70)