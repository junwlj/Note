[toc]
## <center>conda 常用命令</center>

#### 1. conda 安装
```Python
conda create -n enviroment Python=3.7
```
#### 2. conda 更新
升级Anaconda需要先升级conda
```bash
conda update conda          # 基本升级
conda update anaconda       # 大的升级
conda update anaconda-navigator    # update最新版本的 anaconda-navigator
```
#### 3. conda 基本命令
```bash
conda update -n base conda        # update最新版本的conda
conda create -n enviroment python=3.7   # 创建python3.7的虚拟环境
conda activate enviroment               # 激活并进入虚拟环境
conda deactivate                  # 关闭环境
conda env list                    # 显示所有的虚拟环境
conda info --envs                 # 显示所有的虚拟环境
```
#### 4. conda 虚拟环境的复制
```Python
# 克隆oldname环境为newname环境
conda create --name newname --clone oldname
```
#### 5. conda 虚拟环境的清理
```Python
# 清理conda环境
conda clean -p      / /删除没有用的包 检查哪些包没有在包缓存中被硬依赖到其他地方，并删除它们
conda clean -t      // 删除conda保存下来的tar包。
conda clean -y --all // 删除所有的安装包及cache
# 删除conda环境   --->>> 注：删除过程在 base环境中
conda remove --name enviroment --all   //创建xxxx虚拟环境
```

#### 6.查看指定包可安装版本信息命令
参考：https://blog.csdn.net/qq_35203425/article/details/79965389
查看tensorflow各个版本：（查看会发现有一大堆TensorFlow源，但是不能随便选，选择可以用查找命令定位）
`anaconda search -t conda tensorflow`
查看指定包可安装版本信息命令
`anaconda show <USER/PACKAGE>`
查看指定anaconda/tensorflow版本信息
`anaconda show tensorflow`
输出结果会提供一个下载地址，使用下面命令就可指定安装1.8.0版本tensorflow
`conda install --channel https://conda.anaconda.org/anaconda tensorflow=1.8.0`
更新，卸载安装包：
```Python
conda list         #查看已经安装的文件包
conda list  -n xxx       #指定查看xxx虚拟环境下安装的package
conda update xxx   #更新xxx文件包
conda uninstall xxx   #卸载xxx文件包
```
