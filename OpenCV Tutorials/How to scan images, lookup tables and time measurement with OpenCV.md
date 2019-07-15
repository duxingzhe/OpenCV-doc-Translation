目的：

我们将在这篇教程中解决以下问题：

* 我们将如何遍历一张图片的每一个像素？
* OpenCV矩阵值具体是如何存储的？
* 如何衡量我们的算法性能？
* 什么是查找表？如何使用查找表？

测试用例

我们先来分析一下一个简单的颜色消除算法。矩阵中的元素数据是按照C和C++的无符号字符类型存储，每一个像素的通道有256种不同的值。对一个有三个通道的图像来说，我们可以生成许多种颜色（大概有1600万颜色）。对这么多的颜色进行处理会严重降低算法的性能。然而，有时候如果我们能对这个图像的部分数据进行舍弃的话，我们依旧可以达到我们想要的效果。

在这个例子当中，我们会使用色域消减的方法来提高性能。也就是说，我们将色域分解，然后设定范围，让当前色域的值设置成一个新值，从而减少颜色的处理规模。这个例子里，0-9的值，我们会赋值为0；10-19的值，我们就赋值为10。

当你区分uchar（unsigned char，其值在0-255之间）和int类型的值时，程序会自动转换为char类型。这些值同样会变成char类型。因此，值域范围会逐步缩小。使用这个功能之后，uchar的值的改变是由以下公式决定的：

![](http://latex.codecogs.com/gif.latex?I_{new}=(\frac{I_{old}}{10})*10)

A simple color space reduction algorithm would consist of just passing through every pixel of an image matrix and applying this formula. It's worth noting that we do a divide and a multiplication operation. These operations are bloody expensive for a system. If possible it's worth avoiding them by using cheaper operations such as a few subtractions, addition or in best case a simple assignment. Furthermore, note that we only have a limited number of input values for the upper operation. In case of the uchar system this is 256 to be exact.

Therefore, for larger images it would be wise to calculate all possible values beforehand and during the assignment just make the assignment, by using a lookup table. Lookup tables are simple arrays (having one or more dimensions) that for a given input value variation holds the final output value. Its strength lies that we do not need to make the calculation, we just need to read the result.

Our test case program (and the sample presented here) will do the following: read in a console line argument image (that may be either color or gray scale - console line argument too) and apply the reduction with the given console line argument integer value. In OpenCV, at the moment there are three major ways of going through an image pixel by pixel. To make things a little more interesting will make the scanning for each image using all of these methods, and print out how long it took.

You can download the full source code here or look it up in the samples directory of OpenCV at the cpp tutorial code for the core section. Its basic usage is:

```
how_to_scan_images imageName.jpg intValueToReduce [G]
```
