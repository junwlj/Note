### 微星PE62 7RD ubuntu16.04 中tensorflow-gpu环境搭建

#### 一、显卡安装：

​	1、使用以下命令查看自己gpu型号

```
lspci | grep -i nvidia
这是我的gpu信息
01:00.0 3D controller: NVIDIA Corporation GP107M [GeForce GTX 1050 Mobile] (rev a1)
```

​	2、验证自己的Linux版本

```
uname -m && cat /etc/*release
```

​	3、安装显卡之前的操作：

​	先卸载本机原有的驱动：

```
sudo apt remove --purge nvidia*
或者使用
sudo /usr/bin/nvidia-uninstall
```

​	a、禁用nouveau驱动

```
sudo ls | grep nouveau
```

​	执行上述命令若无输出则表示禁用成功。

​	然后执行下面命令，修改blacklist.conf文件

```
sudo chmod 666 /etc/modprobe.d/blacklist.conf
sudo vi /etc/modprobe.d/blacklist.conf
```

​	在里面写入：

```
blacklist vga16fb
blacklist nouveau
blacklist lbm-nouveau
options nouveau modeset=0
alias nouveau off
alias lbm-nouveau of
```

```
sudo chmod 644 /etc/modprobe.d/blacklist.conf
将修改的权限修改回来
sudo update-initramfs -u （更新内核）
apt-get install linux-source # 获取kernel source
```

​	b、安装显卡驱动：

​	在Ubuntu中安装显卡方式有3种方式安装，这儿我说其中两种安装。

​	1.使用.run文件安装

​	进入到你驱动下载的文件夹中，在终端中输入

```
sudo chmod +x NVIDIA-Linux-x86_64-418.43.run （给驱动文件夹执行权限）
```

​	对于这种方式安装显卡驱动，不能够直接运行sudo ./NVIDIA-Linux-x86_64-418.43.run ,有可能会出现安装之后电脑无限重启！所以需要在上述命令后面添加上参数，我们可以通过sudo NVIDIA-Linux-x86_64-418.43.run -A该命令来查看参数设置，然后使用以下命令进行安装（此处的参数有可能不同的电脑有更改，我的只为参照。

```
sudo ./NVIDIA-Linux-x86_64-418.43.run --no-opengl-files --no-x-check  --no-nouveau-check
```

​	之后在弹出的页面中选择yes直接回车，在接下来的操作中等待安装完成，完成之后重启电脑。

```
reboot
```

​	重启之后使用nvidia-smi命令查看显卡驱动的详细信息

```
nvidia-smi
Tue Mar 19 14:01:39 2019       
+-----------------------------------------------------------------------------+
| NVIDIA-SMI 410.78       Driver Version: 410.78       CUDA Version: 10.0     |
|-------------------------------+----------------------+----------------------+
| GPU  Name        Persistence-M| Bus-Id        Disp.A | Volatile Uncorr. ECC |
| Fan  Temp  Perf  Pwr:Usage/Cap|         Memory-Usage | GPU-Util  Compute M. |
|===============================+======================+======================|
|   0  GeForce GTX 1050    Off  | 00000000:01:00.0 Off |                  N/A |
| N/A   49C    P8    N/A /  N/A |    575MiB /  4042MiB |      4%      Default |
+-------------------------------+----------------------+----------------------+
                                                                               
+-----------------------------------------------------------------------------+
| Processes:                                                       GPU Memory |
|  GPU       PID   Type   Process name                             Usage      |
|=============================================================================|
|    0      1120      G   /usr/lib/xorg/Xorg                           242MiB |
|    0      2072      G   compiz                                       171MiB |
|    0      2900      G   ...quest-channel-token=4410341543763773962    40MiB |
|    0      2901      G   ...uest-channel-token=16701371682146151933    97MiB |
|    0      4853      G   ...-token=89AA74BC02F6D5F95206AB3318F45465    23MiB |
+-----------------------------------------------------------------------------+

```

​	2.apt安装

​	添加Graphic Drivers PPA

```
$ sudo add-apt-repository ppa:graphics-drivers/ppa
$ sudo apt-get update
```

​	寻找NVIDIA驱动版本

```
ubuntu-drivers devices
== /sys/devices/pci0000:00/0000:00:01.0/0000:01:00.0 ==
modalias : pci:v000010DEd00001C8Dsv00001462sd000011C8bc03sc02i00
vendor   : NVIDIA Corporation
driver   : xserver-xorg-video-nouveau - distro free builtin
driver   : nvidia-384 - distro non-free
driver   : nvidia-396 - third-party free
driver   : nvidia-410 - third-party free
driver   : nvidia-390 - third-party free
driver   : nvidia-415 - third-party free recommended
```

​	安装显卡驱动：

```
sudo apt install nvidia-410
```

​	安装完成之后reboot重启电脑，使用nvidia-smi查看安装状态上述安装同样

#### 二、安装cuda与cudnn

​	从上述安装的显卡驱动中可以发现我的显卡所匹配的cuda的类型是CUDA Version: 10.0，在该网站上下载cuda10.0<https://developer.nvidia.com/cuda-10.0-download-archive?target_os=Linux&target_arch=x86_64&target_distro=Ubuntu&target_version=1604&target_type=deblocal>，![](/home/user/Pictures/cuda.jpg)

我下载的cuda是cuda的deb包，下载之后按照安装说明，进行cuda的安装。然后在这个网站上查看cuda与cudnn和tensorflow-gpu多对应的版本<https://tensorflow.google.cn/install/source>。在该网站上下载cudnn<https://developer.nvidia.com/rdp/cudnn-archive>，首先去注册一个NVIDIA的网站的账号，登录之后寻找你所对应的cudnn下载。

​	将文件下载好之后，通过以下命令安装cuda

```
sudo dpkg -i cuda-repo-ubuntu1604-10-0-local-10.0.130-410.48_1.0-1_amd64.deb
sudo apt-key add /var/cuda-repo-10-0-local-10.0.130-410.48/7fa2af80.pub
sudo apt-get update
sudo apt-get install cuda
```

​	安装完成之后将以下环境变量添加到.bashrc中，

```
export CUDA_HOME=/usr/local/cuda
export PATH=$PATH:$CUDA_HOME/bin
export LD_LIBRARY_PATH=/usr/local/cuda-10.0/lib64${LD_LIBRARY_PATH:+:${LD_LIBRARY_PATH}}
```

​	然后进入到cd /usr/local/cuda/samples/1_Utilities/deviceQuery该目录下执行下面语句查看cuda是否安装成功

```
sudo make
./deviceQuery 
```

出现如下内容则表明安装成功

```
./deviceQuery Starting...

 CUDA Device Query (Runtime API) version (CUDART static linking)

Detected 1 CUDA Capable device(s)

Device 0: "GeForce GTX 1050"
  CUDA Driver Version / Runtime Version          10.0 / 10.0
  CUDA Capability Major/Minor version number:    6.1
  Total amount of global memory:                 4042 MBytes (4238737408 bytes)
  ( 5) Multiprocessors, (128) CUDA Cores/MP:     640 CUDA Cores
  GPU Max Clock rate:                            1493 MHz (1.49 GHz)
  Memory Clock rate:                             3504 Mhz
  Memory Bus Width:                              128-bit
  L2 Cache Size:                                 524288 bytes
  Maximum Texture Dimension Size (x,y,z)         1D=(131072), 2D=(131072, 65536), 3D=(16384, 16384, 16384)
  Maximum Layered 1D Texture Size, (num) layers  1D=(32768), 2048 layers
  Maximum Layered 2D Texture Size, (num) layers  2D=(32768, 32768), 2048 layers
  Total amount of constant memory:               65536 bytes
  Total amount of shared memory per block:       49152 bytes
  Total number of registers available per block: 65536
  Warp size:                                     32
  Maximum number of threads per multiprocessor:  2048
  Maximum number of threads per block:           1024
  Max dimension size of a thread block (x,y,z): (1024, 1024, 64)
  Max dimension size of a grid size    (x,y,z): (2147483647, 65535, 65535)
  Maximum memory pitch:                          2147483647 bytes
  Texture alignment:                             512 bytes
  Concurrent copy and kernel execution:          Yes with 2 copy engine(s)
  Run time limit on kernels:                     Yes
  Integrated GPU sharing Host Memory:            No
  Support host page-locked memory mapping:       Yes
  Alignment requirement for Surfaces:            Yes
  Device has ECC support:                        Disabled
  Device supports Unified Addressing (UVA):      Yes
  Device supports Compute Preemption:            Yes
  Supports Cooperative Kernel Launch:            Yes
  Supports MultiDevice Co-op Kernel Launch:      Yes
  Device PCI Domain ID / Bus ID / location ID:   0 / 1 / 0
  Compute Mode:
     < Default (multiple host threads can use ::cudaSetDevice() with device simultaneously) >

deviceQuery, CUDA Driver = CUDART, CUDA Driver Version = 10.0, CUDA Runtime Version = 10.0, NumDevs = 1
Result = PASS
```

安装cudnn

```
tar xvf cudnn-10.0-linux-x64-v7.4.1.5.tgz
cd cuda/include
ls
sudo cp cudnn.h /usr/local/cuda/include
cd lib64/
ls
sudo cp lib* /usr/local/cuda/lib64/
sudo chmod a+r /usr/local/cuda/include/cudnn.h /usr/local/cuda/lib64/libcudnn*
```



#### 三、tesorflow-gpu安装

​	此处我是使用Anaconda中创建tensorflow的运行虚拟环境，在该虚拟环境中安装tensorflow-gpu。

​	创建虚拟环境：

```
conda create -n tensorflow python=3.5
进入环创建的环境使用：
	conda activate tensorflow
退出使用：
	conda deactivate
```

​	安装conda install tensorflow-gup，完成之后，输入Python进入Python环境，然后使用下面命令查看安装是是否成功！

```
(tensorflow) xxx@xxx-msi:~$ python 
Python 3.5.6 |Anaconda, Inc.| (default, Aug 26 2018, 21:41:56) 
[GCC 7.3.0] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>> import tensorflow as tf
>>> 
```

​	到此，整个环境的部署已成功！

#### 若要转载，注明出处！！！！