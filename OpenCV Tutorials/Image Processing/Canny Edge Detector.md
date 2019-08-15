Goal

In this tutorial you will learn how to:

* Use the OpenCV function cv::Canny to implement the Canny Edge Detector.

Theory

The Canny Edge detector [34] was developed by John F. Canny in 1986. Also known to many as the optimal detector, the Canny algorithm aims to satisfy three main criteria:

* Low error rate: Meaning a good detection of only existent edges.
* Good localization: The distance between edge pixels detected and real edge pixels have to be minimized.
* Minimal response: Only one detector response per edge.

Steps

1.Filter out any noise. The Gaussian filter is used for this purpose. An example of a Gaussian kernel of size=5 that might be used is shown below:

![](http://latex.codecogs.com/gif.latex?K%20%3D%20%5Cdfrac%7B1%7D%7B159%7D%5Cbegin%7Bbmatrix%7D%202%20%26%204%20%26%205%20%26%204%20%26%202%20%5C%5C%204%20%26%209%20%26%2012%20%26%209%20%26%204%20%5C%5C%205%20%26%2012%20%26%2015%20%26%2012%20%26%205%20%5C%5C%204%20%26%209%20%26%2012%20%26%209%20%26%204%20%5C%5C%202%20%26%204%20%26%205%20%26%204%20%26%202%20%5Cend%7Bbmatrix%7D)

2.Find the intensity gradient of the image. For this, we follow a procedure analogous to Sobel:
    a.Apply a pair of convolution masks (in x and y directions:

    ![](http://latex.codecogs.com/gif.latex?G_%7Bx%7D%20%3D%20%5Cbegin%7Bbmatrix%7D%20-1%20%26%200%20%26%20+1%20%5C%5C%20-2%20%26%200%20%26%20+2%20%5C%5C%20-1%20%26%200%20%26%20+1%20%5Cend%7Bbmatrix%7D)

    ![](http://latex.codecogs.com/gif.download?G_%7By%7D%20%3D%20%5Cbegin%7Bbmatrix%7D%20-1%20%26%20-2%20%26%20-1%20%5C%5C%200%20%26%200%20%26%200%20%5C%5C%20+1%20%26%20+2%20%26%20+1%20%5Cend%7Bbmatrix%7D)

    b.Find the gradient strength and direction with:

    ![](http://latex.codecogs.com/gif.download?%5Cbegin%7Barray%7D%7Bl%7D%20G%20%3D%20%5Csqrt%7B%20G_%7Bx%7D%5E%7B2%7D%20+%20G_%7By%7D%5E%7B2%7D%20%7D%20%5C%5C%20%5Ctheta%20%3D%20%5Carctan%28%5Cdfrac%7B%20G_%7By%7D%20%7D%7B%20G_%7Bx%7D%20%7D%29%20%5Cend%7Barray%7D)

3.Non-maximum suppression is applied. This removes pixels that are not considered to be part of an edge. Hence, only thin lines (candidate edges) will remain.

4.Hysteresis: The final step. Canny does use two thresholds (upper and lower):

    a.If a pixel gradient is higher than the upper threshold, the pixel is accepted as an edge
    b.If a pixel gradient value is below the lower threshold, then it is rejected.
    c.If the pixel gradient is between the two thresholds, then it will be accepted only if it is connected to a pixel that is above the upper threshold.

Canny recommended a upper:lower ratio between 2:1 and 3:1.

5.For more details, you can always consult your favorite Computer Vision book.