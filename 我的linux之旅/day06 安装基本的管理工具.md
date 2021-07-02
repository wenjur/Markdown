# day06 安装基本的管理工具

***9-28***

本来想先分个vfat格式的区，直接把镜像文件放到这里当本地yum源，省的后面想装个什么东西折腾一堆。结果：好家伙，连分区命令gdisk都没有......那我就直接插上u盘，在windows下拷贝CentOS-8.2.2004-x86_64-dvd1文件到U盘，纳尼？无法复制？因为镜像文件过大！我的u盘只有16G，经查询fat32u盘最大支持4G单一文件，而镜像7.66G。这我该说什么好。。。

最后的解决方案是用CentOS-8.2.2004-x86_64-minimal作为本地源。

### 已有的设备或软件

* u盘两个，金士顿8G，闪迪16G。
* 镜像文件两个，centos8的minimal和dvd1两个版本
* 电脑双系统，windows和linux，windows下有ultraISO软件

### 挂载u盘

**首先需要挂载usb设备**。不像windows插入u盘就可以直接读写，linux需要先挂载u盘才能读写。

在/mnt目录下新建/usb目录及/iso9660，目录名随便取啦，无所谓的，当然建议按规范来。

```
[root@localhost mnt]# mkdir usb
[root@localhost mnt]# mkdir iso9660
```

使用blkid列出系统当前已识别设备的参数：

```
[root@localhost mnt]# blkid
...
/dev/sdb4: LABEL="CENTOS" UUID="1EBD-461D" TYPE="vfat" PARTUUID="cad4ebea-04"
...
```

可以看到卷标LABEL为CENTOS，文件格式TYPE="vfat"，这个就是我的u盘了。显然设备名是/dev/sdb4。

然后将设备/dev/sdb4挂载到/mnt/usb：

```
[root@localhost mnt]# mount /dev/sdb4 /mnt/usb
```

这里因为我的iso文件放在u盘里，所以下一步将iso文件挂载到/mnt/iso9660：

```
[root@localhost mnt]# mount /mnt/usb/CentOS-8.2.2004-x86_64-minimal.iso /mnt/iso9660
```

（老实说，上面这一步为什么能成功，我也不晓得，我记得当时学mount指令时好像没有这么个用法），挂载成功后会有个只读保护的警告，这个没有影响。需要注意的是，如果iso文件是直接刻录到u盘了（装系统的u盘还保留着的话），直接挂载u盘就ok的。

### 设置本地yum源

redhat/centos系列有三种方式安装软件，一个是rpm手动安装，自己解决软件的依赖问题，巨麻烦。还有一种是yum管理工具，只要软件源有repodata这个目录，yum就可以自己解决依赖问题，相当的好用啊。最后一种，呵呵，我不想提。所以这里我选择yum工具。

需要说明的是，yum的软件源有两种，在线源和本地光盘镜像源。**在正式安装之前，需要修改yum的配置文件，设置本地ios镜像为yum源，毕竟我没有网络嘛**。

不过在此之前又有想法了，我上面使用的是minimal版的镜像，万一里面的软件不够呢？再说，难道使用dvd1完整的不是更好吗？，所以，还是决定牺牲一个u盘用来刻录完整版的镜像文件。这样一来，上面挂载u盘那一步就可以直接将u盘挂载到/mnt/usb下即可，无需再挂载iso文件，现在，我的u盘即是我的安装光盘，而不是那个iso文件了。

yum配置文件位于`/etc/yum.repos.d/`，这个目录下的几个文件便是yum各种源的配置文件了。**我需要做的修改是，让本地源的配置文件里的软件库生效，设置baseurl指定本地源的位置，设置字段enabled=1**。

查看`yum.repos.d`目录，我却发现了好多好多配置文件：

<div align="left"><img src="..\images\yum配置文件列表.jpg" width = 600 height = 240/></div>

在centos6版本中只有`CentOS-Base.repo`，`CentOS-Debuginfo.repo`，`CentOS-Media.repo`， `CentOS-Vault.repo`这四个，但是我现在用的centos8中为什么会有这么多呢？

对于centos6来说，yum源默认生效的是`CentOS-Base.repo`，这里存放着yum源在线安装时要查询的镜像网址，打开后有下面这些内容：

<div align="left"><img src="..\images\CentOS-Base软件库.jpg" width = 600 height = 320/></div>

除了基本的`[base]`库之外，还有`[updates]`，`[extras]`，`[centosplus]`等等，而这些库名大致对应于刚刚在centos8的`/etc/yum.repos.d/`下列出的配置文件名。所以，centos8这一版本其实只是把原先centos6的`CentOS-Base.repo`里面的软件库单拿出来成一个文件而已。

**那么我需要修改的是`CentOS-Media.repo`这个文件**，即yum安装的本地软件源的配置文件。vi打开它：

<div align="left"><img src="..\images\CentOS-Media配置文件.jpg" width = 600 height = 300/></div>

有两个库源`BaseOS`和`AppStream`。查看iso镜像文件里面可以发现也有这两个同名目录，并且这两个目录下面都有repodata目录，这就没错了，我要找到软件源就是这个。

回到`CentOS-Media`这个配置文件，可以看到两个库的baseurl字段后的目录都有三个，意思是说这三个目录都可以作为yum源，而enabled=0说明当前这个库不生效。进行如下修改：

```
[c8-media-BaseOS]
baseurl=file:///mnt/usb/BaseOS
#		 file:///media/cdrom/BaseOS
#		 file:///media/cdrecorder/BaseOS
enabled=1

[c8-media-AppStream]
baseurl=file:///mnt/usb/AppStream
#		 file:///media/cdrom/AppStream
#		 fiel:///media/cdrecorder/AppStream
enabled=1
```

只需修改上述的两个字段`baseurl`和`enabled`即可，baseurl修改一条目录，其余两条可以注释掉，也可以不注释。两个库的字段都要修改。但特别需要强调的是，linux的配置文件一定要严格按照原本的格式来修改，可能多一个空白符都会造成配置文件失效，因此注释时不要乱输入字符。

到这里为止，我发现，可以**直接把u盘挂载到`/media/CentOS`就可以了啊！baseurl不需要修改的，然后把enabled=0修改为enabled=1即可**。反正我个人原则是配置文件修改量能少则少。

### 安装管理工具

到目前为止，我只装了gdisk、net-tools、vim、xgamma。安装过程如下：

```
[root@localhost ~]# yum --disablerepo=\* --enablerepo=c8-media-BaseOS -y install gdisk
```

`--disablerepo=\*`指出让所有的软件库失效，而`--enablerepo=c8-media-BaseOS`指出选择使用`c8-media-BaseOS`这个库生效。前面已经提到了默认生效的是`CentOS-Base.repo`文件里的`base`库，因此这里需要指明我安装的软件要在`c8-media-BaseOS`这个库里找，否则直接`yum -y install gdisk`会报网络错误。这个用法是在`CentOS-Media.repo`里的注释看到的，有点疑惑的是为什么注释里指出使用c8-media呢？这个也不是库啊~不太清楚，反正我试了一下，没有用的，会报错找不到这个库。那么后面的就都同理了：

```
[root@localhost ~]# yum --disablerepo=\* --enablerepo=c8-media-BaseOS -y install net-tools
[root@localhost ~]# yum --disablerepo=\* --enablerepo=c8-media-AppStream -y install vim
[root@localhost ~]# yum --disablerepo=\* --enablerepo=c8-media-AppStream -y install xgamma
```

值得一提的是，centos8最小化安装时，默认预装了minimal版的vim，但尝试使用仍然会报command not found的错误，其实是vim包不全。在`BaseOS`这个库里只有vim-minimal的版本，剩下的完整版的包在`AppStream`里，包括vim-filesystem、vim-common、vim-enhanced。而xgamma是用于调节屏幕亮度的工具，装完实测无效，报错：xgramma: can't open display，所以我反手一个`yum remove xgamma`卸载。

这部分差不多就是这些内容了。