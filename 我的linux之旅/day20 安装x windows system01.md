# day20 安装x windows system01

***10-12***

前面无线网卡驱动安装失败后，今天闲着无聊装了一下xorg和gdm（gnome display manager），没想到安装gdm解决依赖时直接把gnome给装上了......随后更扯淡的事出现了：startx开启图形界面，然后我发现这里居然可以直接调节屏幕亮度、声音大小、使用蓝牙了？？？那我之前折腾了好长时间来想着安装intel的核显驱动岂不白白浪费......让我先呕血三升去

### 安装xorg

其实前几天已经尝试过安装这玩意，当时yum源是AppStream这个库，结果有若干个依赖的c库不存在，我估计是在BaseOS里面，但是一个一个解决太麻烦了就不了了之。今天重新想了想，我可不可以把BaseOS和AppStream两个库同时作为源而且不用修改配置文件？答案是可以的：

```
[root@localhost ~]# yum --disablerepo=\* --enablerepo=c8-media-BaseOS,c8-media-AppStream -y install xorg-x11-server-Xorg.x86_64
```

（我这里使用的还是本地光盘镜像源）这样子，原本AppStream里没有的c库就可以直接到BaseOS里解决了。于是xorg就装好了。

### 安装gdm

```
[root@localhost ~]# yum --disablerepo=\* --enablerepo=c8-media-BaseOS,c8-media-AppStream -y install gdm
```

直接贴图。

<div align="left"><img src="..\images\centos\gdm安装过程1.jpg" width = 600 height = 1000 /></div>

<div align="left"><img src="..\images\centos\gdm安装过程2.jpg" width = 600 height = 450 /></div>

上面两幅图是安装过程中显示的正在安装的包。下面三幅图是安装完毕后显示的gdm的依赖包：

<div align="left"><img src="..\images\centos\gdm安装过程5.jpg" width = 600 height = 200 /></div>

<div align="left"><img src="..\images\centos\gdm安装过程4.jpg" width = 600 height = 200 /></div>

<div align="left"><img src="..\images\centos\gdm安装过程3.jpg" width = 600 height = 200 /></div>

这些装完之后，我输入startx执行，结果就有了下面这个桌面：

<div align="left"><img src="..\images\centos\gnome桌面.jpg" width = 600 height = 360 /></div>



不得不说，centos8这一代的gnome3很棒了，原先用的centos6的gnome真滴蛮丑。

为甚么要详细记载这个呢？既然装好了gnome能够调节亮度声音、也能使用蓝牙，这就说明那些驱动是自带的呀！我还编译个什么源码包.....现在的任务就是在上面这些包里找到那些驱动包。比较可惜的是，wifi依然无法使用，暂时不知道是驱动的问题，还是服务没有开启什么的。

今天就到这里了，后面几天我可能得重新思考一下为什么要从最小化的字符界面来一步步搭建图形化界面，今天无意间装上的这个图形界面对我来所并没有多大用处，总的来说还是感觉很迷。就比如这个w7ifi为什么不能使用？目前驱动着我的显卡和声卡的是哪个软件？又如何管理？我没有头绪。短时间内估计也无法很好地给出答案。何况再过十几天就得正式开始准备研究生考试了。