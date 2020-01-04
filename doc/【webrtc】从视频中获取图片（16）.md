# 【webrtc】从视频中获取图片（16）

![](https://img-blog.csdnimg.cn/20190920123414987.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM0MjczMDU5,size_16,color_FFFFFF,t_70)
> 效果图片
> ![](https://img-blog.csdnimg.cn/20190920125045686.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM0MjczMDU5,size_16,color_FFFFFF,t_70)


> #### 思路：
> - 截取关键帧
> - canvas画出

```shell
//核心代码
//client.js

var snapshot = document.querySelector("button#snapshot");
var picture = document.querySelector("canvas#picture");
picture.width = 320;
picture.height = 240;

……



snapshot.onclick = function(){
    picture.className = filtersSelect.value;    //设置滤镜
    picture.getContext('2d').drawImage(videoplay,   //数据源
                                        0,0,            //开始和结束位置
                                        picture.width,      //画布宽
                                        picture.height)     //画布高
}


//html
 <video autoplay playsinline id = 'player'></video>

    <div>
        <button id='snapshot'>take snapShot</button>
    </div>
    <div>
        <canvas id='picture'></canvas>
    </div>
```