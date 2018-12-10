---
layout: blog0
codes: true
background: yellow
title:  在windows安装OpenCV及其contrib和Matlab应用
date:   2018-10-06 23:41:30
background-image: https://www.google.com.hk/url?sa=i&rct=j&q=&esrc=s&source=images&cd=&cad=rja&uact=8&ved=2ahUKEwjx_9XUk_LdAhUI97wKHUtfAmsQjRx6BAgBEAU&url=https%3A%2F%2Fzh.wikipedia.org%2Fwiki%2FOpenCV&psig=AOvVaw0HWVnTqtpqSiysPuUTHdSa&ust=1538926907746498
category: 代码
tags:
- OpenCV
- Matlab
- MexOpenCV
- 配置
---
## 必需软件

1. CMake

2. Visual Studio等任何支持C++ compile的软件

3. Matlab

4. OpenCV

   [https://github.com/opencv/opencv/archive/3.4.0.zip]: 

   和OpenCV-contrib

   [https://github.com/opencv/opencv_contrib/archive/3.4.0.zip]: 

## CMake操作

   1. 打开CMake，在source code中选择OpenCV文件地址（保证该文件夹内有CMakeLists.txt文件）在build the binaries中选择编译目标文件夹。选择编译器（64位）单击`Configure`

   2. 取消选择`BUILD_DOCS`, `BUILD_EXAMPLES`, `BUILD_JAVA`, `BUILD_PACKAGE`, `BUILD_PERF_TESTS`, `BUILD_TESTS``BUILD_opencv_apps`, `BUILD_opencv_cuda*`, `BUILD_opencv_cudev`, `BUILD_opencv_java`, `BUILD_opencv_js`, `BUILD_opencv_python*`, `BUILD_opencv_ts`, `BUILD_opencv_viz`, `BUILD_opencv_world`

      `WITH_CUDA`, `WITH_CUFFT`, `WITH_CUBLAS`, `WITH_NVCUVID`, `WITH_MATLAB`, `WITH_VTK`

   3. 选择 `OPENCV_ENABLE_NONFREE`，在`OPENCV_EXTRA_MODULES_PATH`中选中`opencv_contrib`下的`modules`文件夹。

