Goal

In this tutorial you will learn:


* hat the gradient structure tensor is
* how to estimate orientation and coherency of an anisotropic image by a gradient structure tensor
* how to segment an anisotropic image with a single local orientation by a gradient structure tensor

Theory

> Note
> 
> The explanation is based on the books [100], [18] and [220]. Good physical explanation of a gradient structure tensor is given in [241]. Also, you can refer to a wikipedia page Structure tensor.
> A anisotropic image on this page is a real world image.

What is the gradient structure tensor?

In mathematics, the gradient structure tensor (also referred to as the second-moment matrix, the second order moment tensor, the inertia tensor, etc.) is a matrix derived from the gradient of a function. It summarizes the predominant directions of the gradient in a specified neighborhood of a point, and the degree to which those directions are coherent (coherency). The gradient structure tensor is widely used in image processing and computer vision for 2D/3D image segmentation, motion detection, adaptive filtration, local image features detection, etc.

Important features of anisotropic images include orientation and coherency of a local anisotropy. In this paper we will show how to estimate orientation and coherency, and how to segment an anisotropic image with a single local orientation by a gradient structure tensor.

The gradient structure tensor of an image is a 2x2 symmetric matrix. Eigenvectors of the gradient structure tensor indicate local orientation, whereas eigenvalues give coherency (a measure of anisotropism).

The gradient structure tensor J of an image Z can be written as:

![](http://latex.codecogs.com/gif.latex?J%20%3D%20%5Cbegin%7Bbmatrix%7D%20J_%7B11%7D%20%26%20J_%7B12%7D%20%5C%5C%20J_%7B12%7D%20%26%20J_%7B22%7D%20%5Cend%7Bbmatrix%7D)

where ![](http://latex.codecogs.com/gif.latex?J_%7B11%7D%20%3D%20M%5BZ_%7Bx%7D%5E%7B2%7D%5D) , ![](http://latex.codecogs.com/gif.latex?J_%7B22%7D%20%3D%20M%5BZ_%7By%7D%5E%7B2%7D%5D) , ![](http://latex.codecogs.com/gif.latex?J_%7B12%7D%20%3D%20M%5BZ_%7Bx%7DZ_%7By%7D%5D) - components of the tensor, M[] is a symbol of mathematical expectation (we can consider this operation as averaging in a window w), Zx and Zy are partial derivatives of an image Z with respect to x and y.

The eigenvalues of the tensor can be found in the below formula:

![](http://latex.codecogs.com/gif.latex?%5Clambda_%7B1%2C2%7D%20%3D%20J_%7B11%7D%20+%20J_%7B22%7D%20%5Cpm%20%5Csqrt%7B%28J_%7B11%7D%20-%20J_%7B22%7D%29%5E%7B2%7D%20+%204J_%7B12%7D%5E%7B2%7D%7D)

where ![](http://latex.codecogs.com/gif.latex?\lambda_1) - largest eigenvalue, ![](http://latex.codecogs.com/gif.latex?\lambda_2) - smallest eigenvalue.

How to estimate orientation and coherency of an anisotropic image by gradient structure tensor?
The orientation of an anisotropic image:

![](http://latex.codecogs.com/gif.latex?%5Calpha%20%3D%200.5arctg%5Cfrac%7B2J_%7B12%7D%7D%7BJ_%7B22%7D%20-%20J_%7B11%7D%7D)

Coherency:

![](http://latex.codecogs.com/gif.latex?C%20%3D%20%5Cfrac%7B%5Clambda_1%20-%20%5Clambda_2%7D%7B%5Clambda_1%20+%20%5Clambda_2%7D)

The coherency ranges from 0 to 1. For ideal local orientation ( ![](http://latex.codecogs.com/gif.latex?\lambda_2) = 0, ![](http://latex.codecogs.com/gif.latex?\lambda_1) > 0) it is one, for an isotropic gray value structure ( λ1 = λ2 > 0) it is zero.