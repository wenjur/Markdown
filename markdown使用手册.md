1.标题
代码

注：# 后面保持空格

# h1
## h2
### h3
#### h4
##### h5
###### h6
####### h7      // 错误代码
######## h8     // 错误代码
######### h9    // 错误代码
########## h10  // 错误代码
2.分级标题
代码
注：= - 最少可以只写一个，兼容性一般

一级标题
======================
二级标题
---------------------
3.TOC
注：根据标题生成目录，兼容性一般

代码

[TOC]
4.引用
代码1(单行式)

> hello world!
> 代码2(多行式)

> hello world!
> hello world!
> hello world!
> 代码3(多层嵌套)

> aaaaaaaaa
> > bbbbbbbbb
> > > cccccccccc
> > > 5.行内标记
> > > 注：用 ` 标记代码块将变成一行

代码

标记之外`hello world`标记之外
错误代码注：不同平台错误略有差异

 标记之外 ` 
< div>   
    < div>div>
    < div>div>
    < div>div>
< /div>
`标记之外
6.代码块
注：与上行距离 - 空行

代码1(```)

注：用```生成块

```
<div>   
<div>div>
<div>div>
<div>div>
div>
```
演示

<div>   
<div>div>
<div>div>
<div>div>
div>
代码2(Tab)

注：Tab缩进

我是文字……

<div>   
<div>div>
<div>div>
<div>div>
div>
代码3(自定义语法)注：根据不同的语言配置不同的代码着色

```javascript
var num = 0;
for (var i = 0; i < 5; i++) {
    num+=i;
}
console.log(num);
```
演示

var num = 0;
for (var i = 0; i < 5; i++) {
    num+=i;
}
console.log(num);
7.插入链接
代码1(内链式)
注：{:target="_blank"}跳转方式兼容性一般 ，多数第三方平台不支持跳转

[百度1](http://www.baidu.com/" 百度一下"){:target="_blank"}   
代码2(引用式)

[百度2][2]{:target="_blank"}
[2]: http://www.baidu.com/   "百度二下"
8.插入图片
代码1(内链式)

[图片上传失败...(image-90880b-1542510791300)]
代码2(引用式)

![name][01]
[01]: ./01.png "描述"
9.插入图片带有链接
代码

[[图片(URL)]](http://www.baidu.com){:target="_blank"}       // 内链式

[[图片上传失败...(URL)]][5]{:target="_blank"}                      // 引用式

[5]: http://www.baidu.com
10.视频插入
代码1

注：多数第三方平台不支持插入视频

<iframe height=498 width=510 src='http://player.youku.com/embed/XMjgzNzM0NTYxNg==' frameborder=0 'allowfullscreen'>iframe>

代码2

[[图片](url)](http://v.youku.com/v_show/id_XMjgzNzM0NTYxNg==.html?spm=a2htv.20009910.contentHolderUnit2.A&from=y1.3-tv-grid-1007-9910.86804.1-2#paction){:target="_blank"}
11.序表
代码1(有序)

注：序列.后 保持空格

1. one
2. two
3. three
代码2(无序)

* one
* two
* three
代码3(序表嵌套)

1. one
    1. one-1
    2. two-2
2. two 
    * two-1
    * two-2
    代码4(序表嵌套代码块)注：换行+两个Tab

* one

var a = 10;     // 与上行保持空行并 递进缩进
12.任务列表
注：兼容性一般 要隔开一行

代码

这是文字……

- [x] 选项一
- [ ] 选项二  
- [ ]  [选项3]
13.表格
注： : 代表对齐方式 ,** : 与 | 之间不要有空格**，否则对齐会有些不兼容

代码1

|     a     | b               |            c |
| :-------: | :-------------- | -----------: |
|   居中    | 左对齐          |       右对齐 |
| ========= | =============== | ============ |
代码2(简约写法)

|      a       | b                 |             c |
| :----------: | :---------------- | ------------: |
|     居中     | 左对齐            |        右对齐 |
| ============ | ================= | ============= |
代码3(特殊表格)

注：一般对合并单元格，以及其他特殊格式表格，markdown 是无能为力的
所以常规的做法是使用HTML标签，但是这样的编写效率极低。
但是有了这款工具的话，所有问题都迎刃而解。

在线生成HTML代码 Tables Generator (国外的站)



15.支持内嵌CSS样式
代码

<p style="color: #AD5D0F;font-size: 30px; font-family: '宋体';">内联样式p>
16.语义标记
描述	效果	代码
斜体	斜体	*斜体*
斜体	斜体	_斜体_
加粗	加粗	**加粗**
加粗+斜体	加粗+斜体	***加粗+斜体***
加粗+斜体	加粗+斜体	**_加粗+斜体_**
删除线	删除线	~~删除线~~
17.语义标签
描述	效果	代码
斜体	斜体	斜体
加粗	加粗	加粗
强调	强调	强调
上标	Za	Za
下标	Za	Za
键盘文本	


Ctrl
换行		
18.格式化文本
保持输入排版格式不变
注：对内含标签需要破坏结构才能显示

代码

<pre>
hello world 
         hi
  hello world 
pre>
错误解决方法注：标签内部添加空格 或者 直接使用```标记来处理代码1(插入空格)

<pre>
    < div>   
        < div>< /div>
        < div>< /div>
        < div>< /div>
    < /div>
pre>
代码2( ``` 代码块标记)

```
<div>   
<div>div>
<div>div>
<div>div>
div>
```
19.公式 {#1}
注：1个$左对齐，2个居中

代码

$$ x \href{why-equal.html}{=} y^2 + 1 $$
$ x = {-b \pm \sqrt{b^2-4ac} \over 2a}. $
20.分隔符
注：最少三个 --- 或 ***或 * * *

代码

***
---
* * *
21.脚注
代码

Markdown[^1]
[^1]: Markdown是一种纯文本标记语言        // 在文章最后面显示脚注
22.锚点
代码
注：只有标题支持锚点， 跳转目录方括号后 保持空格

[公式标题锚点](#1)

### [需要跳转的目录] {#1}    // 方括号后保持空格
23.定义型列表
注：解释型定义代码

Markdown 
:   轻量级文本标记语言，可以转换成html，pdf等格式  //  开头一个`:` + `Tab` 或 四个空格

代码块定义
:   代码块定义……

var a = 10;         // 保持空一行与 递进缩进
24.自动邮箱链接
代码

25.流程图
代码1

```flow                     // 流程
st=>start: 开始|past:> http://www.baidu.com // 开始
e=>end: 结束              // 结束
c1=>condition: 条件1:>http://www.baidu.com[_parent]   // 判断条件
c2=>condition: 条件2      // 判断条件
c3=>condition: 条件3      // 判断条件
io=>inputoutput: 输出     // 输出
//----------------以上为定义参数-------------------------

//----------------以下为连接参数-------------------------
// 开始->判断条件1为no->判断条件2为no->判断条件3为no->输出->结束
st->c1(yes,right)->c2(yes,right)->c3(yes,right)->io->e
c1(no)->e                   // 条件1不满足->结束
c2(no)->e                   // 条件2不满足->结束
c3(no)->e                   // 条件3不满足->结束
```
演示



代码详解
流程图分为两个部分： 定义参数 然后 连接参数

定义示例：

tag=>type: content:>url         // 形参格式 
st=>start: 开始:>http://www.baidu.com[blank]  //实参格式
注：** st=>start: 开始 的：后面保持空格**

形参	实参	含义
tag	st	标签 (可以自定义)
=>	=>	赋值
type	start	类型  (6种类型)
content	开始	描述内容 (可以自定义)
:>url	http://www.baidu.com[blank]	链接与跳转方式 兼容性很差
6种类型	含义
start	启动
end	结束
operation	程序
subroutine	子程序
condition	条件
inputoutput	输出
连接示例：

st->c1(yes,right)->c2(yes,right)->c3(yes,right)->io->e
开始->判断条件1为no->判断条件2为no->判断条件3为no->输出->结束
形参	实参	含义
->	->	连接
condition	c1	条件
(布尔值,方向)	(yes,right)	如果满足向右连接，4种方向：right ，left，up ，down 默认为：down
注：operation (程序); subroutine (子程序) ;condition (条件)，都可以在括号里加入连接方向。

operation(right) 
subroutine(left)
condition(yes,right)    // 只有条件 才能加布尔值
代码2

注：添加样式和url跳转 需要添加第三方的脚本
实际效果很差，使用起来麻烦，意义不大

```flow                             // 流程
st=>start: 启动|past:>http://www.baidu.com[blank] // 开始
e=>end: 结束                      // 结束
op1=>operation: 方案一             // 运算1
sub2=>subroutine: 方案二|approved:>http://www.baidu.com[_parent]  // 运算2
sub3=>subroutine: 重新制定方案        // 运算2
cond1=>condition: 行不行？|request  // 判断条件1
cond2=>condition: 行不行？// 判断条件2
io=>inputoutput: 结果满意           // 输出

// 开始->方案1->判断条件->
st->op1->cond1
// 判断条件1为no->方案2->判断条件2为no->重新制定方案->方案1
cond1(no,right)->sub2->cond2(no,right)->sub3(right)->op1
cond1(yes)->io->e       // 判断条件满足->输出->结束
cond2(yes)->io->e       // 判断条件满足->输出->结束
```
演示



26.时序图
代码1

```sequence
A->>B: 你好
Note left of A: 我在左边     // 注释方向，只有左右，没有上下
Note right of B: 我在右边
B-->A: 很高兴认识你
```
演示



代码详解

注：A->>B: 你好 后面可以不写文字，但是一定要在最后加上：
Note left of A 代表注释在A的左边

符号	含义
-	实线
>		实心箭头
>	--	虚线
>	>		空心箭头
>	>
>	>	代码2

    ```sequence
    起床->吃饭: 稀饭油条
    吃饭->上班: 不要迟到了
    上班->午餐: 吃撑了
    上班->下班:
    Note right of 下班: 下班了
    下班->回家:
    Note right of 回家: 到家了
    回家-->>起床:
    Note left of 起床: 新的一天
    ```
演示

————————————————
版权声明：本文为CSDN博主「深孵小秘书-红小白」的原创文章，遵循CC 4.0 BY-SA版权协议，转载请附上原文出处链接及本声明。
原文链接：https://blog.csdn.net/weixin_32816563/article/details/112535469