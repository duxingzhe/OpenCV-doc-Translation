Goal

In this tutorial you will learn how to:

* Use the OpenCV functions pyrUp() and pyrDown() to downsample or upsample a given image.

Theory

> Note
> 
> The explanation below belongs to the book Learning OpenCV by Bradski and Kaehler.

* Usually we need to convert an image to a size different than its original. For this, there are two possible options:
    1.Upsize the image (zoom in) or
    2.Downsize it (zoom out).
* Although there is a geometric transformation function in OpenCV that -literally- resize an image (resize, which we will show in a future tutorial), in this section we analyze first the use of Image Pyramids, which are widely applied in a huge range of vision applications.

Image Pyramid

* An image pyramid is a collection of images - all arising from a single original image - that are successively downsampled until some desired stopping point is reached.
* There are two common kinds of image pyramids:
    * Gaussian pyramid: Used to downsample images
    * Laplacian pyramid: Used to reconstruct an upsampled image from an image lower in the pyramid (with less resolution)
* In this tutorial we'll use the Gaussian pyramid.

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