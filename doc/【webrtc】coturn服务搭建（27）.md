# 【webrtc】coturn服务搭建（27）

![](https://img-blog.csdnimg.cn/20191205183026474.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM0MjczMDU5,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20191205183043911.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM0MjczMDU5,size_16,color_FFFFFF,t_70)

###### 一、下载corturn
> [github地址](https://github.com/coturn/coturn)，git clone到本地
```shell
//将安装的文件克隆到本地
git clone https://github.com/coturn/coturn
//安装依赖libevent-dev
sudo apt-get install libevent-dev
//进入corturn执行下面的代码。注意此处后面的路径是安装的路径，可以自选；执行成功如下
 ./configure --prefix=/usr/local/coturn
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/2019120611402371.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM0MjczMDU5,size_16,color_FFFFFF,t_70)
> 执行成功之后查看makefile

```shell
ls -alt Makefile
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20191206114150711.png)
######  二、使用make工具，可以使用多线程进行并行的编译（一般后面是内核的两倍，例如：电脑内核为6，这里-j后面为12）
```shell
make -j 12
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20191206114358463.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM0MjczMDU5,size_16,color_FFFFFF,t_70)
######  三、最后install
```shell
sudo make install 
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20191206114447111.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM0MjczMDU5,size_16,color_FFFFFF,t_70)
> 安装完毕，在`/usr/local/corturn`下面就可以看到安装的corturn了
> `/bin`下面就是可执行的服务
> `/etc`下面就是一些配置
> `/include`下面是一些头文件
> `/lib`下面是一些库文件

![在这里插入图片描述](https://img-blog.csdnimg.cn/20191206114948340.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM0MjczMDU5,size_16,color_FFFFFF,t_70)
###### 修改turnserver.conf.default文件
```shell
smileyqp@smileyqp:/usr/local/coturn/etc$ sudo vim turnserver.conf.default 
//简单设置几个参数
turnserver.conf.default 
user=smileyqp:123456
listening-port=3478
```
###### 环境变量中添加coturn
```shell
sudo vim ~/.bashrc 
export PATH=/usr/local/coturn/bin
source  ~/.bashrc 
```
###### 开启服务
```shell
//如果没添加到环境变量种种就用，加了就直接turnserver
./bin/turnserver -c ./etc/turnserver.conf.default 
```
```shell
//查看turn服务是否启动
ps -ef | grep turn
```