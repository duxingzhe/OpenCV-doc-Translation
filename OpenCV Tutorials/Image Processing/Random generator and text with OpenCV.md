Goals

In this tutorial you will learn how to:

Use the Random Number generator class (cv::RNG ) and how to get a random number from a uniform distribution.
Display text on an OpenCV window by using the function cv::putText

Code

In the previous tutorial (Basic Drawing) we drew diverse geometric figures, giving as input parameters such as coordinates (in the form of cv::Point), color, thickness, etc. You might have noticed that we gave specific values for these arguments.

In this tutorial, we intend to use random values for the drawing parameters. Also, we intend to populate our image with a big number of geometric figures. Since we will be initializing them in a random fashion, this process will be automatic and made by using loops .

This code is in your OpenCV sample folder. Otherwise you can grab it from here