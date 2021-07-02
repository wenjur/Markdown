# pip安装matplotlib总是失败

在看python从入门到实践的项目二时需要安装matplotlib总是失败：

当前python版本是3.8，pip-20.2.2

#### 已尝试的方法：

首先是常用的安装方法，因为服务器在国外的缘故， 该方法经常因断网而安装失败

```
python -m pip install matplotlib
```

```
pip install matplotlib
```

在网上得知可以使用清华的pip镜像安装相应的包，使用-i选项，后面加上网址即可

```
python -m pip install matplotlib -i https://mirrors.tuna.tsinghua.edu.cn/pypi/simple
```

然而，依然是报错，找不到相应的matplotlib版本，可惜截图找不到了

最后将镜像地址更换成功安装

```
python -m pip install matplotlib -i https://pypi.tuna.tsinghua.edu.cn/simple
```

成功界面如下：

![image-20200910101149324](C:\Users\wenju\AppData\Roaming\Typora\typora-user-images\image-20200910101149324.png)

随后更新pip

```
python -m pip install --upgrade pip -i https://pypi.tuna.tsinghua.edu.cn/simple
```

然后是一堆报错

![image-20200910123822115](C:\Users\wenju\AppData\Roaming\Typora\typora-user-images\image-20200910123822115.png)

原因可能是网络问题，随后再次尝试该指令，更新成功