# dlib-opencv-openface

cd  opencv-2.4.11  
mkdir  release  
cd  release  
cmake -D CMAKE_BUILD_TYPE=RELEASE -D CMAKE_INSTALL_PREFIX=/usr/local ..  
make  -j8  
make  install 

登录 | 注册
itas109的专栏
十年磨一剑， 一朝试锋芒
目录视图摘要视图订阅
2017直通软考，拿证无忧      程序员简历优化指南！        程序员1月书讯     云端应用征文大赛，秀绝招，赢无人机！
 [置顶] Ubuntu 14.04下Openface的环境搭建
标签： openfaceface recognition人脸识别
2016-03-03 18:20 6891人阅读 评论(9) 收藏 举报
版权声明：本文为itas109原创文章，未经允许不得转载。【itas109@qq.com】

目录(?)[+]
如需转载请标明出处：http://blog.csdn.NET/itas109 

QQ技术交流群：129518033

 

 

一、什么是Openface？
Openface是一个基于深度神经网络的开源人脸识别系统。该系统基于谷歌的文章FaceNet: A Unified Embedding for Face Recognition and Clustering。Openface是卡内基梅隆大学的 Brandon Amos主导的。

 

官方地址：http://cmusatyalab.github.io/openface/

代码：https://github.com/cmusatyalab/openface

 



 

二、Openface环境搭建
系统：Ubuntu 14.04 64位桌面操作系统

参考：http://cmusatyalab.github.io/openface/setup/

1、Ubuntu切换root用户
此处不详述，如果要用普通用户，请自行测试。

参考文章：

http://blog.csdn.net/itas109/article/details/50679251

 

2、安装前准备工作
安装必要的程序，可以用下面的批处理，也可以一个一个的进行安装。

 

[cpp] view plain copy 在CODE上查看代码片派生到我的代码片
#!/bin/sh  
sudo apt-get install build-essential -y  
sudo apt-get install cmake -y  
sudo apt-get install curl -y  
sudo apt-get install gfortran -y  
sudo apt-get install git -y  
sudo apt-get install libatlas-dev -y  
sudo apt-get install libavcodec-dev -y  
sudo apt-get install libavformat-dev -y  
sudo apt-get install libboost-all-dev -y  
sudo apt-get install libgtk2.0-dev -y  
sudo apt-get install libjpeg-dev -y  
sudo apt-get install liblapack-dev -y  
sudo apt-get install libswscale-dev -y  
sudo apt-get install pkg-config -y  
sudo apt-get install python-dev -y  
sudo apt-get install python-pip -y  
sudo apt-get install wget -y  
sudo apt-get install zip –y  
 

3、安装必要的库
[cpp] view plain copy 在CODE上查看代码片派生到我的代码片
pip2 install numpy scipy pandas  
pip2 install scikit-learn scikit-image  

注意：

a.如果出现某一个安装失败的情况，可以一个一个的安装

 

b.提高pip安装速度

可以更换pip镜像加快下载速度
建立./pip/pip.conf，输入以下内容（或者其他可用镜像）：

[cpp] view plain copy 在CODE上查看代码片派生到我的代码片
[global]  
timeout = 6000  
index-url = http://pypi.douban.com/simple  
  
[install]  
use-mirrors = true  
mirrors = <a target=_blank href="http://pypi.douban.com/">http://pypi.douban.com/</a>  
 

c.报错：SSLError: The read operation timed out

可以用下列指令将延时加长

[cpp] view plain copy 在CODE上查看代码片派生到我的代码片
pip2 -install scikit-image --timeout 100  

 

4、安装Torch
a.安装依赖

[cpp] view plain copy 在CODE上查看代码片派生到我的代码片
curl -shttps://raw.githubusercontent.com/torch/ezinstall/master/install-deps  | bash –e  

 

b.安装

[cpp] view plain copy 在CODE上查看代码片派生到我的代码片
git clone https://github.com/torch/distro.git ~/torch --recursive  
cd ~/torch && ./install.sh  
 

c.安装依赖

luarocks install $NAME, where $NAME is as listed below.

dpnn

nn

csvigo

cunn (使用CUDA)

fblualib  (仅为了训练DNN)

torchx  (仅为了训练DNN)

 

命令行，按照需要安装：

[cpp] view plain copy 在CODE上查看代码片派生到我的代码片
~/torch/install/bin/luarocks install dpnn  
~/torch/install/bin/luarocks install nn  
~/torch/install/bin/luarocks install optim  
~/torch/install/bin/luarocks install csvigo  
~/torch/install/bin/luarocks install cunn  
~/torch/install/bin/luarocks install fblualib  
~/torch/install/bin/luarocks install torchx  
 

d.验证是否安装依赖成功

用th命令验证

 

注意：

a.gitclone更新网络老中断

 

Git submodule update --init –recursive

或者torch目录下的

Update.sh

 

建议用Update.sh解决

 

b.错误：

Cloning into'extra/luaffifb'...

remote:Counting objects: 918, done.

error: RPCfailed; result=56, HTTP code = 200| 0 bytes/s    

fatal: Theremote end hung up unexpectedly

fatal: earlyEOF

fatal:index-pack failed

Clone of 'https://github.com/facebook/luaffifb' intosubmodule path 'extra/luaffifb' failed

解决：

[cpp] view plain copy 在CODE上查看代码片派生到我的代码片
git config --global http.postBuffer 524288000  
 

5、安装opencv
OpenCV版本为2.4.11，下载地址：https://github.com/Itseez/opencv/archive/2.4.11.zip

编译参考：http://docs.opencv.org/2.4/doc/tutorials/introduction/linux_install/linux_install.html

 

a.指令下载：

[cpp] view plain copy 在CODE上查看代码片派生到我的代码片
cd ~  
mkdir  -p src  
cd  src  
curl  -L https://github.com/Itseez/opencv/archive/2.4.11.zip -o ocv.zip  

 

b.解压：

[cpp] view plain copy 在CODE上查看代码片派生到我的代码片
unzip  ocv.zip  
 

c.编译：

[cpp] view plain copy 在CODE上查看代码片派生到我的代码片
cd  opencv-2.4.11  
mkdir  release  
cd  release  
cmake -D CMAKE_BUILD_TYPE=RELEASE -D CMAKE_INSTALL_PREFIX=/usr/local ..  
make  -j8  
make  install  
 
d.验证

import cv2 


6、安装dlib
dlib v18.16下载地址：https://github.com/davisking/dlib/releases/download/v18.16/dlib-18.16.tar.bz2

 

a.安装编译

[html] view plain copy 在CODE上查看代码片派生到我的代码片
mkdir -p ~/src  
cd ~/src tar xf dlib-18.16.tar.bz2  
cd dlib-18.16/python_examples  
mkdir build  
cd build  
cmake ../../tools/python  
cmake --build . --config Release  
cp dlib.so /usr/local/lib/python2.7/dist-packages  
 

b.确保

在a中最后一条命令中，确保路径在默认的Python路径，可以在Python解释器里面用sys.path查找

For the final command, make sure the directory is in your default Python path, which can be found withsys.path in a Python interpreter.

 

c.验证

[cpp] view plain copy 在CODE上查看代码片派生到我的代码片
import dlib  
 

7、Git获取openface
a.下载Openface

[cpp] view plain copy 在CODE上查看代码片派生到我的代码片
git clone https://github.com/cmusatyalab/openface.git  
git submodule init  
git submodule update  
 

b.在Openface根目录执行

[cpp] view plain copy 在CODE上查看代码片派生到我的代码片
sudo python2 setup.py install  
 

python2一定要确保dlib和opencv安装成功

[cpp] view plain copy 在CODE上查看代码片派生到我的代码片
import cv2   
import dlib  
 

c.获取模型

[cpp] view plain copy 在CODE上查看代码片派生到我的代码片
models/get-models.sh  
 

8、运行demo
运行demo1：

./demos/compare.pyimages/examples/{lennon*,clapton*}

运行demo2：

./demos/classifier.py infermodels/openface/celeb-classifier.nn4.small2.v1.pkl ./images/examples/carell.jpg

运行demo3：

./demos/web/start-servers.sh

 

9、demo1可用浏览器
可用浏览器：

360浏览器极速模式

火狐浏览器

搜狗浏览器高速模式

 

不可用浏览器：

Chrome谷歌浏览器（可能与浏览器更新有关系，getUserMedia()）

IE浏览器

 

 

 

如需转载请标明出处：http://blog.csdn.Net/itas109 

QQ技术交流群：129518033
 

 

顶
2
踩
0
 
 
上一篇Ubuntu 14.04 root用户自动登录
下一篇在VS中添加lib的三种方法

参考知识库
img
Python知识库
18887关注|1324收录
img
.NET知识库
3003关注|815收录
img
OpenCV知识库
3285关注|1367收录
img
操作系统知识库
4941关注|2210收录
img
Git知识库
5665关注|612收录
img
软件测试知识库
3492关注|310收录
猜你在找
基于Ubuntu Core系统的DragonBoard 410c开发案例解析使用python操作OracleArcGIS for javascript 项目实战（环境监测系统）OpenCVchrome浏览器视频教程
MySQL+nginx+php环境在ubuntu1404下的搭建ubuntu1404下android开发环境的搭建5-2ubuntu的安装PHP 环境搭建要领 call to undefined function mysql_connect 深度学习笔记1_Ubuntu1404下caffe环境的搭建无GPU版本以及python可视化环境的配置64位Ubuntu1404下配置PBC环境

查看评论
5楼 d844819903 2016-07-20 11:24发表 [回复]

4、安装Torch
a.安装依赖
curl -shttps://raw.githubusercontent.com/torch/ezinstall/master/install-deps | bash –e
时报错：bash: –e: 没有那个文件或目录，怎么回事啊
Re: 蔻蔻a 2016-11-29 14:20发表 [回复]

回复d844819903： s htttps 中间有空格 然后最后e前面是-不是–
Re: 我叫Pascal 2016-12-06 16:36发表 [回复]

回复蔻蔻a：有用
4楼 hitchina 2016-06-27 16:53发表 [回复]

感谢博主分享的好东西。我遇到一些问题想请教下，我在训练一个新的DNN模型的时候，./training/main.lua -data data/mydataset/aligned 。 总是遇到一个libGraphicsMagickWand.so: cannot open shared object file: No such file or directory的错误。不知道您是否遇到过？谢谢。
3楼 hexinzhenzi 2016-04-25 16:21发表 [回复]

你好，训练模型需要装依赖fblualib (only for training a DNN)，但是需要从goole下载东西，怎么办？
Re: 希希梦 2016-07-02 08:31发表 [回复]

回复hexinzhenzi：请问demo1运行，一定需要这个吗？
Re: Jocelyn_ying 2016-05-18 20:32发表 [回复]

回复hexinzhenzi：https://github.com/facebook/fblualib
这个网址下有你要的东西
2楼 liuzhongwei425 2016-04-11 10:15发表 [回复]

您好，本人刚接触LINUX，非常感谢您写的这么详细！按照您的步骤配置，sudo pip2 install scikit-learn scikit-image之后，出现错误，请问是什么原因

。。。

Running setup.py (path:/tmp/pip_build_root/scikit-image/setup.py) egg_info for package scikit-image
Unable to find pgen, not compiling formal grammar.
No eggs found in /tmp/easy_install-yEFD1d/Cython-0.24/egg-dist-tmp-zsyHcX (setup script problem?)

。。。

Cleaning up...
Command python setup.py egg_info failed with error code 1 in /tmp/pip_build_root/scikit-image
Storing debug log for failure in /home/xiang/.pip/pip.log
1楼 hexinzhenzi 2016-04-08 10:05发表 [回复]

特别好，特别有帮助！
您还没有登录,请[登录]或[注册]
* 以上用户言论只代表其个人观点，不代表CSDN网站的观点或立场
核心技术类目
全部主题 Hadoop AWS 移动游戏 Java Android iOS Swift 智能硬件 Docker OpenStack VPN Spark ERP IE10 Eclipse CRM JavaScript 数据库 Ubuntu NFC WAP jQuery BI HTML5 Spring Apache .NET API HTML SDK IIS Fedora XML LBS Unity Splashtop UML components Windows Mobile Rails QEMU KDE Cassandra CloudStack FTC coremail OPhone CouchBase 云计算 iOS6 Rackspace Web App SpringSide Maemo Compuware 大数据 aptech Perl Tornado Ruby Hibernate ThinkPHP HBase Pure Solr Angular Cloud Foundry Redis Scala Django Bootstrap
个人资料
 访问我的空间 
itas109
 
 2
访问：155928次
积分：2371
等级： 
排名：第12967名
原创：66篇转载：21篇译文：1篇评论：154条
公告
觉得文章对你有帮助，请扫描二维码捐赠给博主，谢谢！

 
 

网易博客:

http://itas109.blog.163.com/

 

QQ交流群:

CSDN博客专用技术交流
 


阅读排行
STC89C52单片机通过HC-06蓝牙模块与Android手机通信(11657)
蓝牙4.0设计 CC2540(9293)
﻿﻿无线蓝牙串口模块 HC-06从机-----AT指令以及其他测试报告(7279)
Ubuntu 14.04下Openface的环境搭建(6887)
VMware中Ubuntu 14.04出现Unknown Display问题解决(6817)
解决CC2540 XDATA内存不足(6776)
CC2540 BLE PeripheralBroadcaster Example（蓝牙4.0从机和广播者多角色实例）(6696)
浅析CC2540的OSAL原理(6410)
Android的重力传感器（3轴加速度传感器）简单实例(4683)
华为G610（Android 4.2）永久关闭键盘灯的方法(4142)
评论排行
STC89C52单片机通过HC-06蓝牙模块与Android手机通信(40)
手机与单片机通过蓝牙通信----手机控制灯(23)
CSerialPort串口类最新修正版2016-08-10(19)
蓝牙4.0设计 CC2540(16)
华为G610（Android 4.2）永久关闭键盘灯的方法(10)
浅析CC2540的OSAL原理(10)
Ubuntu 14.04下Openface的环境搭建(9)
【源码】基于Android和蓝牙的单片机温度采集系统(9)
CSerialPort串口类的修正版2014-01-10(2)
基于CSerialPort修改类的串口调试助手源代码（支持中文、自动保存等）(2)
文章分类
Linxux(4)
ARM(0)
单片机(2)
错误总结(1)
Android(14)
Linux交叉编译(3)
蓝牙(8)
OBD(2)
技巧(5)
杂谈(3)
传感器(1)
串口(5)
andengine(1)
VS+QT+VTK(3)
QT(4)
面经(2)
PHP(1)
MySQL(2)
C(1)
NDK(1)
VTK(1)
git(2)
mfc(1)
Machine Learning(2)
SQL Server(1)
MyEclipse(1)
人脸识别(0)
visual studio(9)
数据结构(1)
github(2)
Windows编程(1)
串口 CSerialPort(0)
C++(1)
dingtalk(0)
解决方案(1)
文章存档
2017年01月(2)
2016年12月(1)
2016年10月(2)
2016年09月(1)
2016年08月(7)
展开
最新评论
【源码】基于Android和蓝牙的单片机温度采集系统
qq_31206851: 楼主，我现在做的这个和你的差不多，也是HC05蓝牙模块，只不过采集的是体重数据罢了，不过我在安卓端这...
【源码】基于Android和蓝牙的单片机温度采集系统
qq_31206851: 楼主，你好，我下了你的源码了，我现在做的毕业设计其实和你的差不多，只不过我采集的是体重数据罢了，在用...
C++开源日志库Glog的使用（VS2010）
woailiuxiaochang: 很详细~
蓝牙4.0设计 CC2540
y26183225: 非常详细！！楼主好棒！
Ubuntu 14.04下Openface的环境搭建
我叫Pascal: @cmd1994:有用
Ubuntu 14.04下Openface的环境搭建
蔻蔻a: @d844819903: s htttps 中间有空格 然后最后e前面是-不是–
绕任意向量旋转分解到坐标系旋转
lijiayu2015: 有用，谢谢博主
STC89C52单片机通过HC-06蓝牙模块与Android手机通信
weita5711: 大神，hc-05的蓝牙模块可以用吗？怎么改程序啊求赐教
手机与单片机通过蓝牙通信----手机控制灯
weita5711: 蓝牙模块是hc-05的可以用吗
CSerialPort串口类最新修正版2016-08-10
itas109: @linhui568:这个不是有自己的通信库吗
