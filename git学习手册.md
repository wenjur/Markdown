# git学习手册

### 基础命令

**`git config --global user.name 名字`**

**`git config --global user.email 邮箱`**

设置当前用户的信息

**`git init`**

初始化本地库

**`git status`**

查看当前git状态

**`git add 文件名`**

将工作区的文件添加到暂存区

**`-A`**：一次性add所有修改过的文件

**`.`**：添加新文件和编辑过的文件，不包含删除的文件。

**`-u`**：添加编辑或者删除的文件，不包括新添加的文件。

**`git rm --cached 文件名`**

将暂存区的文件删除

**`git commit -m "日志信息" 文件名`**

将暂存区的文件提交到本地库

**`-a`**：不许add操作，直接提交

**`git log`**

查看git详细版本信息

**`git reflog`**

查看git版本信息,第一个字段为版本标识号

**`git reset --hard 版本序列号`**

回滚版本，此时本地工作区文件也会随之发生改变。

### 分支操作

**`git branch 分支名`**

创建分支

**`git branch -v`**

查看分支

**`git checkout 分支名`**

切换分支

**`git merge 分支名`**

把给定的分支合并到当前分支上

### 远程仓库操作



**`git remote -V`**

查看当前远程链接的别名

**`-v`**：查看所有远程连接的别名。

**`git remote add 别名 远程地址`**

给远程链接创建别名

**`git push 别名/地址 分支`**

将本地库某一分支push到已有的远程仓库中

**`-u`**：将已经commit的代码push到远程仓库



**`git clone 远程地址`**

新建一目录，在该目录下使用bash将远程仓库完成地克隆到本地

1、拉取代码	2、初始化仓库	3、为克隆的仓库自动创建别名origin

**`git pull 远程仓库地址/仓库别名/ssh链接 远程分支名`**

将远程仓库代码同步到本地仓库



### SSH免密登录

**`ssh-keygen -t rsa -C 邮箱`**

在C盘家目录下使用bash执行该命令获取ssh公私钥，随后在github设置中添加公钥，此后即可使用仓库的ssh链接。

**`ssh -T git@github.com`**

登录github账户。

### 在IDEA中集成Git

**1、在家目录下配置.ignore文件**

```
# Compiled class file
*.class

# Log file
*.log

# BlueJ files
*.ctxt

# Mobile Tools for Java (J2ME)
.mtj.tmp/

# Package Files
*.jar
*.war
*.nar
*.ear
*.zip
*.tar.gz
*.rar

# virtual machine crash logs, see http://www.java.com/en/download/help/error_hotspot.xml
hs_err_pid*

.classpath
.project
.settings
target
.idea
*.iml
```

**2、在家目录下.gitconfig中引用.ignore**

```
[core]
	excludesfile = C:/Users/wenju/git.ignore
```

**3、在idea中配置Git**

先创建一个maven项目，File -> settings -> version control -> Git -> 右上添加Git安装路径，定位到.exe文件。

**4、将项目添加到Git控制下**

 菜单栏VCS -> Import Into Version Control -> Create Git repository

**5、add、commit**

直接在Project中文件或目录上右键选择Git - > *

**6、切换版本、切换分支、合并等**

左下角Git工具栏，打开后在中间会有日志提交记录，右键选择checkout revision即可切换版本

其他操作都在idea下方Git工具栏中。



### IDEA中的GitHub

**1、配置github账号到idea**

File -> settings -> version control ->Github -> + -> 按照要求验证即可

**2、将当前项目分享到github**

无需先创建repository后再push。方法：VCS -> Import into Version Control -> Share Project on Github

**3、推送代码到远程仓库**

1）右键模块名，Git -> Repository -> Push

2）VCS -> Git -> Push

这里值得注意的是，再动手改本地代码并Push前，一定要先Pull拉取远程库的最新代码。

**4、拉取远程库到本地库**

VCS -> Git -> Pull

**5、克隆代码到本地**

1、IDEA启动界面点击Get From Version Control

2、输入ssh链接或https链接，点击clone即可

### Git码云



### GitLab



