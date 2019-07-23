目的

在这篇文章中你将会学到：

访问像素值

* 初始化生成一个全部为0的矩阵
* 学习cv::saturate_cast，了解到它的作用
* 学习一些十分好玩的像素转化算法
* 在一个简单的例子中学习如何增强图片的亮度

理论

> 注意
>
> 以下用例来自Richard Szeliski所著的《机器视觉：算法和应用》一书

图像处理

* 一个普通的图像操作方法是输入一个或多个图像并最后生成一个图像的函数。
* 图像转换可以是以下几种途径：
  * 点操作（像素转换）
  * 临近点（基于某一区域）操作
  
像素级转换

* 在这种图片处理转换中，每一个输出点的值和输入点的值之间都有一定的联系，而且只能与输入点像素值之间有联系（例如加，有的时候则是全局收集到信息或变量）。
* 这些操作的例子包括了亮度和对比度的调整以及颜色纠正和转换。

亮度和对比度调整

* 两个常用的点处理方法一般是通过常量进行加和乘：

![](http://latex.codecogs.com/gif.latex?g(x)=\alpha*f(x)+\beta)

* 变量![](http://latex.codecogs.com/gif.latex?\alpha)>0和![](http://latex.codecogs.com/gif.latex?\beta)通常被称为增益参数和修正参数；有时这些参数会用于控制对比度和亮度。

* 你可以认为f(x)是原图像像素，而g(x)是输出图像像素。那么，更加清晰的写法，我们可以用以下公式表示：

![](http://latex.codecogs.com/gif.latex?g(i,j)=\alpha*f(i,j)+\beta)

这里i和j表示图像中第i行、第j列的像素。

实际例子

在本篇，我们将之前提到的算法运用到实践当中，学习如何将曝光不足的照片进行亮度和对比度修正。我们同样学习一种名为伽马修正的图像亮度修复算法。

亮度和对比度修正

增加(减少)β的值将对每一个像素增加(/减少)一个常量。像素值在[0 ; 255]之外的将会被修正 (例如，如果计算后图像值大于(小于)255 (0)将会被修正成255(0)).

![](https://docs.opencv.org/4.1.0/Basic_Linear_Transform_Tutorial_hist_beta.png)

In light gray, histogram of the original image, in dark gray when brightness = 80 in Gimp

直方图表示的是每一个颜色区域中像素的总数。一张偏暗的图片有许多像素点是有较低的色值，所以直方图的峰会主要集中在左半部分。当加上了一个常量，直方图的峰会逐步往右移动。这是因为我们对每一个像素点都加上了一个常量。

变量α将改变对比度的范围。因此，如果α<1，则色级会被压缩，由此图片的对比度会下降。

![](https://docs.opencv.org/4.1.0/Basic_Linear_Transform_Tutorial_hist_alpha.png)

In light gray, histogram of the original image, in dark gray when contrast < 0 in Gimp
Note that these histograms have been obtained using the Brightness-Contrast tool in the Gimp software. The brightness tool should be identical to the β bias parameters but the contrast tool seems to differ to the α gain where the output range seems to be centered with Gimp (as you can notice in the previous histogram).

It can occur that playing with the β bias will improve the brightness but in the same time the image will appear with a slight veil as the contrast will be reduced. The α gain can be used to diminue this effect but due to the saturation, we will lose some details in the original bright regions.

Gamma correction

Gamma correction can be used to correct the brightness of an image by using a non linear transformation between the input values and the mapped output values:

O=(I255)γ×255

As this relation is non linear, the effect will not be the same for all the pixels and will depend to their original value.

Basic_Linear_Transform_Tutorial_gamma.png
Plot for different values of gamma

When γ<1, the original dark regions will be brighter and the histogram will be shifted to the right whereas it will be the opposite with γ>1.

Correct an underexposed image

The following image has been corrected with: α=1.3 and β=40.

Basic_Linear_Transform_Tutorial_linear_transform_correction.jpg

By Visem (Own work) [CC BY-SA 3.0], via Wikimedia Commons

The overall brightness has been improved but you can notice that the clouds are now greatly saturated due to the numerical saturation of the implementation used (highlight clipping in photography).

The following image has been corrected with: γ=0.4.

Basic_Linear_Transform_Tutorial_gamma_correction.jpg

By Visem (Own work) [CC BY-SA 3.0], via Wikimedia Commons

The gamma correction should tend to add less saturation effect as the mapping is non linear and there is no numerical saturation possible as in the previous method.

Basic_Linear_Transform_Tutorial_histogram_compare.png

Left: histogram after alpha, beta correction ; Center: histogram of the original image ; Right: histogram after the gamma correction

The previous figure compares the histograms for the three images (the y-ranges are not the same between the three histograms). You can notice that most of the pixel values are in the lower part of the histogram for the original image. After α, β correction, we can observe a big peak at 255 due to the saturation as well as a shift in the right. After gamma correction, the histogram is shifted to the right but the pixels in the dark regions are more shifted (see the gamma curves figure) than those in the bright regions.

In this tutorial, you have seen two simple methods to adjust the contrast and the brightness of an image. They are basic techniques and are not intended to be used as a replacement of a raster graphics editor!
