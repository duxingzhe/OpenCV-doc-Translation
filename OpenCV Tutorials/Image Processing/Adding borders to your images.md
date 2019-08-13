目的

在这篇教程中，你将会学到：

* 使用OpenCV copyMakeBorder()函数来设置边框（额外的图像边距）

理论

> 注意
> 
> 本文例子来源于Bradski和Kaehler所著的Learning OpenCV一书。

1.在之前的教程中，我们学会了如何使用卷积的方式处理图片。但是，很自然地我们还有其他问题，如何处理图像的边框？如果我们需要处理的图像像素位于图像的边界上，我们应该如何处理？

2.What most of OpenCV functions do is to copy a given image onto another slightly larger image and then automatically pads the boundary (by any of the methods explained in the sample code just below). This way, the convolution can be performed over the needed pixels without problems (the extra padding is cut after the operation is done).

3.在这篇教程中，我们将简单使用两种确认图像额外边界的方法：

    * BORDER_CONSTANT: 图像边界使用常量值进行操作（例如黑色或0）
    * BORDER_REPLICATE: 原始图像的行或列通过额外边界来进行复制。
    
这些概念在代码部分可以更加直观的感受到。

* 这些代码能做什么？

    * 加载一张图片
    * 允许用户使用各种各样的值来对图像的边界进行边框处理。这里有两个选项：
        
        1.Constant value border: Applies a padding of a constant value for the whole border. This value will be updated randomly each 0.5 seconds.
        2.Replicated border: The border will be replicated from the pixel values at the edges of the original image.
    
    The user chooses either option by pressing 'c' (constant) or 'r' (replicate)

    * The program finishes when the user presses 'ESC'