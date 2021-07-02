# 使用rm删除文件之后，空间被释放了吗？

在linux中，使用rm删除一个文件是否真的将其占用的空间资源释放了？

### 产生一个指定大小的文件

先查看一下/root挂载目录的大小：

```
$ df -h
/dev/sda11		454M	280M	147M	66%	/root
```

接下来在root下生成一个50M的文件

```
$ dd if=/dev/urandom of=/root/rest.txt bs=50M count=1
```

再来查看/root：

```
$ df -h
/dev/sda11		454M	312M	115M	74%	/root
```

不用关心到底多了多少，只需知道/root下的文件确实多了

测试程序：

```c
#include<stdio.h>
#include<unistd.h>
int main(void)
{
    FILE* fp = NULL;
    fp = fopen("/root/test.txt","rw+");
    if(NULL == fp) {
        perror("open file failed");
        return -1;
    }
    while(1) {
        sleep(1);
    }
    fclose(fp);
    return 0;
}
```

该程序打开一个文件，然后一直循环，执行sleep()

编译并运行：

```
$ gcc -o openFile openFile.c
$ ./openFile
```

此时打开另外一个窗口，删除test.txt：

```
$ rm /root/test.txt
```

查看一下/root：

```
$ df -h
dev/sda11		454M	312M	115M	74%	/root
```

可以发现空间大小并没有改变，明明使用rm把它删除了啊？

把openFile程序停掉，再查看/root：

```
$$ df -h
/dev/sda11		454M	280M	147M	66%	/root
```

可以看到空间立马就被释放了，也就是文件被删除了

### 一个文件什么时候会被删除？

Q：当一个文件的**引用计数**为零时（包括硬链接数），此时才会调用unlink删除

只要它不是零，就不会被删。删除文件，实际上是文件名到inode的链接删除，只要不重写如新的数据，磁盘上的block数据块不会被删除。当一个程序打开一个文件（获取到文件描述符），引用计数+1，rm实际上只是将引用计数-1，text.txt初始引用计数为1，故而没有被删除。

如下是inode的数据定义：

```c
struct inode {
    struct hlist_node i_hash;	//hash链表的指针
    struct list_head i_list;	//backing dev IO list
    struct list_head i_sb_list;	//超级块的inode链表
    struct list_head i_dentry;	//引用inode的目录项对象链表头
    unsigned long i_ino;		//索引节点号
    atomic_t i_count;			//引用计数
    unsigned int i_nlink		//硬链接数
}
```

### 如何释放已经被删除文件占用的空间？

其实前面已经说明了：重启打开该文件的进程即可。但是怎么知道那些文件已经被删除了，但还是被进程打开了呢？

Q：如下

```
$ lsof | grep deleted
```



