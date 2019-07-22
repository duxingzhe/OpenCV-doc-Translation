Goal

In this tutorial you will learn how to:

Access pixel values

* Initialize a matrix with zeros
* Learn what cv::saturate_cast does and why it is useful
* Get some cool info about pixel transformations
* Improve the brightness of an image on a practical example

Theory

> Note
>
> The explanation below belongs to the book Computer Vision: Algorithms and Applications by Richard Szeliski

Image Processing

* A general image processing operator is a function that takes one or more input images and produces an output image.
* Image transforms can be seen as:
  * Point operators (pixel transforms)
  * Neighborhood (area-based) operators
  
Pixel Transforms

* In this kind of image processing transform, each output pixel's value depends on only the corresponding input pixel value (plus, potentially, some globally collected information or parameters).
* Examples of such operators include brightness and contrast adjustments as well as color correction and transformations.

Brightness and contrast adjustments

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
