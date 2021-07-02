# day13 修改grub2设置windows默认启动

***10-5***

事情是这样的，我的笔记本配了一个蓝牙键盘。在开机来到grub2的开机菜单界面时，经常因为没有来得及拿开蓝牙键盘选择启动项而默认进入了linux（我的蓝牙键盘放在笔记本键盘上面），平时windows用的多一些，这样就让人很困扰啊！

在网上查询了很多，几乎都是说centos6/7的，8代如何修改grub2的资料很少很少，即便找到一个，也很难描述清楚，所以自己才记录一下我是如何解决这个问题的。先来了解一下centos8相较于centos7有关grub2的配置文件的变化，这玩意是我困惑的主要原因：

### grub2配置文件的变化

从centos7开始，就已经默认使用grub2作为boot loader了，这里还是想吐槽几句：2018年我买这台电脑时，win10就已经使用uefi/gpt安装系统，现在都已经2020年了，在网上一搜系统安装，有关gpt分区的安装方法竟然少的可怜，绝大多数还是在逼扯bios/mbr。。。即便我看到几个，也没能解释明白uefi/gpt分区下安装linux到底该如何分区以及引导该如何处理的问题，有的作者甚至不知道自己是bios/mbr就稀里糊涂地安装好了，然后记录安装过程再往逼乎上面一贴。现在win10都强制gpt分区了，各大电脑厂商也都用的uefi引导的主板，用习惯了bios/mbr的那些老家伙们也该与时俱进一下了吧？行了，废话不多说了，等我弄明白了uefi/gpt这个家伙之后，再把前面的没填的坑--双系统安装详细记录一下。

**在centos8下**，与grub2相关的配置文件主要有：`/boot/efi/EFI/centos/grub.cfg`、`/boot/efi/EFI/centos/grubenv`、`/boot/loader/entries/`、`/etc/default/grub`、`/etc/grub.d/`，鸟哥介绍grub2时平台是centos7，其相关的配置文件和8有很大的出入。

#### /boot/efi/EFI/centos/grub.cfg

* **由指令grub2-mkconfig依据模板/etc/grub.d/和主环境配置文件/etc/default/grub执行自动生成的主配置文件**。
* 《鸟哥的linux私房菜》4th，第19章3.2小节介绍grub.cfg时这个配置文件位于/boot/grub2/grub.cfg。后续鸟哥查看该文件时通过输出的内容也能确定鸟哥测试机是gpt分区的，但是无法确定是否为uefi引导，而我自己电脑是uefi/gpt开机。我实机演示后发现在/boot/grub2下并没有grub.cfg这个配置文件，也没有i386-pc这个包含众多模块的目录，该目录下只有一个链接到/boot/efi/EFI/centos/grubenv的软链接文件。那么我自己的这个grub.cfg文件在哪里呢？答案是/boot/efi/EFI/centos/下。这个差异到底是因为centos8和7之间版本的差异，还是因为uefi引导需要的/boot/efi这个分区的缘故？现在我并没办法回答这个问题，也是我前面想吐槽的原因。还有一个变化是grub.cfg具体内容的变化，linux的两个启动项（正常启动和救援模式）不再有menuentry标识。

#### /boot/efi/EFI/centos/grubenv

* 这个文件里的saved_entry字段保存了当前默认启动项的设置，可设置为title、id、数字。很好，这正是我需要修改的地方。怎么修改呢？我的意思是，既然要修改为windows默认启动，**总得有什么值赋给saved_entry来标记windows启动项吧，这个值是什么？**自然就要找到window启动项的title、id或者数字了，接着往下看。

#### /boot/loader/entries/

* 这个目录是确确实实和centos7不一样的地方（centos7里没有这玩意），在我的电脑里，这下面有两个文件。分别对应着开机菜单界面的前两个选项（开机菜单界面看下面）。

#### /etc/default/grub

* grub2的主要环境配置文件，这里记载着一些grub2的功能设置，比如开机菜单等待多少秒？指定默认哪一个菜单来开机？是否显示开机菜单等等。诶等会儿，这个文件也可以设置开机菜单默认的选项？对啊没错的，这个也可以设置。那这个和前面的那个grubenv设置的有什么区别呢？先说结论好了：在这里设置默认菜单的话，要想使配置生效，还需使用grub2-mkconfig来重新生成grub.cfg文件，而如果修改的是grubenv的话，就只需修改而已，不需要别的什么就直接生效。由于grub.cfg这玩意可太重要了，我不敢乱改，所以我选择后者。前者需要执行grub2-mkconfig生效我知道为什么，至于后者为什么不需要我自己理解的也不是很透彻，等会儿展示时就说明一下。

#### /etc/grub.d/

* 这目录下放着为了生成grub.cfg的模板文件，不需要修改，和这次任务没什么关系。

### 详细思路

先说一下我的环境：装好双系统（二者都是uefi/gpt方式）之后grub2启动菜单就有windows manager了，也就是grub2已经在nvme磁盘识别出了windows的启动选项，亦即在grub.cfg里已经有了windows启动项的配置。

看完了鸟哥19章的讲述后，仍然有点迷，我该怎么修改默认启动项？迷的原因有两点：一是鸟哥grub2的配置文件和我的不一样，文件里的内容也有差异。二是鸟哥演示的windows和linux双系统是位于同一个磁盘并且为mbr分区。那我该怎么设置？

因为我看的是鸟哥的书，那就先跟随鸟哥的思路走：19.3.3节介绍/etc/default/grub的grub_default字段有4个设置值，分别是saved、数字、title、id。这个saved是什么？鸟哥说明了代表使用grub2-set-default来设置的值，通常默认为0，然而然而，这个0到底指什么？其实就是grub2启动菜单的那几个选项（编号从0开始）：

<div align="left"><img src="..\images\centos\grub2启动菜单.jpg" width = 600 height = 80 /></div>

现在的问题是，我的centos8这个grub.cfg里面只有一个windows的menuentry（鸟哥书里有三个menuentry包括两个linux的），由于这个差异，数字2是否真的能代表我电脑上面这个启动菜单的第三项（window boot...）还说不准呐，所以给saved_dafault赋一个数字值的方法暂时被我pass（我没有实机演示数字值是否有用）。在centos8的这个grub.cfg配置文件中，linux的menutry哪里去了？我的理解是centos8已经将这个menuentry的相关信息单独提出来作为配置文件来**动态加载**了，放在目录`/boot/loader/entries/`下。为什么这么说呢？看一下下图grub.cfg文件的这个注释：

<div align="left"><img src="..\images\centos\grub动态加载.jpg" width = 600 height = 200 /></div>

"The blscfg command parses the B....."这句指明blscfg会解析/boot/loader/entries这个目录下的BootLoaderSpec文件，然后将其部署到boot menu中，这意思很明显了，相对于centos7，8代把原本Linux两个启动项（包括一个救援模式）的配置信息单独提出来放在了/boot/loader/entries/下：

<div align="left"><img src="..\images\centos\entries目录下.jpg" width = 600 height = 130 /></div>

该目录下的这两个文件正对应linux的两个启动项。打开一看会发现，这里记载着id号和title呢！有没有注意到这个title和启动菜单的选项一摸一样？（如果把它修改了会怎样？）

<div align="left"><img src="..\images\centos\entries内容.jpg" width = 600 height = 130 /></div>

不过`/boot/loader/entries/`下只有linux的那两个启动项有记载，就是说只找到了linux两份启动项的id号，还是没有windows的啊。那windows的id号怎么找呢？继续查看grub.cfg的内容，在下图这个30_os-prober模板生成的配置中，仍保留了windows启动项的menuentry（我的windows在另一个磁盘）：

<div align="left"><img src="..\images\centos\centos8的grub_cfg.jpg" width = 600 height = 250 /></div>

这个menuentry后面接着的第一个字符串其实就是启动项windows的title啦，没错，title也可以设置为那个saved_default的值。既然找不到id，就使用title好了，有没有发现其实这个title和启动菜单windows的选项也是一样的？

接下来得说明一下为甚么设置这个`/boot/efi/EFI/centos/grubenv`里的saved_default是可行的：`/etc/default/grub`这个文件的grub_default字段其实才是正儿八经的决定默认启动项的设置，前面说过了，当此项设置为saved时用的值是指令grub2-set-default设置的值，而这个指令其实就是去修改`/boot/efi/EFI/centos/grubenv`里的saved_default字段，所以就是这样。可惜的是鸟哥并没有去解释他centos7里的那个grubenv文件。

总结一下就是：当`/etc/default/grub`里字段grub_default字段为saved，代表着这个值取`/boot/efi/EFI/centos/grubenv`里的saved_default值。这个值可以是启动项的title、id。而针对我的系统，我要修改windows为默认启动只需：

```
[root@localhost ~]# grub2-set-default "Windows Boot Manager (on /dev/nvme0n1p1)"
```

然后就可以了，开机进入启动菜单后，就会默认选择windows了。

前面说过，`/etc/default/grub`是环境配置文件，而`/boot/efi/EFI/centos/grub.cfg`这个主配置文件是开机时grub2会去加载分析的配置文件，并且grub.cfg是由指令grub2-mkconfig依据模板`/etc/grub.d/***`和`/etc/default/grub`里的参数而创建出来的，因此可以推断出：假如`/etc/default/grub`的grub_default字段取的不是saved，而是id或者数字，这时就需要grub2-mkconfig来重新生成grub.cfg文件使其生效。建议将原先的grub.cfg添加后缀成为`grub.cfg.bak`代表这个文件废弃，然后再grub2-mkconfig生成新的grub.cfg，这样出了问题还可以恢复。

### 总结

总结下来就是，有两种方法进行修改默认启动项：

* 将`/etc/default/grub`文件的grub_default字段修改为saved，然后使用`grub2-set-default  title/id/num`来修改即可，title的找寻方法前面已经说明。

* 直接修改`/etc/default/grub`文件的grub_default字段为`title/id/num`，然后使用`grub2-mkconfig`重新创建grub.cfg文件，创建方法在鸟哥书里19章已详细说明。

今天就到这里了。