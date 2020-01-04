# 【webrtc】webrtc通过peerconnection进行音视频互通（25）

![](https://img-blog.csdnimg.cn/20191205162849561.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM0MjczMDU5,size_16,color_FFFFFF,t_70)
###### js部分：包括媒体协商整个流程等
```shell
'user strict'

var localvideo = document.querySelector('video#localvideo');
var remoteVideo = document.querySelector('video#remotevideo');

var btnStart = document.querySelector('button#start');
var btnCall = document.querySelector('button#call');
var btnHangup = document.querySelector('button#hangup');

btnStart.onclick = start;
btnCall.onclick = call;
btnHangup.onclick = hangup;

var localStream;
var pc1,pc2;
function getMediaStream (stream){
    localvideo.srcObject = stream;
    localStream = stream;
}
function handleError(err){
    console.err('Failed to get stream')
}

function start(){
    if(!navigator.mediaDevices || !navigator.mediaDevices.getUserMedia){
        console.error('the getUserMedia is not support!')
        return;
    }else{
        var constraints = {
            video:true,
            audio:false
        }
        navigator.mediaDevices.getUserMedia(constraints).then(getMediaStream).catch(handleError)
    }
}

function getRemoteStream(e){
    remoteVideo.srcObject = e.streams[0];
}

function handleOfferError(err){
    console.error('Failed to create offer',err)
}

function handleAnswerError(err){
    console.log("Failed to create answer",err)
}

function getOffer(desc){
    pc1.setLocalDescription(desc);
    //send desc to signal
    //receive desc from signal
    pc2.setRemoteDescription(desc);

    pc2.createAnswer()
        .then(getAnswer)
        .catch(handleAnswerError);
}

function getAnswer(desc){
    pc2.setLocalDescription(desc);
    //send desc to signal和pc1进行交换
    //pc1 receive desc from signal

    pc1.setRemoteDescription(desc);
}

function call(){
    pc1 = new RTCPeerConnection();
    pc2 = new RTCPeerConnection();

    pc1.onicecandidate = (e) => {
        pc2.addIceCandidate(e.candidate)
    }

    pc2.onicecandidate = (e) => {
        pc1.addIceCandidate(e.candidate)
    }

    pc2.ontrack = getRemoteStream;

    localStream.getTracks().forEach((track) => {//localStream.getTracks()拿到音视频轨道；将本地采集的音视频流添加到peerconnection
        pc1.addTrack(track,localStream);
    });

    var offerOPtions = {
        offerToReceiveAudio:0,
        offerToReceiveVideo:1 
    }
    pc1.createOffer(offerOPtions)
        .then(getOffer)
        .catch(handleOfferError);
}

function hangup(){
    pc1.close();

    pc2.close();

    pc1 = null;
    pc2 = null;
}


```
###### html部分
```shell
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>webrtc PeerConnection</title>
</head>
<body>
    <div>
        <video id = 'localvideo' autoplay playsinline></video>
        <video id = 'remotevideo' autoplay playsinline></video>
        <div>
            <button id = "start">start</button>
            <button id = "call">Call</button>
            <button id = "hangup">Hang Up</button>
        </div>
    </div>
    <script src="./lib/adapter-latest.js"></script>
    <script src="./js/main.js"></script>
</body>
</html>
```
> [github地址](https://github.com/smileyqp/webrtc/tree/master/webserver),webserver/peerconnection文件夹下面，直接`node server.js`之后打开chrome浏览器输入`https://localhost:9999`,进入webserver下面的peerconnection文件中，打开index.html。注意`cert`下面的两个证书需要把自己的证书放到webserver/cert文件目录下面，如果没有的话也可以尝试改写https的为http的。