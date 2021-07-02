# day27 安装桌面

***10-19***

经过几天的折腾，我决定放弃自己原先的路线，我是有多么想不开要自己一个一个地装驱动~直接安装桌面会提供一些驱动。在这之后结合鸟哥的书和谷歌重新来系统地认识centos8。

首先是安装桌面：我使用的是群组安装，已安装的群组有`Workstation`、`System Tools`、`Development Tools`、`Graphical Administrator Tools`，和`Server with GUI`、`Workstation`里的一些可选群组，还有一些其他的我没有详细地一一记录。

安装完`Workstation`后，默认提供了声卡、蓝牙、显卡的驱动，其中显卡疑似使用vesa驱动，其余两个暂不清楚。很遗憾的是依然没有wifi的驱动，我还是不能联网。值得一提的是，centos8已经默认使用wayland作为x图形服务器了，当然xorg依然做为可选项兼容，这个在gdm登录界面那个设置里可以选择。

尝试了使用xgamma来调节终端亮度，还是报错cannot open display，但是我在查询xorg的man手册时，意外地发现Xorg这个指令有一个gamma选项，后面接数字0.0~10.0，这个和xgamma的用法很像，尝试使用了一下：

```
Xorg -gamma 1.0
```

结果黑屏了......此时有点懵逼，等了一会儿没有反应后，我打开tty2，想干掉刚刚那个进程，神奇的是亮度竟然变了~切回tty1，这时tty1可以正常显示了。多尝试几次后发现，这个Xorg -gamma确实可以调节亮度（按照man手册来说叫灰度系数gamma），问题就是会黑屏，切换一下tty就可以了。尽管如此，这用的不爽。更何况我发现gnome里的terminal蛮漂亮的，还能多窗口，那还用什么终端嘛。

经过一天的折腾，我收回自己想不开的那句话，应该改为：我是有多么想不开非要拿centos8作为桌面来折腾。

今天复习完第十章bash后，果断打开`~/.bashrc`文件，加上两句常用的指令：

```
alias lla='ls -al'
alias yum='yum --disablerepo=\* --enablerepo=c8-media-BaseOS,c8-media-AppStream'
```

<div align="left"><img src="..\images\centos\root环境设置.jpg" width = 600 height = 300 /></div>

这样修改完之后，以后我以root用户使用yum安装就不需要敲长长的一串了，这个设置的原理是怎么回事？等下一篇来详细总结，这个涉及到bash的环境配置。其实配置本地源、换源是有更好的方法的，等网络问题弄好了就来说明。