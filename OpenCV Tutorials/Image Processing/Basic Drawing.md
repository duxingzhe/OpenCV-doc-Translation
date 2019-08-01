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

Represents a 4-element vector. The type Scalar is widely used in OpenCV for passing pixel values.
In this tutorial, we will use it extensively to represent BGR color values (3 parameters). It is not necessary to define the last argument if it is not going to be used.

Let's see an example, if we are asked for a color argument and we give:

```
Scalar( a, b, c )
```

We would be defining a BGR color such as: Blue = a, Green = b and Red = c