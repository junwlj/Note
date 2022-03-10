[toc]
# github 安装配置
### 1.github 安装
- `windows`下安装
  - 在Windows上使用Git，可以从Git官网直接下载安装程序，然后按默认选项安装即可。安装完成后，在开始菜单里找到“Git”->“Git Bash”，蹦出一个类似命令行窗口的东西，就说明Git安装成功！
- `Ubuntu` 安装教程
  - `sudo apt-get install git`
#### 2 安装SSH keys

<p style='color: red; font-size: 18'>在使用仓库之前，需要先安装SSH keys</p>

- 检查是否已井具有`ssh keys`

```bash
cd ~/.ssh
ls
```
![git_ssh1](../images/git/git_ssh1.png)
- 创建 `ssh keys`
```bash
ssh-keygen -t rsa -C "你自己的github对应的邮箱地址"

# 注1：“”是必须的
# 注2：在ssh目录下进行的
```
运行过程中会提示输入文件名，可直接按回车或者输入`id_rsa`,并提示你输入两次密码（该密码是你push文件的时候要输入的密码，而不是github管理者的密码），或者直接按回车，不输入密码。那么push的时候就不需要输入密码，直接提交到github上了。

运行过程如下：
![git_ssh2](../images/git/git_ssh2.png)

- 备份并移除已经存在的ssh keys(如果已存在ssh keys)

```bash
mkdir key_backup
cp id_rsa* key_backup
rm id_rsa*
# 即将已经存在的id_rsa，id_rsa.pub文件备份到key_backup文件夹
```


- 将刚刚创建的ssh keys添加到github中
  - 利用gedit/cat命令，查看id_rsa.pub的内容
..
- 检查SSH连接情况：
```bash
ssh -T git@github.com
```
    Hi 你的用户名! You’ve successfully authenticated, but GitHub does not provide shell access.

说明github上已有了SSH keys

**注1：之前在设置公钥时如果设置了密码，在该步骤会要求输入密码，那么，输入当时设置的密码即可；**

**注2：通过以上的设置之后，就能够通过SSH的方式，直接使用Git命令访问GitHub托管服务器了；**

**注3：若在服务器添加完公钥后报错。**
```bash
sign_and_send_pubkey: signing failed: agent refused operation
```
则需执行
```bash
eval "$(ssh-agent -s)"
ssh-add
```

### 3. 基础仓库配置
##### 3.1 *全局配置*：本机所有`git`仓库都使用该配置
```bash
$ git config --global user.name "Your Name"
$ git config --global user.email "email@example.com"
```
##### 3.2 *局部配置*：切换到仓库所在根目录，使得只有当前仓库使用该配置
```bash
$ git config user.name "Your name"
$ git config user.email "Your email"
```
配置完成之后可以通过`git config --list`查看所有的配置信息，或者只通过`git config user.name`查看当前用户信息，使用`git config user.email`查看邮箱信息
