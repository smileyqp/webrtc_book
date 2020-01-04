# 【webrtc】webrtc视频采集、播放、下载（19）

> webrtc还是一个非常有意思的东西。怕忘记，简单记录一下webrtc采集数据和播放下载数据吧！

##### Step1:获取连接的设备，获取音视频流
```shell
function start(){
    if( !navigator.mediaDevices || !navigator.mediaDevices.getUserMedia ){
        console.log('getUserMedia is not support!');
        return;
    }else{
        var deviceId = videoSource.value;   //获取设备ID；用于当设备选择改变的时候改变设备
        var constraints = {
            video : {
                width:640,
                height:480,
                frameRate:30,    //帧率
                facingMode:'enviroment',    //facingMode摄像头的q前置还是后置的设置
                deviceId:deviceId ? deviceId :undefined
            },
            audio : {
                noiseSupression:true,   //降噪
                echoCancellation:true,  //回音消除设置成true
            },
            
        }
        navigator.mediaDevices.getUserMedia(constraints)
        .then(gotMediaStream)   //获取流
        .then(gotDevices)       //设备获取处理
        .catch(handleError);
    }
}
start();

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
    var videoTrack = stream.getVideoTracks()[0];//獲取視頻track；这里取第一個
    var videoConstraints =  videoTrack.getSettings();//這裏拿到video所有的約束
    
    divConstraints.textContent = JSON.stringify(videoConstraints,null,2)//轉成JSON，第一個參數是約束，第二個參數null，第三個參數是縮進的空格

    window.stream = stream;//挂载到全局；方便调用
    videoplay.srcObject = stream;

    return navigator.mediaDevices.enumerateDevices();   //成功获取流；并返回一个promise；用于后边对设备的判断
}

//错误处理
function handleError (err){
    console.log(err);
}

```
> 注意：这里面的`navigator`是系统自带的接口；可以拿到连接的设备信息
##### Step2:开始录制
```shell
btnRecord.onclick = () => {
    if(btnRecord.textContent === 'Start Record'){
        startRecord();
        btnRecord.textContent = 'Stop Record';
        btnPlay.disabled = true;
        btnDownload.disabled = true;
    }else{
        stopRecord();
        btnRecord.textContent = 'Start Record';
        btnPlay.disabled = false;
        btnDownload.disabled = false;
    }
}
function startRecord(){
    buffer = [];//定義buffer爲一個數組
    var options = {
        mimType:'video/webm;codecs=vp8'//音頻視頻同時有的時候是video只有音頻的時候是audio
    } 
    if(!MediaRecorder.isTypeSupported(options.mimType)){//這裏進行檢驗這個mimType是否支持
        console.error(`${options.mimType} is not supported!`)
        return;
    }
    try{
        mediaRecorder = new MediaRecorder(window.stream,options);
    }catch(e){
        console.log('Failed to create MediaRecorder,e')
        return;
    }
    mediaRecorder.ondataavailable = handleDataAvailable;//數據有效時候的處理
    mediaRecorder.start(10);//傳入時間片，每隔這個時間片存儲一次數據
}
function stopRecord(){
    mediaRecorder.stop();
}
function handleDataAvailable(e){
    if(e && e.data && e.data.size>0){
        buffer.push(e.data)
    }
}
```
> 写好相关button的事件，btnRecord是指开始录制的一个`button`。在`startRecord`方法中，初始化`buffer`数组，用于存储采集的数据。初始化`mediaRecorder = new MediaRecorder(window.stream,options)`，在mediaRecorder的ondataavailable数据有效时候处理，handleDataAvailable方法将数据push到buffer中`buffer.push(e.data)`。点击停止录制之后，调用`stopRecord`方法停止录制。之后`btnPlay`点击的时候，创建一个可以处理buffer对象的`Blob`，将存储在`buffer`中的数据转换成`Blob`之后，用`window.URL`创建一个url将video标签的src指向它`recvideo.src = window.URL.createObjectURL(blob)`，并且`recvideo.controls = true`控制播放，用`play()`方法调用播放。此时就已经完成了webrtc的数据采集和播放。
##### Step3:录制完成下载
```shell
btnDownload.onclick = () => {
    var blob = new Blob(buffer,{type:'video/webm'});
    var url = window.URL.createObjectURL(blob);
    var a = document.createElement('a');

    a.href = url;
    a.style.display = 'none';
    a.download = 'aaa.webm';
    a.click();
}
```
> 同播放，创建blob处理下载，创建url进行下载（第一次接触这种方法觉得有点意思）。很久没有时间好好写一篇blog记录了。webrtc真是一个强大有意思的东西。附上[github](https://github.com/smileyqp/webrtc/tree/master/webserver) ,当然cert文件中的证书相关的需要自己申请，github中没有上传。

###### 效果图
![](https://img-blog.csdnimg.cn/20191203133143976.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM0MjczMDU5,size_16,color_FFFFFF,t_70)
###### 采集数据下载的视频文件
![在这里插入图片描述](https://img-blog.csdnimg.cn/20191203133303355.png)