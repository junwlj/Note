# Linux安装ssh并启动ssh服务

在Linux中我们利用`ftp`、`ssh`等服务，实现Linux与Linux之间、Linux与Windows之间的通信。

#### 1、安装`ssh`

```bash
# 注意：在Linux不同的发行版中ssh的安装命令不同
# 在redhat、Fedora、centos等发行版中安装命令为;
sudo yum install sshd
sudo yum install openssh-server

# 在Ubuntu、debian、mint等发行版中安装命令为;
sudo apt-get install sshd
sudo apt-get openssh-server
```

### 2、开启`ssh`服务

```bash
sudo service start sshd
```

### 3、检测`ssh`是否启动或者更改远程端口

```bash
ps -e | grep ssh
```

