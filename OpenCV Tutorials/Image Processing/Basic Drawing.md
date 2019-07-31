Goals

In this tutorial you will learn how to:

* Draw a line by using the OpenCV function line()
* Draw an ellipse by using the OpenCV function ellipse()
* Draw a rectangle by using the OpenCV function rectangle()
* Draw a circle by using the OpenCV function circle()
* Draw a filled polygon by using the OpenCV function fillPoly()

OpenCV Theory

For this tutorial, we will heavily use two structures: cv::Point and cv::Scalar :

Point

It represents a 2D point, specified by its image coordinates x and y. We can define it as:

```
Point pt;
pt.x = 10;
pt.y = 8;
```

or

```
Point pt =  Point(10, 8);
```

Scalar

Represents a 4-element vector. The type Scalar is widely used in OpenCV for passing pixel values.
In this tutorial, we will use it extensively to represent BGR color values (3 parameters). It is not necessary to define the last argument if it is not going to be used.

Let's see an example, if we are asked for a color argument and we give:

```
Scalar( a, b, c )
```

We would be defining a BGR color such as: Blue = a, Green = b and Red = c