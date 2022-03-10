# Ubuntu18 docker安装

> 参考： https://blog.csdn.net/u010889616/article/details/80170767
>
> Window7:  https://www.cnblogs.com/linjj/p/5606687.html
>
> Window10:  https://www.cnblogs.com/5bug/p/8506085.html		     

##  第一种方法从Ubuntu的仓库安装：

安装比较简单，这种安装的Docker不是最新版本，不过对于学习够用了，依次执行下面命令进行安装。

```
$ sudo apt install docker.io
```

    $ sudo systemctl start docker
    $ sudo systemctl enable docker

查看是否安装成功

    $ docker -v
    Docker version 17.12.1-ce, build 7390fc6

![1556262247848](C:\Users\Wlj_sky\AppData\Roaming\Typora\typora-user-images\1556262247848.png)

![1556261176680](C:\Users\Wlj_sky\AppData\Roaming\Typora\typora-user-images\1556261176680.png)

## 第二种方法从Docker仓库安装

这种安装方式首先要保证Ubuntu服务器能够访问Docker仓库地址：https://download.docker.com/linux/ubuntu，如果能够访问，按照下面的操作步骤进行安装。

    $ sudo apt update
    $ sudo apt install apt-transport-https ca-certificates curl software-properties-common

在/etc/apt/sources.list.d/docker.list文件中添加下面内容

deb [arch=amd64] https://download.docker.com/linux/ubuntu bionic stable

添加秘钥

$ curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -

安装docker-ce

$ sudo apt install docker-ce

查看是否安装成功：

    $ docker --version
    Docker version 18.03.0-ce, build 0520e24


## 配置镜像加速

安装 python-pip

安装 docker-compose

创建 /etc/docker/daemon.json 文件, 内容:

```
{
  "registry-mirrors": ["https://y4tay211.mirror.aliyuncs.com"]
}
```

$ sudo systemctl daemon-reload

$ sudo systemctl restart docker          



## 配置docker 私有仓库的安全地址

创建 /etc/docker/daemon.json 文件, 内容:

```
{
		"registry-mirrors": ["https://y4tay211.mirror.aliyuncs.com"],
		"insecure-registries": [ "10.35.166.143:5000"]  
}
```

![1556262404022](C:\Users\Wlj_sky\AppData\Roaming\Typora\typora-user-images\1556262404022.png)

![1556262488316](C:\Users\Wlj_sky\AppData\Roaming\Typora\typora-user-images\1556262488316.png)



