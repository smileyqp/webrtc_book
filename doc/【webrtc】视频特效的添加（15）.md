# 【webrtc】视频特效的添加（15）

![](https://img-blog.csdnimg.cn/20190920112907402.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM0MjczMDU5,size_16,color_FFFFFF,t_70)
![](https://img-blog.csdnimg.cn/20190920112919378.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM0MjczMDU5,size_16,color_FFFFFF,t_70)

>  invert
>  ![](https://img-blog.csdnimg.cn/2019092012223799.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM0MjczMDU5,size_16,color_FFFFFF,t_70)
>  none
>  ![](https://img-blog.csdnimg.cn/20190920122516979.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM0MjczMDU5,size_16,color_FFFFFF,t_70)

> gray
> ![在这里插入图片描述](https://img-blog.csdnimg.cn/20190920122407948.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM0MjczMDU5,size_16,color_FFFFFF,t_70)

>sepia
>![](https://img-blog.csdnimg.cn/201909201224431.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM0MjczMDU5,size_16,color_FFFFFF,t_70)

```shell
//html
<!DOCTYPE html>
<html>
<head>
  <title>webrtc test</title>
  <style>
      .none{
          -webkit-filter:none;
      }
      .blur{
        -webkit-filter:blur(3px);
      }
      .grayscale{
          -webkit-filter:grayscale(1);
      }
      .invert{
          -webkit-filter:invert(1);
      }
      .sepia{
          -webkit-filter:sepia(1);
      }
  </style>
</head>
<body>
    <div>
        <label>audioSource音频输入: </label>
        <select id="audioSource">
    </select>
    </div>
    <div>
        <label>audioOutput音频输出: </label>
        <select id="audioOutput">
    </select>
    </div>
    <div>
        <label>videosource: </label>
        <select id="videoSource">
    </select>
    </div>
    <div>
        <label>filter加一点点特效: </label>
        <select id="filter">
            <option value='none'>None</option>
            <option value='bluer'>Blur</option>
            <option value='grayscale'>grayscale</option>
            <option value='invert'>invert</option>
            <option value='sepia'>sepia</option>
        </select>
    </div>
    

    <video autoplay playsinline id = 'player'></video>
    <script src="./lib/adapter-latest.js"></script>
    <script src="./js/client.js"></script>  
</body>
</html>
```

```shell
//改变video的class；通过样式来设置效果
filtersSelect.onchange = function(){        //视频特效
    videoplay.className = filtersSelect.value;
}

```