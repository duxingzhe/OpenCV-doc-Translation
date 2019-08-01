目的

在本篇教程中，你将会学到：

* 通过OpenCV的line()函数画线
* 通过OpenCV的ellipse()函数画椭圆
* 通过OpenCV的rectangle()函数画矩形
* 通过OpenCV的circle()函数画圆
* 通过OpenCV的fillPoly()函数画实心多边形

OpenCV原理

在这篇教程中，我们主要使用两个数据结构：cv::Point和cv::Scalar：

Point

它代表的是一个二维的点，通过指定他在图像中的x和y坐标来确定一个点。我们可以用下面的方式定义：

```
Point pt;
pt.x = 10;
pt.y = 8;
```

或者

```
Point pt =  Point(10, 8);
```

Scalar

它代表的是一个有四个元素的向量。Scalar类型变量在OpenCV中常用于传递像素值。在这篇教程中，我们将用它来代表BGR颜色值（3个变量）。如果最后一个参数并没有被用到，我们可以不用去定义。

现在我们来看一个例子，假设我们需要一个颜色参数，于是我们做了如下定义：

```
Scalar( a, b, c )
```

我们将定义了一个BGR颜色值：Blue = a, Green = b和Red = c。