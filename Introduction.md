OpenCV（开源机器视觉库的缩写：[http://opencv.org](http://opencv.org ""))是一个基于BSD协议的开源库，它包含了上百种机器视觉算法。本篇文档介绍了OpenCV 2.x的API使用，这些API都是C++风格，而不是OpenCV 1.x的C风格API（C版本的API已经被弃用，从2.4版本开始，已经不再推荐使用C编译器来测试功能）。

OpenCV提供了一个模块化的结构，也就说，OpenCV是由几个动态库或静态库组成的。OpenCV包含了以下库：

* 核心库（core） ：一个压缩的模块，定义了基本的数据结构，其中包括一个高纬度的数组Mat和提供给其他库使用的基本函数。

* 图像处理库 （imgproc） ：一个包含了线性和非线性图形滤镜、图形的几何变换（放缩、放射、透视、基于通用表的重映射），色彩空间转换，直方图等功能的图像处理库。

* 视频分析（Video）：包含了运动预测，背景提取和物体轨迹描述算法的模块。

* 摄像机校准和3D重建（calib3d）:基本多视图几何算法，简单的立体相机校准，物体形态预测，立体匹配算法和3d重建元素绘制。

* 2D特征框架（features 2d）：突出特征检测装置、描述器和特征描述匹配。

* 物体检测（objdetect）：对物体和预先设置的类（如，脸，眼睛，大杯子，人，车等）进行检测。

* 高阶图形用户界面（highgui）：对用户非常友好的简单界面设计程序

* 视频I/O：提供简单易用的视频捕捉和视频解析接口

* 另外，还有一些帮助模块，例如FLANN和Google test封装接口，Python绑定模块和其他的一些接口。

The further chapters of the document describe functionality of each module. But first, make sure to get familiar with the common API concepts used thoroughly in the library.

API Concepts
cv Namespace
All the OpenCV classes and functions are placed into the cv namespace. Therefore, to access this functionality from your code, use the cv:: specifier or using namespace cv; directive:

```
#include "opencv2/core.hpp"
...
cv::Mat H = cv::findHomography(points1, points2, cv::RANSAC, 5);
...
```

or:

```
#include "opencv2/core.hpp"
using namespace cv;
...
Mat H = findHomography(points1, points2, RANSAC, 5 );
...
```
