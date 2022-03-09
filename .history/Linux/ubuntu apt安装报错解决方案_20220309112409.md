#  `ubuntu`  安装软件包过程中 `Could not get lock /var/lib/dpkg/lock-frontend`解决方案

 	  `Ubuntu`在安装软件包过程出现`Could not get lock /var/lib/dpkg/lock-frontend`，表明在安装之前使用**apt**出现异常，没用正常关闭进程，此进程还在运行。


​		解决方案：利用`ps`和`grep`查找*apt*的**pid**，并使用**kill**杀死进程。

```shell
ps -afx  # 找到正在执行的 apt 的进程
kill -9 16214
```

```shell
XXX@XXXXXX:~$  ps afx|grep apt
16214 ?        S      0:00  \_ sudo apt install rpm
16215 ?        S      0:01  |   \_ apt install rpm
16721 pts/3    S+     0:00          \_ grep --color=auto apt
XXX@XXXXXX:~$ kill 16214
bash: kill: (16214) - Operation not permitted
XXX@XXXXXX:~$ sudo kill 16214
XXX@XXXXXX:~$ sudo kill 16215
```

​		再重新执行安装命令。如果再次报错，则通过手动删除`/var/lib/dpkg/lock`