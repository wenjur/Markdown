# day1 centos8安装成功

***9月23日***

在[阿里镜像](http://mirrors.aliyun.com/centos/8.2.2004/isos/x86_64/) 下载了centos8镜像后，系统安装成功，至于安装详细过程，以后有空再填坑吧

先放图（可以注意html的实现）

<div align="left"><img src="..\images\centos8界面.jpg" width = 600 height = 200 /></div>

可以看到我是以minimal版本安装的，不含图形界面，干啥啥不行，想连个网，嗯？可是wifi开关在哪里？想用一下excel，呵呵。所以，在这只有字符界面的Linux系统，我能干什么？

### 我的Linux目标

将Linux打造成类似于macOS的可以日常使用的系统，在这一过程中实践并进一步熟悉linux是我的终极目标。发行版的选择原先是arch的，但听说arch初始安装很麻烦，所以就暂时先拿熟悉的centos试一下。centos/redhat系列的适合做企业级服务器来使用，而ubuntn/arch这样的才适合玩桌面，等centos熟练了之后，再弄arch吧。

### 短期内待解决的问题

这部分的问题是我刚刚安装好centos就碰到的。首先是各种驱动问题，最最最重要的是显卡驱动，没有显卡驱动的话默认最大亮度显示，现在字符界面还好（绿色字都快闪瞎了我的双眼），等图形界面装好后就更可怕了，然后是声卡驱动、蓝牙驱动、无线网卡驱动什么的，这些估计都挺麻烦的

我需要要将centos装上图形化界面，然而，安装kde图形界面需要centos先联网，因为我是最小化安装，所以我的系统现在连最基本的网络管理工具都没有，又何谈给centos联网呢？

安装网络管理工具使用yum包管理工具，软件源有两个

* 一是使用红帽系列的yum服务器，这个是需要联网安装工具包的
* 二是使用安装系统时使用到的ISO镜像文件，里面包含了centos基本的管理工具和图形界面之类的，可以离线本地安装

这里我需要使用ISO文件本地源安装系统基本管理工具。

### 让centos和windows共享数据

安装管理工具总得让linux知道镜像文件在哪里，对吧。可以把镜像文件放在u盘里，然后插入电脑，在centos里访问U盘里的ISO文件，之后就可以安装包管理工具了。然而我想通过一块相同的硬盘分区让windows和Linux共享数据。这块分区的格式要求是Windows和Linux都能识别的vfat文件格式。建立共享磁盘分区的工作我还没有开始，这一部分就先写到这里了。

