目的：

我们将在这篇教程中回答一下问题：

* 我们将如何遍历一张图片的每一个像素？
* OpenCV矩阵值具体是如何存储的？
* 如何衡量我们的算法性能？
* 什么是查找表？如何使用查找表？

测试用例

我们先来分析一下一个简单的颜色消除算法。矩阵中的元素数据是按照C和C++的无符号字符类型存储，每一个像素的通道有256种不同的值。对一个有三个通道的图像来说，我们可以生成许多种颜色（大概有1600万颜色）。对这么多的颜色进行处理会严重降低算法的性能。然而，有时候如果我们能对这个图像的部分数据进行舍弃的话，我们依旧可以达到我们想要的效果。

In this cases it's common that we make a color space reduction. This means that we divide the color space current value with a new input value to end up with fewer colors. For instance every value between zero and nine takes the new value zero, every value between ten and nineteen the value ten and so on.

When you divide an uchar (unsigned char - aka values between zero and 255) value with an int value the result will be also char. These values may only be char values. Therefore, any fraction will be rounded down. Taking advantage of this fact the upper operation in the uchar domain may be expressed as:
