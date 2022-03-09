# Linux 文件权限操作

### 1. Linux文件属性

```shell
xxx@xxxx:~$ ll
total 2
drwxrwxr-x 20 lijun lijun   4096 9月  11 20:17 ./
drwxrwxr-x 15 lijun lijun   4096 9月  11 19:19 ../
drwxrwxr-x 11 lijun lijun   4096 9月  11 19:52 3rdparty/
drwxrwxr-x  8 lijun lijun   4096 9月  11 19:53 apps/

```

- 当为[ *d* ]则是目录

- 当为[ *-* ]则是文件；

- 若是[ *l* ]则表示为链接文档(link file)；

- 若是[ *b* ]则表示为装置文件里面的可供储存的接口设备(可随机存取装置)；

- 若是[ *c* ]则表示为装置文件里面的串行端口设备，例如键盘、鼠标(一次性读取装置)。

  ![](/home/lijun/Desktop/Note/Linux/images/flameshot_2.png)

- 第0位确定文件类型

- 第1-3位确定属主（该文件的所有者）拥有该文件的权限。

- 第4-6位确定属组（所有者的同组用户）拥有该文件的权限

-  第7-9位确定其他用户拥有该文件的权限

- **第1、4、7位表示读权限**

- **第2、5、8位表示写权限**

- **第3、6、9位表示可执行权限**

###  2. chmod：更改文件9个属性 更改文件 只读 只写 可执行的属性

​		Linux文件属性有两种设置方法，一种是数字，一种是符号。Linux文件的基本权限就有九个，分别是owner/group/others三种身份各有自己的read/write/execute权限。读写执行权限

#### 2.1 第一种案例:数字更改

​		数字来代表各个权限，各权限的分数对照表如下：

- **r:4**

- **w:2**

- **x:1**

  每种身份(owner/group/others)各自的三个权限(r/w/x)分数是需要累加的，例如当权限为： [-rwxrwx---] 分数则是：

- owner = rwx = 4+2+1 = 7

- group = rwx = 4+2+1 = 7

- others= --- = 0+0+0 = 0

  设定权限的变更时，该文件的权限数字就是770啦！变更权限的指令chmod的语法是这样的：

```
 chmod [-R] xyz 文件或目录
```

​	选项与参数：

- xyz : 就是刚刚提到的数字类型的权限属性，为 rwx 属性数值的相加。

- -R : 进行递归(recursive)的持续变更，亦即连同次目录下的所有文件都会变更

  举例来说，如果要将.bashrc这个文件所有的权限都设定启用，那么命令如下：

```
改下m.c 文件的属性 为 rwx rwx ---  意思是 主用户权限是 rwx=7 root用户是 rwx=7 其他用户 无权限
```

```
daokr@daokr-sys:/home/test/tt$ ls -l
总用量 4
-rw-r--r-- 1 daokr daokr 26 3月  27 14:45 m.c
daokr@daokr-sys:/home/test/tt$ chmod 770 m.c 
daokr@daokr-sys:/home/test/tt$ ls -l
总用量 4
-rwxrwx--- 1 daokr daokr 26 3月  27 14:45 m.c
```

```
daokr@daokr-sys:/home/test/tt$ sudo chown daokr:root m.c
[sudo] daokr 的密码： 
daokr@daokr-sys:/home/test/tt$ ls -l
总用量 4
-r--rwx--- 1 daokr root 26 3月  27 14:45 m.c
daokr@daokr-sys:/home/test/tt$ vim m.c 
daokr@daokr-sys:/home/test/tt$ chmod 440 m.c
```

​		上面的 把 m.c 修改了权限组root 和 文件属性 440;说明 daokr用户和root用户都无法修改该文件;因为是只读文件

```
第二种案例:通过字符更改
基本上就九个权限分别是(1)user (2)group (3)others三种身份啦！
 那么我们就可以藉由u, g, o来代表三种身份的权限！
```

此外， a 则代表 all 亦即全部的身份！那么读写的权限就可以写成r, w, x！也就是可以使用底下的方式来看： 

| chmod | u g o a | +(加入) -(除去) =(设定) | r w x | 文件或目录 |
| ----- | ------- | ----------------------- | ----- | ---------- |
|       |         |                         |       |            |

如果我们需要将文件权限设置为 **-rwxr-xr--** ，可以使用 chmod u=rwx,g=rx,o=r 文件名 来设定:

```
daokr@daokr-sys:/home/test/tt$ chmod u=rwx,g=rwx,o=- m.c
daokr@daokr-sys:/home/test/tt$ ls -l
总用量 4
-rwxrwx--- 1 daokr root 26 3月  27 14:45 m.c
daokr@daokr-sys:/home/test/tt$ chmod u=r-x,g=-wx,o=- m.c
daokr@daokr-sys:/home/test/tt$ ls -l
总用量 4
-r-------- 1 daokr root 32 3月  27 15:35 m.c
daokr@daokr-sys:/home/test/tt$ chmod u=r-x,g=r,o=- m.c
daokr@daokr-sys:/home/test/tt$ ls -l
总用量 4
-r--r----- 1 daokr root 32 3月  27 15:35 m.c
daokr@daokr-sys:/home/test/tt$ chmod u=r-x,g=rw,o=- m.c
daokr@daokr-sys:/home/test/tt$ ls -l
总用量 4
-r--rw---- 1 daokr root 32 3月  27 15:35 m.c
```

而如果是要将权限去掉而不改变其他已存在的权限呢？例如要拿掉全部人的可执行权限，则：

```
#  chmod  a-x test1
# ls -al test1
-rw-r--r-- 1 root root 0 Nov 15 10:32 test1然后
```

daokr@daokr-sys:/home/test/tt$ chmod a-x m.c
daokr@daokr-sys:/home/test/tt$ ls -l
总用量 4
-r--rw---- 1 daokr root 32 3月 27 15:35 m.c
daokr@daokr-sys:/home/test/tt$ chmod u=rwx,g=rwx,o=r m.c
daokr@daokr-sys:/home/test/tt$ ls -l
总用量 4
-rwxrwxr-- 1 daokr root 32 3月 27 15:35 m.c

```
利用 加号和减号 添加和删除权限
分别u，g，o用户进行加减操作权限
daokr@DK:~$ chmod o-x a.out
daokr@DK:~$ chmod o+x a.out
daokr@DK:~$ chmod u-x a.out
daokr@DK:~$ chmod g-x a.out

```

设置权限当前用户有读写执行；用户组；读写执行；other用户只有可执行权限

```
chmod u=rwx,g=rwx,o=x myfile
```

```
4.对目录dir默认系统设置umaks

umask 022设置当前系统 新建的目录默认权限是022 就是775
参数-S 表示以源码查看
```

```
daokr@DK:~$ umask -S
u=rwx,g=rwx,o=rx
daokr@DK:~$ umask 022
daokr@DK:~$ umask -S
u=rwx,g=rx,o=rx
```

