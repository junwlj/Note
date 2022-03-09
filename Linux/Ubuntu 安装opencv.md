# 在Ubuntu 20.04上安装OpenCV

​		[OpenCV ](https://opencv.org/)（开源计算机视觉库）是一种开源计算机视觉库，具有针对C ++，Python和Java的绑定，并支持所有主要操作系统。 它可以利用多核处理的优势，并具有GPU加速功能以进行实时操作。

​		OpenCV可用于广泛的应用程序，包括医学图像分析，拼接街景图像，监视视频，检测和识别人脸，跟踪运动对象，提取3D模型等。

### 1. 从Ubuntu存储库安装OpenCV

​		可从默认的Ubuntu 20.04存储库安装OpenCV。 要安装它，请运行：

```bash
sudo apt update
sudo apt install libopencv-dev python3-opencv
```

​		上面的命令将安装OpenCV所需的所有软件包。

​		通过导入`cv2`模块并打印OpenCV版本来验证安装：

```bash
python3 -c "import cv2; print(cv2.__version__)"
```

在撰写本文时，存储库中的版本为4.2：

### 2. 从源代码安装OpenCV

​		从源代码构建OpenCV库可让您获得最新的可用版本。 它将针对您的特定系统进行优化，并且您将完全控制构建选项。 这是安装OpenCV的推荐方法。

执行以下步骤以从源代码安装最新的OpenCV版本：

#### 2.1 安装[构建工具](https://www.myfreax.com/how-to-install-gcc-on-ubuntu-20-04/)和依赖项：

```shell
sudo apt install build-essential cmake git pkg-config libgtk-3-dev \
    libavcodec-dev libavformat-dev libswscale-dev libv4l-dev \
    libxvidcore-dev libx264-dev libjpeg-dev libpng-dev libtiff-dev \
    gfortran openexr libatlas-base-dev python3-dev python3-numpy \
    libtbb2 libtbb-dev libdc1394-22-dev libopenexr-dev \
    libgstreamer-plugins-base1.0-dev libgstreamer1.0-dev
```

#### 2.2 克隆OpenCV和OpenCV贡献存储库：

```bash
mkdir ~/opencv_build && cd ~/opencv_build
git clone https://github.com/opencv/opencv.git
git clone https://github.com/opencv/opencv_contrib.git
```

 		在撰写本文时，github存储库中的默认版本是版本4.3.0。如果要安装较旧版本的OpenCV，请在`opencv`和`opencv_contrib`目录中安装并运行`git checkout <opencv-version>`

​		下载完成后，创建一个临时构建目录，并对其进行[导航](https://www.myfreax.com/linux-cd-command/)：

```bash
cd ~/opencv_build/opencv
mkdir -p build && cd build
```

#### 2.3 使用CMake设置OpenCV构建：

```shell
cmake -D CMAKE_BUILD_TYPE=RELEASE \
    -D CMAKE_INSTALL_PREFIX=/usr/local \
    -D INSTALL_C_EXAMPLES=ON \
    -D INSTALL_PYTHON_EXAMPLES=ON \
    -D OPENCV_GENERATE_PKGCONFIG=ON \
    -D OPENCV_EXTRA_MODULES_PATH=~/opencv_build/opencv_contrib/modules \
    -D BUILD_EXAMPLES=ON ..
```

​		输出结果为:

```shell
-- Configuring done
-- Generating done
-- Build files have been written to: /home/vagrant/opencv_build/opencv/build
```

​		开始编译：

```shell
make -j12
```

​		根据您的处理器修改`-j`选项。如果您不知道处理器的内核数，可以通过键入`nproc`找到CPU核心数。编译可能需要几分钟或更长时间，具体取决于您的系统配置。

#### 2.4 安装OpenCV：

```bash
sudo make install
```

​		要验证安装，请键入以下命令，您应该会看到OpenCV版本

#### 2.5 C ++绑定：

```bash
pkg-config --modversion opencv4
```

```bash
4.3.0
```

#### 2.6 Python绑定：

```bash
python3 -c "import cv2; print(cv2.__version__)"
```

```bash
4.3.0-dev
```

#### 参考

本文参考博客：
 [https://www.myfreax.com/how-to-install-opencv-on-ubuntu-20-04/]



