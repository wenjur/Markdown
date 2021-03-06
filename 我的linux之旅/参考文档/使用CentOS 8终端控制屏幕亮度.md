# 使用CentOS 8终端控制屏幕亮度

在GUI模式下很容易控制CentOS屏幕的亮度。但是，如果您使用的是命令行系统，并且想从终端控制显示器的亮度，则需要了解一些用于在这种情况下控制显示器亮度的命令行工具。有。经过研究，我发现了一个名为“ xrandr”的命令行工具。使用此工具，您可以轻松调整屏幕的亮度。您可以使用“ xrandr”实用程序来调整指定屏幕的大小和方向。

本文将向您展示如何使用CentOS 8中预装的xrandr实用程序来调整屏幕亮度。

要调整屏幕亮度，您需要执行以下操作：



步骤1：您可以使用快捷键Ctrl + Alt + t打开终端窗口，也可以使用应用程序启动器搜索栏将其打开，如下所示：

## 步骤2：显示系统的当前状态

要检查显示系统的当前状态，屏幕分辨率和大小，请在终端中键入以下命令。

```
$ xrandr -q
```

在终端中看起来像这样：

```

```

从上图可以看到，当前连接的屏幕是“ XWAYLAND 0”。运行以上命令还将在终端窗口中显示当前分辨率和屏幕尺寸。

## 仅打印活动显示名称

![使用CentOS 8终端控制屏幕亮度](C:\Users\wenju\Documents\Markdown\images\1588679774.png)

要仅显示当前活动的显示名称，请使用“ xrandr”，“ grep”和“ head”命令，如下所示：

## 步骤3：设定萤幕亮度

您可以使用以下语法设置显示器的亮度：

$ xrandr –输出 [monitor-name] -亮度 [brightness-level]

亮度值限制为0到1，其中0代表完美的黑色，而1代表最亮的值。

例如，如果要设置屏幕亮度，请描述显示屏幕名称“ XWAYLAND0”，并将其值设置为0.75。使用以下命令设置显示屏的亮度。

```
$ xrandr --output XWAYLAND0 --brightness 0.75
```

![使用CentOS 8终端控制屏幕亮度](C:\Users\wenju\Documents\Markdown\images\1588679776.png)