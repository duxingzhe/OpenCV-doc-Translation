Goal

In this tutorial you will learn how to:

* Use the OpenCV function copyMakeBorder() to set the borders (extra padding to your image).

Theory

> Note
> 
> The explanation below belongs to the book Learning OpenCV by Bradski and Kaehler.

1.In our previous tutorial we learned to use convolution to operate on images. One problem that naturally arises is how to handle the boundaries. How can we convolve them if the evaluated points are at the edge of the image?

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