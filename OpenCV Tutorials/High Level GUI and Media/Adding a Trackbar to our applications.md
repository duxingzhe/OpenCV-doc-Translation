### Adding a Trackbar to our applications

* In the previous tutorials (about Adding (blending) two images using OpenCV and the Changing the contrast and brightness of an image!) you might have noted that we needed to give some input to our programs, such as α and beta. We accomplished that by entering this data using the Terminal.

* Well, it is time to use some fancy GUI tools. OpenCV provides some GUI utilities (highgui module) for you. An example of this is a Trackbar.

![](https://docs.opencv.org/Adding_Trackbars_Tutorial_Trackbar.png)

* In this tutorial we will just modify our two previous programs so that they get the input information from the trackbar.

#### Goals

In this tutorial you will learn how to:

* Add a Trackbar in an OpenCV window by using cv::createTrackbar
