目标

在本篇教程你将学到：

* 我们使用OpenCV的cv::threshold函数实现基本的阈值操作

理论介绍

> 注意
>
> 这些例子来自Bradski和Kaehler所著的Learning OpenCV一书。

什么是阈值？

* 最简单的分割方法
* 应用例子：将图片中的物体进行分隔，以提取出我们需要分析的物体。这种分割算法是基于物体的像素和背景像素之间的强度变量值。
* 为了从剩下的部分中区分出我们感兴趣的像素（最终不会包含在结果里），我们会对每个像素的强度值计算后，跟阈值进行比较（阈值需要根据你需要解决的问题进行设置）。
* 一旦我们正确分离了重要的像素，我们就可以个用一个确定的颜色值来标记他们（例如，我们可以用0（黑色）、255（白色）或者其他颜色值来标记，只要这个颜色能满足你的需要）。

![](https://docs.opencv.org/4.1.0/Threshold_Tutorial_Theory_Example.jpg)

阈值的类型

* OpenCV提供了cv::Threshold来进行阈值操作。
* 我们可以通过这个函数实现5中不同类型的阈值操作。我们将会在剩下的部分中为大家一一讲解。
* 为了能说明阈值操作的作用，我们来考虑一张有像素强度值src(x,y)的照片。下面的关系图揭示了像素值之间的强度关系。垂直的蓝色代表的是阈值（固定的）。

![](https://docs.opencv.org/4.1.0/Threshold_Tutorial_Theory_Base_Figure.png)

二值阈值

* This thresholding operation can be expressed as:

![](http://latex.codecogs.com/gif.latex?\texttt{dst}(x,y)=%5Cbegin%7Bcases%7D%20%26%20maxVal%5Ctexttt%7B%20if%20%7D%20%5Ctexttt%7Bsrc%28x%2Cy%29%7D%3D%20%5Ctexttt%7Bthresh%7D%20%5C%5C%20%26%20%5Ctexttt%7B0%7D%20%5Ctexttt%7B%20otherwise%20%7D%20%5Cend%7Bcases%7D)

* So, if the intensity of the pixel src(x,y) is higher than thresh, then the new pixel intensity is set to a MaxVal. Otherwise, the pixels are set to 0.

![](https://docs.opencv.org/4.1.0/Threshold_Tutorial_Theory_Binary.png)

Threshold Binary, Inverted

* This thresholding operation can be expressed as:

![](http://latex.codecogs.com/gif.latex?\texttt{dst}(x,y)=%5Cbegin%7Bcases%7D%20%26%20%5Ctexttt%7B0%7D%5Ctexttt%7B%20if%20%7D%20%5Ctexttt%7Bsrc%28x%2Cy%29%7D%3D%20%5Ctexttt%7Bthresh%7D%20%5C%5C%20%26%20maxVal%20%5Ctexttt%7B%20otherwise%20%7D%20%5Cend%7Bcases%7D)

* If the intensity of the pixel src(x,y) is higher than thresh, then the new pixel intensity is set to a 0. Otherwise, it is set to MaxVal.

![](https://docs.opencv.org/4.1.0/Threshold_Tutorial_Theory_Binary_Inverted.png)

Truncate

* This thresholding operation can be expressed as:

![](http://latex.codecogs.com/gif.latex?\texttt{dst}(x,y)=%5Cbegin%7Bcases%7D%20%26%20%5Ctexttt%7Bthreshold%7D%2C%20%5Ctexttt%7Bsrc%7D%28x%2Cy%29%3E%20%5Ctexttt%7Bthresh%7D%20%5C%5C%20%26%20%5Ctexttt%7Bsrc%7D%28x%2Cy%29%2Cotherwise%20%5Cend%7Bcases%7D)

* The maximum intensity value for the pixels is thresh, if src(x,y) is greater, then its value is truncated. See figure below:

![](https://docs.opencv.org/4.1.0/Threshold_Tutorial_Theory_Truncate.png)

Threshold to Zero

* This operation can be expressed as:

![](http://latex.codecogs.com/gif.latex?\texttt{dst}(x,y)=%5Cbegin%7Bcases%7D%20%26%20src(x,y)%2C%20%5Ctexttt%7Bsrc%7D%28x%2Cy%29%3E%20%5Ctexttt%7Bthresh%7D%20%5C%5C%20%26%200%2Cotherwise%20%5Cend%7Bcases%7D)

* If src(x,y) is lower than thresh, the new pixel value will be set to 0.

![](https://docs.opencv.org/4.1.0/Threshold_Tutorial_Theory_Zero.png)

Threshold to Zero, Inverted

* This operation can be expressed as:

![](http://latex.codecogs.com/gif.latex?\texttt{dst}(x,y)=%5Cbegin%7Bcases%7D%20%26%20%5Ctexttt%7Bsrc%28x%2Cy%29%7D%5Ctexttt%7B%20if%20%7D%20%5Ctexttt%7Bsrc%28x%2Cy%29%7D%3D%20%5Ctexttt%7Bthresh%7D%20%5C%5C%20%26%20%5Ctexttt%7B0%7D%20%5Ctexttt%7B%20otherwise%20%7D%20%5Cend%7Bcases%7D)

* If src(x,y) is greater than thresh, the new pixel value will be set to 0.

![](https://docs.opencv.org/4.1.0/Threshold_Tutorial_Theory_Zero_Inverted.png)