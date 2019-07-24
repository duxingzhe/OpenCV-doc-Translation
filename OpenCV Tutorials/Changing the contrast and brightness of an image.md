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

我们需要注意的是，直方图已经包含在Gimp中的亮度-对比度工具。亮度工具应该和偏正变量β一致，但是对比度工具似乎和修正值α有所不同，我们可以通过GIMP看出，修改α的值，让直方图的分布更趋向于中间数值（对比之前的直方图）。

更改偏正变量值β会增加亮度，但与此同时，图片会变得有点朦胧，这是因为对比度会有所下降。修正值α减弱这种影响，但是这种影响的减弱是因为饱和度修正，我们会丢掉原本图像在亮度区域方面的细节。

伽马修正

伽马修正可以用来修正图片的亮度，这种修正方式是在输入值和输出值之间进行非线性变换。

![](http://latex.codecogs.com/gif.latex?O=(I255)\gamma*255)

由于这个关系是非线性的，对于每一个像素而言，伽马修正的影响也有所不同，最终的值决定于他们的初始值。

![](https://docs.opencv.org/4.1.0/Basic_Linear_Transform_Tutorial_gamma.png)

Plot for different values of gamma

当γ<1，原来比较黑的区域就会变得亮一点，直方图变回移动到右边，γ>1则向右移动。

对曝光不足的图像进行修正

以下图片通过给定值进行了修正：α=1.3和β=40。

![](https://docs.opencv.org/4.1.0/Basic_Linear_Transform_Tutorial_linear_transform_correction.jpg)

By Visem (Own work) [CC BY-SA 3.0], via Wikimedia Commons

总体的亮度已经得到了提高，但是你会发现白云变得过饱和，这便是因为我们使用的是数值饱和算法。

下面的图片是通过给定值进行的修正: γ=0.4.

![](https://docs.opencv.org/4.1.0/Basic_Linear_Transform_Tutorial_gamma_correction.jpg)

By Visem (Own work) [CC BY-SA 3.0], via Wikimedia Commons

伽马修正看上去就只是加上了少许的饱和效果，因为我们是通过非线性函数进行的处理，就不存在之前的数值饱和修复的情况。

![](https://docs.opencv.org/4.1.0/Basic_Linear_Transform_Tutorial_histogram_compare.png)

Left: histogram after alpha, beta correction ; Center: histogram of the original image ; Right: histogram after the gamma correction

The previous figure compares the histograms for the three images (the y-ranges are not the same between the three histograms). You can notice that most of the pixel values are in the lower part of the histogram for the original image. After α, β correction, we can observe a big peak at 255 due to the saturation as well as a shift in the right. After gamma correction, the histogram is shifted to the right but the pixels in the dark regions are more shifted (see the gamma curves figure) than those in the bright regions.

In this tutorial, you have seen two simple methods to adjust the contrast and the brightness of an image. They are basic techniques and are not intended to be used as a replacement of a raster graphics editor!
