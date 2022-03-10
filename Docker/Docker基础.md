# mdocker 基础操做

### 1、docker镜像的拉取

```dockerfile
docker pull  【镜像名称】
```



### 2、docker容器创建
；‘’
````dockerfile
### 注意在创建过程中-p在前，--name在后
docker run -it -d -p ****:**** --name=【容器名称】 Image id
````



### 3、docker容器端口的映射



### 4、docker容器与Windows、Linux之间的文件传输

```bash
docker cp Windows的文件路径 容器名称：容器路径
```

还没有安装Docker的小伙伴点这里《Docker教程（一）Centos Docker安装》，话不多说，一起来操作吧

容器
启动并创建

-d: 放入后台运行
-p: 指定端口映射关系（第一个为本地端口、第二个为容器端口）
docker run -d -p 3306:3306 mysql
或


1.下载镜像
创建一个docker文件夹，进入目录后：

docker pull centos


列出所有镜像

docker images


2.开始制作镜像


如果已经配置完成进入方式：

docker ps #查看
docker exec -it a1b64cfc23e3 /bin/bash #进入




↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓反之执行下面↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓

启动并进入

docker run -it --name docker1 centos /bin/bash


》》》》》》》》》》》》》》》》》》》》》》》》》》》》》》》》》》》》》》》》》》》》》》》》》》》》》》》》》》》》

如果报错/usr/bin/docker-current: Error parsing reference: "/bin/bash" is not a valid repository/tag: invalid

docker ps -a

#启动
docker start 8e96ca929731
这些都是我们之前创建的容器,要使用就不需要再创建,直接启动就行了

docker start 容器id

然后通过查询docker启动的进程 命令:

docker ps

这个时候 我们容器已经启动了



如果是要更新容器可以使用命令删除原有name:
通过删除命令:

docker rm 容器id
《《《《《《《《《《《《《《《《《《《《《《《《《《《《《《《《《《《《《《《《《《《《《《《《《《《《《《《《《《《《

接下来开始定制化，和我们正常操作centos一样



3.镜像/容器删除
docker容器删除
#查看所有运行容器
docker ps

#显示所有容器包括未运行的 -q静默模式 只显示容器编号
docker ps -a -q

#停止所有的container
docker stop $(docker ps -a -q)

#删除所有的container
docker rm $(docker ps -a -q)

#停止容器
docker stop 容器ID

#启动容器
docker start

#重启容器
docker restart

#删除运行中的容器
docker rm -f 容器ID

#删除未运行的容器
docker rm 容器ID


docker镜像删除
#查看镜像
docker images

#删除全部镜像
docker rmi $(docker images -q)

#根据ID强制删除，删除镜像后，所创的容器也一并删除
docker rmi -f 镜像ID

4.端口映射
#将主机的8081端口映射到容器的8080端口
#java_docker（容器名称）
#uncleliu/java_docker（镜像REPOSITORY）
docker run -it -d --name java_docker -p 8081:8080 uncleliu/java_docker /bin/bash






借鉴

https://www.cnblogs.com/murry/p/9332747.html



https://www.cnblogs.com/kevingrace/p/9453987.html
------------------------------------------------
版权声明：本文为CSDN博主「绿林寻猫」的原创文章，遵循CC 4.0 BY-SA版权协议，转载请附上原文出处链接及本声明。
原文链接：https://blog.csdn.net/qq_40369944/article/details/109516590
