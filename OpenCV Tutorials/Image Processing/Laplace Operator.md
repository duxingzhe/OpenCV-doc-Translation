Goal

In this tutorial you will learn how to:

Use the OpenCV function Laplacian() to implement a discrete analog of the Laplacian operator.

Theory

In the previous tutorial we learned how to use the Sobel Operator. It was based on the fact that in the edge area, the pixel intensity shows a "jump" or a high variation of intensity. Getting the first derivative of the intensity, we observed that an edge is characterized by a maximum, as it can be seen in the figure:

![](https://docs.opencv.org/4.1.0/Laplace_Operator_Tutorial_Theory_Previous.jpg)

And...what happens if we take the second derivative?

![](https://docs.opencv.org/4.1.0/Laplace_Operator_Tutorial_Theory_ddIntensity.jpg)

You can observe that the second derivative is zero! So, we can also use this criterion to attempt to detect edges in an image. However, note that zeros will not only appear in edges (they can actually appear in other meaningless locations); this can be solved by applying filtering where needed.

Laplacian Operator

From the explanation above, we deduce that the second derivative can be used to detect edges. Since images are "\*2D\*", we would need to take the derivative in both dimensions. Here, the Laplacian operator comes handy.
The Laplacian operator is defined by:

![](http://latex.codecogs.com/gif.latex?Laplace(f)=\dfrac{\partial^{2}f}{\partialx^{2}}+\dfrac{\partial^{2}f}{\partial{y}^{2}})

The Laplacian operator is implemented in OpenCV by the function Laplacian() . In fact, since the Laplacian uses the gradient of images, it calls internally the Sobel operator to perform its computation.