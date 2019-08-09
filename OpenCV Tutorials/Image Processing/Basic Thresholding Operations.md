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
* To differentiate the pixels we are interested in from the rest (which will eventually be rejected), we perform a comparison of each pixel intensity value with respect to a threshold (determined according to the problem to solve).
* Once we have separated properly the important pixels, we can set them with a determined value to identify them (i.e. we can assign them a value of 0 (black), 255 (white) or any value that suits your needs).

![](https://docs.opencv.org/4.1.0/Threshold_Tutorial_Theory_Example.jpg)

Types of Thresholding

* OpenCV offers the function cv::threshold to perform thresholding operations.
* We can effectuate 5 types of Thresholding operations with this function. We will explain them in the following subsections.
* To illustrate how these thresholding processes work, let's consider that we have a source image with pixels with intensity values src(x,y). The plot below depicts this. The horizontal blue line represents the threshold thresh (fixed).

![](https://docs.opencv.org/4.1.0/Threshold_Tutorial_Theory_Base_Figure.png)

Threshold Binary

* This thresholding operation can be expressed as:

dst(x,y)={maxVal0if src(x,y)>threshotherwise

* So, if the intensity of the pixel src(x,y) is higher than thresh, then the new pixel intensity is set to a MaxVal. Otherwise, the pixels are set to 0.

![](https://docs.opencv.org/4.1.0/Threshold_Tutorial_Theory_Binary.png)

Threshold Binary, Inverted

* This thresholding operation can be expressed as:

dst(x,y)={0maxValif src(x,y)>threshotherwise

* If the intensity of the pixel src(x,y) is higher than thresh, then the new pixel intensity is set to a 0. Otherwise, it is set to MaxVal.

![](https://docs.opencv.org/4.1.0/Threshold_Tutorial_Theory_Binary_Inverted.png)

Truncate

* This thresholding operation can be expressed as:

![](http://latex.codecogs.com/gif.latex?\texttt{dst}(x,y)=%5Cbegin%7Bcases%7D%20%26%200%2C%20%5Ctexttt%7Bsrc%7D%28x%2Cy%29%3E%20%5Ctexttt%7Bthresh%7D%20%5C%5C%20%26%20%5Ctexttt%7BmavVal%7D%2Cotherwise%20%5Cend%7Bcases%7D)

* The maximum intensity value for the pixels is thresh, if src(x,y) is greater, then its value is truncated. See figure below:

![](https://docs.opencv.org/4.1.0/Threshold_Tutorial_Theory_Truncate.png)

Threshold to Zero

* This operation can be expressed as:

![](http://latex.codecogs.com/gif.latex?\texttt{dst}(x,y)=%5Cbegin%7Bcases%7D%20%26%20%5Ctexttt%7Bthreshold%7D%2C%20%5Ctexttt%7Bsrc%7D%28x%2Cy%29%3E%20%5Ctexttt%7Bthresh%7D%20%5C%5C%20%26%20%5Ctexttt%7Bsrc%7D%28x%2Cy%29%2Cotherwise%20%5Cend%7Bcases%7D)

* If src(x,y) is lower than thresh, the new pixel value will be set to 0.

![](https://docs.opencv.org/4.1.0/Threshold_Tutorial_Theory_Zero.png)

Threshold to Zero, Inverted

* This operation can be expressed as:

![](http://latex.codecogs.com/gif.latex?\texttt{dst}(x,y)=%5Cbegin%7Bcases%7D%20%26%20maxVal%2C%20%5Ctexttt%7Bsrc%7D%28x%2Cy%29%3E%20%5Ctexttt%7Bthresh%7D%20%5C%5C%20%26%200%2Cotherwise%20%5Cend%7Bcases%7D)

* If src(x,y) is greater than thresh, the new pixel value will be set to 0.

![](https://docs.opencv.org/4.1.0/Threshold_Tutorial_Theory_Zero_Inverted.png)