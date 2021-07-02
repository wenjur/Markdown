# day21 梳理x windows system启动过程

***10-13***

在此梳理记录一下x windows 图形窗口的启动过程，在此之前要先**按照我自己的理解**介绍一下几个名词

## 常见术语

linux本身是基于命令行的操作系统，不带图形界面的。平时也会见到一些图形界面比较漂亮的linux系统比如ubuntn、arch等，其实这个图形界面是一整套软件，包含很多东西比如x server、x client、window manager、display manager、应用程序wps office、firefox等等，称这一整套为x windows system。

* X、X11R6/7

X是图形接口的协议，一切的根本，X11R6/7其实是X protocol version 11Realease 6，就是说X协议第11版第六次发行。X这个协议规定了什么我暂时描述不清楚，可能是规定了x server和x window manager/client之间的交互吧。

* x server/client

x server这个软件负责管理计算机上的显示硬件和驱动程序以进行图像绘制，包括但不限于显示器的字体、分辨率、刷新率、显卡驱动和显卡等等。而client处理来自x server的请求，告知x server如何绘制图形。常见的一些应用程序比如wps、火狐等等图形软件其实就是x client。还有狭义的gnome、kde也是特殊的x client，负责协调x client之间的显示。这个狭义是我自己编的，广义的gnome、kde不仅仅是一个窗口管理器，还包含配套的应用软件、桌面环境比如任务栏、开始菜单等等。

* xorg、xfree86

俩都是实现了X协议的软件，就是这个x server，不过实际说的xorg软件肯定不单单指一个x server。这里还有一些历史问题，xfree86是1992年由XFree86计划维护的软件，后来由于授权问题，XFree86计划无法提供类似GPL的自由软件，这个软件就交给Xorg基金会维护了。所以这个xorg软件和xfree86到底什么关系我还没有查到。

## x windows system启动过程

设想我现在处于root用户已登陆的命令行界面下。

输入startx，启动图形界面，这一过程