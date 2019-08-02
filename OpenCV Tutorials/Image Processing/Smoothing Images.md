Goal 

In this tutorial you will learn how to apply diverse linear filters to smooth images using OpenCV functions such as:

* blur()
* GaussianBlur()
* medianBlur()
* bilateralFilter()

Theory 

> Note
>
> The explanation below belongs to the book Computer Vision: Algorithms and Applications by Richard Szeliski and to LearningOpenCV

* Smoothing, also called blurring, is a simple and frequently used image processing operation.
* There are many reasons for smoothing. In this tutorial we will focus on smoothing in order to reduce noise (other uses will be seen in the following tutorials).
* To perform a smoothing operation we will apply a filter to our image. The most common type of filters are linear, in which an output pixel's value (i.e. ![](http://latex.codecogs.com/gif.latex?g(i,j))) is determined as a weighted sum of input pixel values (i.e. ![](http://latex.codecogs.com/gif.latex?f(i+k,j+l))) :

![](http://latex.codecogs.com/gif.latex?g(i,j)=\sum_{k,l}f(i+k,j+l)h(k,l))

![](http://latex.codecogs.com/gif.latex?h(k,l)) is called the kernel, which is nothing more than the coefficients of the filter.

It helps to visualize a filter as a window of coefficients sliding across the image.

* There are many kind of filters, here we will mention the most used:

Normalized Box Filter

* This filter is the simplest of all! Each output pixel is the mean of its kernel neighbors ( all of them contribute with equal weights)

* The kernel is below:

![](http://latex.codecogs.com/gif.latex?K%3D%5Cdfrac%7B1%7D%7BK_%7Bwidth%7D%5Ccdot%7BK_%7Bheight%7D%7D%7D%5Cbegin%7Bbmatrix%7D1%261%261%26...%261%5C%5C1%261%261%26...%261%5C%5C.%26.%26.%26...%26%201%5C%5C.%26.%26.%26...%261%5C%5C1%261%261%26...%261%5Cend%7Bbmatrix%7D)

Gaussian Filter

* Probably the most useful filter (although not the fastest). Gaussian filtering is done by convolving each point in the input array with a Gaussian kernel and then summing them all to produce the output array.

* Just to make the picture clearer, remember how a 1D Gaussian kernel look like?
