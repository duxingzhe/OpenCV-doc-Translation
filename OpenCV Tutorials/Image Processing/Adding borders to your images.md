目的

在这篇教程中，你将会学到：

* 使用OpenCV copyMakeBorder()函数来设置边框（额外的图像边距）

理论

> 注意
> 
> 本文例子来源于Bradski和Kaehler所著的Learning OpenCV一书。

1.在之前的教程中，我们学会了如何使用卷积的方式处理图片。但是，很自然地我们还有其他问题，如何处理图像的边框？如果我们需要处理的图像像素位于图像的边界上，我们应该如何处理？

2.What most of OpenCV functions do is to copy a given image onto another slightly larger image and then automatically pads the boundary (by any of the methods explained in the sample code just below). This way, the convolution can be performed over the needed pixels without problems (the extra padding is cut after the operation is done).

3.In this tutorial, we will briefly explore two ways of defining the extra padding (border) for an image:

    * BORDER_CONSTANT: Pad the image with a constant value (i.e. black or 0
    * BORDER_REPLICATE: The row or column at the very edge of the original is replicated to the extra border.
    
This will be seen more clearly in the Code section.

* What does this program do?

    * Load an image
    * Let the user choose what kind of padding use in the input image. There are two options:
        
        1.Constant value border: Applies a padding of a constant value for the whole border. This value will be updated randomly each 0.5 seconds.
        2.Replicated border: The border will be replicated from the pixel values at the edges of the original image.
    
    The user chooses either option by pressing 'c' (constant) or 'r' (replicate)

    * The program finishes when the user presses 'ESC'