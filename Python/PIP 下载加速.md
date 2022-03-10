#### PIP 下载加速

Python中使用PIP下载加速的方式主要有2中，一种是通过在`pip install package`的命令后面添加国内源地址，提升`package`的下载速度，另一种使通过修改`pip`的配置文件，在配置文件中声明国内下载源地址，提高`pip`的下载速度。
1. 通过`pip install package`命令后添加国内下载源地址
```python
# 利用清华源加速下载
pip install package -i https://pypi.tuna.tsinghua.edu.cn/simple
# 使用阿里源加速加载
pip install package -i http://mirrors.aliyun.com/pypi/simple/ --trusted-host mirrors.aliyun.com
```
2.修改`pip`配置文件，利用阿里云源进行加速
- Windows下`PIP`配置文件修改：
```Python
# 修改C:\Users\M\AppData\Roaming\pip\pip.ini中的内容如下
[global]
index-url=http://mirrors.aliyun.com/pypi/simple/

[install]
trusted-host=mirrors.aliyun.com
```
- Linux下`pip`配置文件修改：
```Python
# 在Linux中创建~/.pip/pip.ini文件，并在文件中添加如下内容
[global]
index-url=http://mirrors.aliyun.com/pypi/simple/

[install]
trusted-host=mirrors.aliyun.com
```
- 直接在Windows或者Linux的命令行中利用如下命令设置与删除国内源
```Python
pip config set global.index-url https://mirrors.ustc.edu.cn/pypi/web/simple
#添加全局使用该数据源
pip config set global.trusted-host https://mirrors.ustc.edu.cn/pypi/web/simple

# 删除
pip config unset key
# 例如
conda config --remove channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/free/
```
- 在添加后通过如下命令显示当前下载源
```Python
#显示目前pip的数据源有哪些
pip config list
pip config list --[user|global] # 列出用户|全局的设置
pip config get global.index-url # 得到这key对应的value 如：https://mirrors.aliyun.com/pypi/simple/
```
