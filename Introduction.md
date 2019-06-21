OpenCV（开源机器视觉库的缩写：[http://opencv.org](http://opencv.org ""))是一个基于BSD协议的开源库，它包含了上百种机器视觉算法。本篇文档介绍了OpenCV 2.x的API使用，这些API都是C++风格，而不是OpenCV 1.x的C风格API（C版本的API已经被弃用，从2.4版本开始，已经不再推荐使用C编译器来测试功能）。

OpenCV提供了一个模块化的结构，也就说，OpenCV是由几个动态库或静态库组成的。OpenCV包含了以下库：

核心库：一个压缩的模块，定义了基本的数据结构，其中包括一个高纬度的数组Mat和提供给其他库使用的基本函数。

图像处理库：一个包含了线性和非线性图形滤镜、图形的几何变换（放缩、放射、透视、基于通用表的重映射），色彩空间转换，直方图等功能的图像处理库。

Video Analysis (video) - a video analysis module that includes motion estimation, background subtraction, and object tracking algorithms.
Camera Calibration and 3D Reconstruction (calib3d) - basic multiple-view geometry algorithms, single and stereo camera calibration, object pose estimation, stereo correspondence algorithms, and elements of 3D reconstruction.
2D Features Framework (features2d) - salient feature detectors, descriptors, and descriptor matchers.
Object Detection (objdetect) - detection of objects and instances of the predefined classes (for example, faces, eyes, mugs, people, cars, and so on).
High-level GUI (highgui) - an easy-to-use interface to simple UI capabilities.
Video I/O (videoio) - an easy-to-use interface to video capturing and video codecs.
... some other helper modules, such as FLANN and Google test wrappers, Python bindings, and others.
