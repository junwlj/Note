在 Windows 下安装 COCO API（pycocotools）

空杯的境界 2018-09-13 14:35:03  15891  收藏 35
分类专栏： 01_机器学习 文章标签： COCO pycocotools Windows Python 目标检测
版权

01_机器学习
专栏收录该内容
23 篇文章0 订阅
订阅专栏
  本内容将介绍在 Windows 下安装 COCO API（pycocotools）。

  本来 COCO 对 Windows 是不支持的。不过为了支持 Windows ，有人对 COCO 做了一些修改。下面是 COCO 在 GitHub 上源码地址信息：
  COCO 地址： https://github.com/cocodataset/cocoapi
  支持 Windows 的 COCO 地址：https://github.com/philferriere/cocoapi


在 Windows 下安装 COCO API
  有两种方法：

（1）使用 pip 命令进行安装
  在 CMD 终端中直接运行以下指令：

pip install git+https://github.com/philferriere/cocoapi.git#subdirectory=PythonAPI
1
  如果这种方法无法正常安装，可以使用下面的方法进行安装。

（2）具体安装步骤如下：
在 https://github.com/philferriere/cocoapi 下载源码，并进行解压。
以管理员身份打开 CMD 终端，并切换到 *\cocoapi-master\PythonAPI 目录。
运行以下指令：
python setup.py build_ext install
1
  运行以上指令时如果出现以下错误提示：

error: Microsoft Visual C++ 14.0 is required.
// 或者
error: Unable to find vcvarsall.bat
1
2
3
  解决方法：此种安装方法需要使用 Microsoft Visual C++ 14.0 对 COCO 源码进行编译。如果本地不支持 Microsoft Visual C++ 14.0 或者版本低于 14.0，可以通过安装 Microsoft Visual Studio 2015 及以上版本。
------------------------------------------------
版权声明：本文为CSDN博主「空杯的境界」的原创文章，遵循CC 4.0 BY-SA版权协议，转载请附上原文出处链接及本声明。
原文链接：https://blog.csdn.net/benzhujie1245com/article/details/82686973