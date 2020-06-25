# 安装



### 方法三： 最省心的方案

教程：https://medium.com/analytics-vidhya/vs-code-with-opencv-c-on-windows-10-explained-256418442c52 
  到 https://github.com/huihut/OpenCV-MinGW-Build 下载一个MinGW编译好了的opencv, 解压后路径放到环境变量中， 

  然后配置vscode的三个json文件就可以开心的使用opencv了！ 

  这三个json文件在本目录的`vscode配置`中，直接拉到.vscode文件夹中即可。



### 方法一：使用预编译的opencv二进制包

直接在官网下载编译好的opencv二进制包`opencv-x.x.0-vc14_vc15.exe`

看到vc15就该明白，这玩意儿是visual studio编译出来的，如果想用mingw，请看方法二 或三。

解压后会得到两个文件夹： build 和 source

build就是预编译好的opencv，而 source是源码。

预编译好的opencv路径添加至环境变量：

```bash
setx -m OPENCV_DIR C:\opencv\build\x64\vc15
```

即可添加静态链接的opencv.

如果希望使用opencv的动态链接库(DLL),就需要告诉系统到哪里找到它的二进制库：

只需要在路径中添加`%OPENCV_DIR%\bin` ,在win10中将该路径添加至系统变量的path中。

**但这种方式无法使用cuda，所以不推荐**

---



### 方法二: 通过源码构建


* 安装mingw (https://sourceforge.net/projects/mingw-w64/files/mingw-w64/), 选择
  随便是online安装或者是离线安装，如`x86_64-posix-seh`，也就是"Threads",必须是选择 "posix"而不是"win32"，因为win32线程模型不支持C ++ 11线程

  路径加入环境变量： 

  ```bash
  C:\mingw\mingw64\bin
  C:\mingw\mingw64\lib
  C:\mingw\mingw64\include
  C:\mingw\mingw64\libexec
  ```

* 安装好cmake,加入环境变量： `C:\Program Files\CMake\bin`

* 使用方法一提到的source， 或者去github仓库下载：`git clone https://github.com/opencv/opencv.git` 以及额外的库 `git clone https://github.com/opencv/opencv_contrib.git`，两者解压后放在同一目录下，入我将它们放在了Opencv目录中：

  ```bash
  OpenCv/
  ├── Build
  ├── opencv-4.3.0
  └── opencv_contrib
  ```

  我新建了一个Build目录，为构建opencv和contrib新建了两个文件夹，
  结构如下(`mkdir -p Build/opencv` , `mkdir -p Build/opencv_contrib`)：

  ```bash
  Build/
  ├── opencv
  └── opencv_contrib
  ```

* 打开cmake-gui
  
  * source code path: `C:/OpenCv/opencv-4.3.0`
  
  * build path: `C:/OpenCv/Build/opencv`
  
  * 点击Configure即可触发配置，第一次配置需要输入编译器类型以及Makefile类型：
    在这里我们先选择MinGW的编译方式需要生成的是MinGW Makefile，编译器由我们自己来指定-->Next：
    compilers 选择 C: `C:/mingw/mingw64/bin/gcc.exe` , C++: `C:/mingw/mingw64/bin/g++.exe`, 然后finish.
  
  * cmake将进行一系列的检测
  
  * 检测完一轮后，会出现很多编译选项，如果我们希望使用CUDA，则勾选WITH_CUDA
  
  * **BUT**!!!
    这里，如果使用MinGW , 在win10中将不能使用CUDA（due to only Visual Studio compiler supported on your platform）
    所以，还不如直接用方法一来得省事。
  
  * 坑2
    ffmpeg donwload失败, 通过build\opencv目录下的CMakeDownloadLog.txt可以找到对应的下载地址，然后手动下载方到source目录下的.cache文件夹中，其中ffmpeg我已经手动下载好了，在当前目录下中可找到。
  
  * generate：生成MinGW Makefile
  
  * 管理员到`C:\OpenCv\Build\opencv` 下：
  
    ```bash
    cd C:\OpenCv\Build\opencv
    mingw32-make
    mingw32-make install
    ```
  
  
  
  




