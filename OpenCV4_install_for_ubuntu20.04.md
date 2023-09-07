参考
1. [《Installation in Linux》](https://docs.opencv.org/3.4/d7/d9f/tutorial_linux_install.html)
2. [《ubuntu20.04环境下安装opencv教程及测试》](http://www.taodudu.cc/news/show-3639529.html?)
3. [《在 Linux 系统中编译安装 OpenCV》](https://zhuanlan.zhihu.com/p/392751819)
4. [《为什么OpenCV4 “pkg-config --modversion opencv”显示“ No package ‘opencv‘ found”？解决方法！》](https://huaweicloud.csdn.net/635666ecd3efff3090b5d748.html)


#0
sudo apt-get install -y libcurl4 build-essential pkg-config cmake \
    libopenblas-dev libeigen3-dev libtbb-dev \
    libavcodec-dev libavformat-dev \
    libgstreamer-plugins-base1.0-dev libgstreamer1.0-dev \
    libswscale-dev libgtk-3-dev libpng-dev libjpeg-dev \
    libcanberra-gtk-module libcanberra-gtk3-module


#1
下载【https://github.com/opencv/opencv/releases/tag/4.8.0】中的Source code.zip

#2
解压

#3
cd opencv-4.8.0

#4
mkdir build

#5
cd build

#6
cmake -D CMAKE_BUILD_TYPE=Release -D OPENCV_GENERATE_PKGCONFIG=YES -D CMAKE_INSTALL_PREFIX=/usr/local/opencv4 ..

> -D CMAKE_BUILD_TYPE=Release // 指定编译的版本类型，Debug /Release/等
> -D OPENCV_GENERATE_PKGCONFIG=YES //：OpenCV4以上版本默认不使用pkg-config，该编译选项开启生成opencv4.pc文件，支持pkg-config功能。
> -D CMAKE_INSTALL_PREFIX=/usr/local/opencv4 // 设置OPENCV4的安装路径：如果 不设置 ，默认是/usr/local/

#7
sudo make -j8

#8
sudo make install

#9
sudo vim /etc/bash.bashrc
> 在文末加上：
PKG_CONFIG_PATH=$PKG_CONFIG_PATH:/usr/local/opencv4/lib/pkgconfig
export PKG_CONFIG_PATH

#10
source /etc/bash.bashrc
sudo updatedb

#11
cd /etc/ld.so.conf.d/
sudo touch opencv.conf  //opencv4.conf?
sudo vim opencv.conf    //opencv4.conf?
> 写入：
/usr/local/opencv4/lib

#12
pkg-config --modversion opencv4












