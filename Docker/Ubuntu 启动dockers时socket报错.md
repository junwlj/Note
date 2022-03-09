# Ubuntu20.04使用docker info报错`ERROR: Got permission denied while trying to connect to the Docker daemon socket at unix:///var/run/docker.sock: Get "http://%2Fvar%2Frun%2Fdocker.sock/v1.24/info": dial unix /var/run/docker.sock: connect: permission denied errors pretty printing info

### 1 问题描述

在终端执行`docker info`命令，出现如下报错：

```shell
jundocker@jun-smi:~$ docker info
Client:
 Context:    default
 Debug Mode: false
 Plugins:
  app: Docker App (Docker Inc., v0.9.1-beta3)
  buildx: Build with BuildKit (Docker Inc., v0.6.1-docker)
  scan: Docker Scan (Docker Inc., v0.8.0)

Server:
ERROR: Got permission denied while trying to connect to the Docker daemon socket at unix:///var/run/docker.sock: Get "http://%2Fvar%2Frun%2Fdocker.sock/v1.24/info": dial unix /var/run/docker.sock: connect: permission denied
errors pretty printing info
```



### 2 原因分析

来自docker mannual：

> Manage Docker as a non-root user
>
> The docker daemon binds to a Unix socket instead of a TCP port. By default that Unix socket is owned by the user root and other users can only access it using sudo. The docker daemon always runs as the root user.
>
> If you don’t want to use sudo when you use the docker command, create a Unix group called docker and add users to it. When the docker daemon starts, it makes the ownership of the Unix socket read/writable by the docker group.

 docker进程使用Unix Socket而不是TCP端口。而默认情况下，Unix socket属于root用户，因此需要**root权限**才能访问。

### 3 解决方法

```shell
sudo groupadd docker #添加docker用户组

sudo gpasswd -a $XXX docker #检测当前用户是否已经在docker用户组中，其中XXX为用户名，例如我的，liangll

sudo gpasswd -a $USER docker #将当前用户添加至docker用户组

newgrp docker #更新docker用户组
```

​		例如：

```shell
jundocker@jun-smi:~$ sudo groupadd docker
[sudo] password for jundocker: 
groupadd: group 'docker' already exists
jundocker@jun-smi:~$ sudo gpasswd -a $jundocker docker
gpasswd: user 'docker' does not exist
jundocker@jun-smi:~$ sudo gpasswd -a $USER docker
Adding user jundocker to group docker
jundocker@jun-smi:~$ newgrp docker
```

### 4 检查是否更新成功

再次执行"docker version"命令，发现不再出现"Got permission denied"权限报错

```shell
undocker@jun-smi:~$ docker version
Client: Docker Engine - Community
 Version:           20.10.8
 API version:       1.41
 Go version:        go1.16.6
 Git commit:        3967b7d
 Built:             Fri Jul 30 19:54:27 2021
 OS/Arch:           linux/amd64
 Context:           default
 Experimental:      true

Server: Docker Engine - Community
 Engine:
  Version:          20.10.8
  API version:      1.41 (minimum version 1.12)
  Go version:       go1.16.6
  Git commit:       75249d8
  Built:            Fri Jul 30 19:52:33 2021
  OS/Arch:          linux/amd64
  Experimental:     false
 containerd:
  Version:          1.4.9
  GitCommit:        e25210fe30a0a703442421b0f60afac609f950a3
 runc:
  Version:          1.0.1
  GitCommit:        v1.0.1-0-g4144b63
 docker-init:
  Version:          0.19.0
  GitCommit:        de40ad0
```

