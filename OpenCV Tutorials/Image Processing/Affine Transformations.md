Goal

In this tutorial you will learn how to:

* Use the OpenCV function cv::warpAffine to implement simple remapping routines.
* Use the OpenCV function cv::getRotationMatrix2D to obtain a 2×3 rotation matrix

Theory

What is an Affine Transformation?

1.A transformation that can be expressed in the form of a matrix multiplication (linear transformation) followed by a vector addition (translation).
2.From the above, we can use an Affine Transformation to express:

    a.Rotations (linear transformation)
    b.Translations (vector addition)
    c.Scale operations (linear transformation)
you can see that, in essence, an Affine Transformation represents a relation between two images.

3.The usual way to represent an Affine Transformation is by using a 2×3 matrix.

![](http://latex.codecogs.com/gif.latex?A%20%3D%20%5Cbegin%7Bbmatrix%7D%20a_%7B00%7D%20%26%20a_%7B01%7D%20%5C%5C%20a_%7B10%7D%20%26%20a_%7B11%7D%20%5Cend%7Bbmatrix%7D_%7B2%20%5Ctimes%202%7D%20B%20%3D%20%5Cbegin%7Bbmatrix%7D%20b_%7B00%7D%20%5C%5C%20b_%7B10%7D%20%5Cend%7Bbmatrix%7D_%7B2%20%5Ctimes%201%7D)

![](http://latex.codecogs.com/gif.latex?M%20%3D%20%5Cbegin%7Bbmatrix%7D%20A%20%26%20B%20%5Cend%7Bbmatrix%7D%20%3D%20%5Cbegin%7Bbmatrix%7D%20a_%7B00%7D%20%26%20a_%7B01%7D%20%26%20b_%7B00%7D%20%5C%5C%20a_%7B10%7D%20%26%20a_%7B11%7D%20%26%20b_%7B10%7D%20%5Cend%7Bbmatrix%7D_%7B2%20%5Ctimes%203%7D)

Considering that we want to transform a 2D vector ![](http://latex.codecogs.com/gif.download?X%20%3D%20%5Cbegin%7Bbmatrix%7Dx%20%5C%5C%20y%5Cend%7Bbmatrix%7D) by using A and B, we can do the same with:

![](http://latex.codecogs.com/gif.latex?T%20%3D%20A%20%5Ccdot%20%5Cbegin%7Bbmatrix%7Dx%20%5C%5C%20y%5Cend%7Bbmatrix%7D%20+%20B) or ![](http://latex.codecogs.com/gif.latex?T%20%3D%20M%20%5Ccdot%20%5Bx%2C%20y%2C%201%5D%5E%7BT%7D)
![](http://latex.codecogs.com/gif.latex?T%20%3D%20%5Cbegin%7Bbmatrix%7D%20a_%7B00%7Dx%20+%20a_%7B01%7Dy%20+%20b_%7B00%7D%20%5C%5C%20a_%7B10%7Dx%20+%20a_%7B11%7Dy%20+%20b_%7B10%7D%20%5Cend%7Bbmatrix%7D)

How do we get an Affine Transformation?

1.We mentioned that an Affine Transformation is basically a relation between two images. The information about this relation can come, roughly, in two ways:

    a.We know both X and T and we also know that they are related. Then our task is to find M
    b.We know M and X. To obtain T we only need to apply T=M⋅X. Our information for M may be explicit (i.e. have the 2-by-3 matrix) or it can come as a geometric relation between points.

2.Let's explain this in a better way (b). Since M relates 2 images, we can analyze the simplest case in which it relates three points in both images. Look at the figure below:

![](https://docs.opencv.org/4.1.0/Warp_Affine_Tutorial_Theory_0.jpg)

the points 1, 2 and 3 (forming a triangle in image 1) are mapped into image 2, still forming a triangle, but now they have changed notoriously. If we find the Affine Transformation with these 3 points (you can choose them as you like), then we can apply this found relation to all the pixels in an image.