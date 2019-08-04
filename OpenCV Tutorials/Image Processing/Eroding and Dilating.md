目的

在这篇教程中你将会学到：

* 应用两种非常常用的形态学操作：腐蚀和扩张。为了完成这项操作，你可以使用一下OpenCV函数：
    * cv::erode
    * cv::dilate

> 注意
> 
> 本教程的例子选自Richard Szeliski的《机器视觉：算法和应用》一书

形态学操作

* 简而言之：形态学就是一系列对图像中的物体形状进行处理的操作。形态学操作是对输入图片的物体进行一系列建模操作然后生成最终图形。

* 大多数常见的形态学操作有：腐蚀和扩张。他们有各种各样的用法，例如：

    * 去除噪声
    * 隔离独立的元素并放置到图像中的其他元素中去
    * 检测图像中的碰撞强度或者是撞击后的洞

* 我们将使用以下图片为侵蚀和扩张进行简单的介绍：

![](https://docs.opencv.org/4.1.0/Morphology_1_Tutorial_Theory_Original_Image.png)

侵蚀

* 这个操作包括了一张图片和一些核函数，核函数可以是任意大小或者形状，通常是一个正方形或圆形。
* 核函数B通常有一个固定点，通常是核函数的中心。
* As the kernel B is scanned over the image, we compute the maximal pixel value overlapped by B and replace the image pixel in the anchor point position with that maximal value. As you can deduce, this maximizing operation causes bright regions within an image to "grow" (therefore the name dilation).
* The dilatation operation is:![](http://latex.codecogs.com/gif.latex?\texttt{dst}(x,y)=\max_{(x',y'):\,\texttt{element}(x',y')\ne0}\texttt{src}(x+x',y+y'))
* Take the above image as an example. Applying dilation we can get:

![](https://docs.opencv.org/4.1.0/Morphology_1_Tutorial_Theory_Dilation.png)

* The bright area of the letter dilates around the black regions of the background.

Erosion

* This operation is the sister of dilation. It computes a local minimum over the area of given kernel.
* As the kernel B is scanned over the image, we compute the minimal pixel value overlapped by B and replace the image pixel under the anchor point with that minimal value.
The erosion operation is: ![](http://latex.codecogs.com/gif.latex?\texttt{dst}(x,y)=\min_{(x',y'):\,\texttt{element}(x',y')\ne0}\texttt{src}(x+x',y+y'))

* Analagously to the example for dilation, we can apply the erosion operator to the original image (shown above). You can see in the result below that the bright areas of the image get thinner, whereas the dark zones gets bigger.

![](https://docs.opencv.org/4.1.0/Morphology_1_Tutorial_Theory_Erosion.png)