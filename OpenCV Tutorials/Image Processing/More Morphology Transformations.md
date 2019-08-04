Goal

In this tutorial you will learn how to:

* Use the OpenCV function cv::morphologyEx to apply Morphological Transformation such as:
    * Opening
    * Closing
    * Morphological Gradient
    * Top Hat
    * Black Hat

Theory

> Note
> 
> The explanation below belongs to the book Learning OpenCV by Bradski and Kaehler.

In the previous tutorial we covered two basic Morphology operations:

* Erosion
* Dilation.

Based on these two we can effectuate more sophisticated transformations to our images. Here we discuss briefly 5 operations offered by OpenCV:

Opening

* It is obtained by the erosion of an image followed by a dilation.

![](http://latex.codecogs.com/gif.latex?dst=open(src,element)=dilate(erode(src,element)))

* Useful for removing small objects (it is assumed that the objects are bright on a dark foreground)
* For instance, check out the example below. The image at the left is the original and the image at the right is the result after applying the opening transformation. We can observe that the small dots have disappeared.

![](https://docs.opencv.org/4.1.0/Morphology_2_Tutorial_Theory_Opening.png)

Closing

* It is obtained by the dilation of an image followed by an erosion.

![](http://latex.codecogs.com/gif.latex?dst=close(src,element)=erode(dilate(src,element)))

* Useful to remove small holes (dark regions).

![](https://docs.opencv.org/4.1.0/Morphology_2_Tutorial_Theory_Closing.png)

Morphological Gradient

* It is the difference between the dilation and the erosion of an image.

![](http://latex.codecogs.com/gif.latex?dst=morph_{grad}(src,element)=dilate(src,element)-erode(src,element))

* It is useful for finding the outline of an object as can be seen below:

![](https://docs.opencv.org/4.1.0/Morphology_2_Tutorial_Theory_Gradient.png)

Top Hat

* It is the difference between an input image and its opening.

![](http://latex.codecogs.com/gif.latex?dst=tophat(src,element)=src-open(src,element))

Morphology_2_Tutorial_Theory_TopHat.png

Black Hat

* It is the difference between the closing and its input image

![](http://latex.codecogs.com/gif.latex?dst=blackhat(src,element)=close(src,element)-src)

![](https://docs.opencv.org/4.1.0/Morphology_2_Tutorial_Theory_BlackHat.png)