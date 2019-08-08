目标

在这篇教程中，你将学到如何：

* 使用OpenCV的pyrUp()和pyrDown()来降低或提高图像采样率。

理论

> 注意
> 
> 本教程例子来自Bradski and Kaehler的Learning OpenCV一书.

* 通常情况下我们需要对图像的大小进行转化。为了达到这一目的，我们有两种不同的方法：
    1.放大图片
    2.缩小图片
* 尽管在OpenCV里，这种几何变换（从字面上）是将图形的形状大小进行改变（我们会在之后的章节解释调整大小的方法）。在这一章我们会分析图像金字塔的用法，这种用法在很多视觉处理软件中有非常多的使用。

图像金字塔

* 图像金字塔是将一张原始图片进行缩放后叠加在一起的一系列图片。图片会不断地缩放直到达到终止目标。
* 通常情况下有两种实现图像金字塔的方法：
    * 高斯金字塔：用于缩放图像
    * 拉普拉斯金字塔：用于重构从金字塔下层提取出来的图片（通常分辨率较低），将他们放大
* 本篇教程，我们会使用高斯金字塔。

Gaussian Pyramid

* Imagine the pyramid as a set of layers in which the higher the layer, the smaller the size.

![](https://docs.opencv.org/4.1.0/Pyramids_Tutorial_Pyramid_Theory.png)

* Every layer is numbered from bottom to top, so layer (i+1) (denoted as ![](http://latex.codecogs.com/gif.latex?G_{i+1}) is smaller than layer i (![](http://latex.codecogs.com/gif.latex?G_{i})).
* To produce layer (i+1) in the Gaussian pyramid, we do the following:

  * Convolve ![](http://latex.codecogs.com/gif.latex?G_{i}) with a Gaussian kernel:

![](http://latex.codecogs.com/gif.latex?%5Cfrac%7B1%7D%7B16%7D%5Cbegin%7Bbmatrix%7D1%264%266%264%261%5C%5C4%2616%2624%2616%264%5C%5C6%2624%2636%2624%266%5C%5C4%2616%2624%2616%264%5C%5C1%264%266%264%261%5Cend%7Bbmatrix%7D)

    * Remove every even-numbered row and column.
* You can easily notice that the resulting image will be exactly one-quarter the area of its predecessor. Iterating this process on the input image ![](http://latex.codecogs.com/gif.latex?G_{0}) (original image) produces the entire pyramid.
*The procedure above was useful to downsample an image. What if we want to make it bigger?: columns filled with zeros ( 0)
    * First, upsize the image to twice the original in each dimension, with the new even rows and
    * Perform a convolution with the same kernel shown above (multiplied by 4) to approximate the values of the "missing pixels"
* These two procedures (downsampling and upsampling as explained above) are implemented by the OpenCV functions pyrUp() and pyrDown() , as we will see in an example with the code below:

> Note
> 
> When we reduce the size of an image, we are actually losing information of the image.