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

以后更为深入的章节将会详细介绍每一个模块的具体用法。但是，首先，我们应该对OpenCV库中大多数API的用法有所了解。

API风格

cv命名空间

所有的OpenCV类和函数都放置在cv命名空间下。因此，如果要使用这些函数，要确保使用cv::标识符或者在使用前声明命名空间using namespace cv；

直接使用：

```
#include "opencv2/core.hpp"
...
cv::Mat H = cv::findHomography(points1, points2, cv::RANSAC, 5);
...
```

或者

```
#include "opencv2/core.hpp"
using namespace cv;
...
Mat H = findHomography(points1, points2, RANSAC, 5 );
...
```

由于不能保证目前或者未来某个时刻OpenCV的外部函数名会与STL或者是其它库发生冲突，请使用显式命名空间标识符来解决命名空间冲突问题：

```
Mat a(100, 100, CV_32F);
randu(a, Scalar::all(1), Scalar::all(std::rand()));
cv::log(a, a);
a /= std::log(2.);
```

自动内存分配管理

OpenCV可以自动处理所有的内存分配问题

首先，std::vector，cv::Mat和其它在函数和方法中使用的数据结构都有析构函数，用于在需要的时候回收占用的内存缓冲区。这就意味着析构函数在Mat中不会总是去尝试回收缓冲区。它是用来表示可能的数据共享量。析构函数会每次执行之后在matrix数据缓冲区中将对应的引用计数减一。缓冲区在且仅在共享缓冲计数变为零的时候才会被回收。缓冲计数变为零表示没有其他的数据结构引用这个缓冲区域。同样的，当一个Mat实例被复制之后，并没有实际数据被复制。而是通过引用计数加一的方式来表示有其他的数据结构在使用同一个缓冲区。我们可以使用Mat::clone方法来创建一个Matrix数据的深拷贝。请查看下面的例子：

```
// create a big 8Mb matrix
Mat A(1000, 1000, CV_64F);
// create another header for the same matrix;
// this is an instant operation, regardless of the matrix size.
Mat B = A;
// create another header for the 3-rd row of A; no data is copied either
Mat C = B.row(3);
// now create a separate copy of the matrix
Mat D = B.clone();
// copy the 5-th row of B to C, that is, copy the 5-th row of A
// to the 3-rd row of A.
B.row(5).copyTo(C);
// now let A and D share the data; after that the modified version
// of A is still referenced by B and C.
A = D;
// now make B an empty matrix (which references no memory buffers),
// but the modified version of A will still be referenced by C,
// despite that C is just a single row of the original A
B.release();
// finally, make a full copy of C. As a result, the big modified
// matrix will be deallocated, since it is not referenced by anyone
C = C.clone();
```

你可以看到使用Mat和其他数据结构的用法是非常简单的。但是对于那些高级类或者甚至没有将自动内存处理考虑进去的自定义数据结构该怎么办呢？对此，OpenCV则提供了cv::Ptr模板类，这个类的使用方法和C++ 11中的std::shared_ptr类似。因此，与其使用普通指针类型：

```
T* ptr = new T(...);
```

你可以这么使用

```
Ptr<T> ptr(new T(...));
```

或者：

```
Ptr<T> ptr = makePtr<T>(...);
```

Ptr<T> 封装了一个指向T实例的指针和一个与指针相关的引用计数。cv::Ptr的描述页面提供了更多细节

对输出数据自动进行内存分配

OpenCV deallocates the memory automatically, as well as automatically allocates the memory for output function parameters most of the time. So, if a function has one or more input arrays (cv::Mat instances) and some output arrays, the output arrays are automatically allocated or reallocated. The size and type of the output arrays are determined from the size and type of input arrays. If needed, the functions take extra parameters that help to figure out the output array properties.