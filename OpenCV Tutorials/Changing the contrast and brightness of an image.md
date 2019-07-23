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
> 以下用例来自书籍Richard Szeliski《机器视觉：算法和应用》

图像处理

* 一个普通的图像操作方法是输入一个或多个图像并最后生成一个图像的函数。
* 图像转换可以是以下几种途径：
  * 点操作（像素转换）
  * 临近点（基于某一区域）操作
  
像素级转换

* 在这种图片处理转换中，每一个输出点的值和输入点的值之间都有一定的联系，而且只能与输入点像素值之间有联系（例如加，有的时候则是全局收集到信息或变量）。
* 这些操作的例子包括了亮度和对比度的调整以及颜色纠正和转换。

亮度和对比度调整

* Two commonly used point processes are multiplication and addition with a constant:

g(x)=αf(x)+β

* The parameters α>0 and β are often called the gain and bias parameters; sometimes these parameters are said to control contrast and brightness respectively.

* You can think of f(x) as the source image pixels and g(x) as the output image pixels. Then, more conveniently we can write the expression as:

g(i,j)=α⋅f(i,j)+β

where i and j indicates that the pixel is located in the i-th row and j-th column.

Practical example

In this paragraph, we will put into practice what we have learned to correct an underexposed image by adjusting the brightness and the contrast of the image. We will also see another technique to correct the brightness of an image called gamma correction.

Brightness and contrast adjustments

Increasing (/ decreasing) the β value will add (/ subtract) a constant value to every pixel. Pixel values outside of the [0 ; 255] range will be saturated (i.e. a pixel value higher (/ lesser) than 255 (/ 0) will be clamp to 255 (/ 0)).

Basic_Linear_Transform_Tutorial_hist_beta.png

In light gray, histogram of the original image, in dark gray when brightness = 80 in Gimp
The histogram represents for each color level the number of pixels with that color level. A dark image will have many pixels with low color value and thus the histogram will present a peak in his left part. When adding a constant bias, the histogram is shifted to the right as we have added a constant bias to all the pixels.

The α parameter will modify how the levels spread. If α<1, the color levels will be compressed and the result will be an image with less contrast.

Basic_Linear_Transform_Tutorial_hist_alpha.png

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
