# 【webrtc】webrtc采集屏幕数据，获取桌面（20）

![](https://img-blog.csdnimg.cn/20191203133616478.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM0MjczMDU5,size_16,color_FFFFFF,t_70)
![](https://img-blog.csdnimg.cn/2019120313485845.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM0MjczMDU5,size_16,color_FFFFFF,t_70)
> 调用的是`getDisplayMedia`，直接将上一条blo中的录制视频的`getUserMedia`改成`getDisplayMedia`就可以实现录制桌面视频、播放、下载等功能。只是调用的方法不一致，其他配置完全一样。
> ![](https://img-blog.csdnimg.cn/2019120313503290.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM0MjczMDU5,size_16,color_FFFFFF,t_70)