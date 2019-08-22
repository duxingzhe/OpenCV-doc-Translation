Goal

In this tutorial you will learn how to:

* Use the OpenCV function cv::split to divide an image into its correspondent planes.
* To calculate histograms of arrays of images by using the OpenCV function cv::calcHist
* To normalize an array by using the function cv::normalize

> Note
> 
> In the last tutorial (Histogram Equalization) we talked about a particular kind of histogram called Image histogram. Now we will considerate it in its more general concept. Read on!

What are histograms?

* Histograms are collected counts of data organized into a set of predefined bins
* When we say data we are not restricting it to be intensity values (as we saw in the previous Tutorial Histogram Equalization). The data collected can be whatever feature you find useful to describe your image.
* Let's see an example. Imagine that a Matrix contains information of an image (i.e. intensity in the range 0−255):

![](https://docs.opencv.org/Histogram_Calculation_Theory_Hist0.jpg)

* What happens if we want to count this data in an organized way? Since we know that the range of information value for this case is 256 values, we can segment our range in subparts (called bins) like:

![](http://latex.codecogs.com/gif.latex?%5Cbegin%7Barray%7D%7Bl%7D%20%5B0%2C%20255%5D%20%3D%20%7B%20%5B0%2C%2015%5D%20%5Ccup%20%5B16%2C%2031%5D%20%5Ccup%20....%5Ccup%20%5B240%2C255%5D%20%7D%20%5C%5C%20range%20%3D%20%7B%20bin_%7B1%7D%20%5Ccup%20bin_%7B2%7D%20%5Ccup%20....%5Ccup%20bin_%7Bn%20%3D%2015%7D%20%7D%20%5Cend%7Barray%7D)

and we can keep count of the number of pixels that fall in the range of each bini. Applying this to the example above we get the image below ( axis x represents the bins and axis y the number of pixels in each of them).

![](https://docs.opencv.org/Histogram_Calculation_Theory_Hist1.jpg)

* This was just a simple example of how an histogram works and why it is useful. An histogram can keep count not only of color intensities, but of whatever image features that we want to measure (i.e. gradients, directions, etc).
* Let's identify some parts of the histogram:

    1.dims: The number of parameters you want to collect data of. In our example, dims = 1 because we are only counting the intensity values of each pixel (in a greyscale image).
    2.bins: It is the number of subdivisions in each dim. In our example, bins = 16
    3.range: The limits for the values to be measured. In this case: range = [0,255]

* What if you want to count two features? In this case your resulting histogram would be a 3D plot (in which x and y would be binx and biny for each feature and z would be the number of counts for each combination of (![](http://latex.codecogs.com/gif.latex?%28bin_%7Bx%7D%2C%20bin_%7By%7D%29)). The same would apply for more features (of course it gets trickier).

What OpenCV offers you

For simple purposes, OpenCV implements the function cv::calcHist , which calculates the histogram of a set of arrays (usually images or image planes). It can operate with up to 32 dimensions. We will see it in the code below!