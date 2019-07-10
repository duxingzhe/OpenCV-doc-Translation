目标

我们能从真实世界的很多地方获得数码图片：数码相机、扫描仪、机器体层扫描仪和核磁共振仪等。在上述情形中，我们（人类）看到的是图片。但对于，我们的机器而言，我们记录的只不过是图像的各种各样像素点的数值而已。

![](https://docs.opencv.org/4.1.0/MatBasicImageForComputer.jpg)

就像上面的那张图片，这是一张汽车的图片，对于计算机而言，他就是一个矩形，矩形的元素代表的是密密麻麻像素点的数值。我们根据我们自身的需求去存储使用其中的值。但是最后，在计算机领域中，每一个张图像都要最终变成一个数值矩阵，还有一些其他能够描述矩阵本身的信息。OpenCV便是一个主要关注于如何处理和使用这些信息的库。因此，首先，你应该对OpenCV如何存储和处理这些图像有非常熟悉的了解。

Mat

自2001年起，OpenCV便开始开发。在当时，OpenCV是用C实现的，并提供C语言接口，存储在内存的方式是通过IplIMage的C语言结构体来实现的。因此，你会在一些比较老的教程或者是教科书里看到OpenCV的C语言使用方式。然而，C语言带来一个问题，那就是C语言本身做的事情非常有限。最大的问题是我们需要对内存进行手动管理。C语言假定程序员应该对内存分配和回收负责。当然，这对于一个小程序而言没有多大问题，但是，一旦代码开始膨胀，程序开发便会从目标功能实现转向异常繁琐的内存管理上去。

庆幸的是，C++开发出来，并带来了类的概念，这让用户可以自动化处理内存占用，从而简化内存管理。而且有一个好消息，C++对C语言是充分兼容的，所以将OpenCV改成C++并不会引发任何问题。因此，从OpenCV 2.0开始，我们便引入了新的C++接口，这样我们为用户提供了新的编程方法，从而让用户不必受困于内存管理的麻烦当中，让用户的代码更加简明扼要（写的更少，做的更多）。C++接口的一个弊端是许多嵌入式开发系统目前只支持C。因此，如果你在使用嵌入式系统，请使用C语言开发。如果你并不是使用嵌入式设备，则不要使用旧方法（除非你是一位喜欢给自己挖坑的程序员，你这样做就是在自找麻烦）。

你需要知道的第一件事情便是：使用了Mat之后，你就不需要手动分配内存和释放内存。许多情况下，OpenCV函数可以自动分配内存来存放输出数据，尽管有的时候你还是需要自己手动分配一下。如果你传递了一个已经存在的Mat对象，而且这个Mat对象已经为矩阵分配了需要的空间，那么就有了额外的好处，这部分内存空间会被重复使用。也就是说，我们在处理任务的时候，始终只用到了我们需要的空间。

Mat通常情况下是一个有两个数据部分组成的类：矩阵头部（包括了矩阵的大小、存储所使用的方法、矩阵存储的位置等等信息）和一个指向矩阵中所包含元素的指针（矩阵的维数取决于所用的存储方式）。矩阵头部大小是一定的，然而矩阵的大小与图像有关，通常而言，维数越大，矩阵的规模也越大。

OpenCV是一个图形处理库。它包含了一系列的图像处理函数。为了解决计算处理问题，许多时候你都需要使用库里的多个函数。正因为如此，将图像数据传递给函数是一个常见的处理方式。我们牢记的是，我们是在讨论图像处理算法，也就是说，需要大量的计算处理过程。我们需要提醒的最后一件事是，对图像进行非必要的复制会对你的程序性能有相当大的影响，会严重减慢你的程序运行速度。

为了解决这个问题，OpenCV引入了引用技术系统。这个解决方案由以下算法实现，Mat对象有自身的头部，但是矩阵本身可以被两个实例共享，共享的方式是他们的矩阵指针指向同一个地址。还有，复制操作符将会只是复制头部和指向大型矩阵的指针，而不是数据本身。

```
Mat A, C;                          // creates just the header parts
A = imread(argv[1], IMREAD_COLOR); // here we'll know the method used (allocate matrix)
Mat B(A);                                 // Use the copy constructor
C = A;                                    // Assignment operator
```

在上面代码的最后，以上所提及的几个对象都是指向同一个数据矩阵。然而，他们的头部是不一样的，对其中一个对象进行数据修改会影响其他对象的数据。在实际操作过程中，程序员需要用不同的函数对同样的数据处理。尽管如此，他们的头部是不一样的。并且，比较有趣的是，你可以创建一个包含了部分矩阵数据的头部。例如，为了创建图片的感兴趣区域（Region of Interest），你可以新建有新边界的新头部。

```
Mat D (A, Rect(10, 10, 100, 100) ); // using a rectangle
Mat E = A(Range::all(), Range(1,3)); // using row and column boundaries
```

Now you may ask if the matrix itself may belong to multiple Mat objects who takes responsibility for cleaning it up when it's no longer needed. The short answer is: the last object that used it. This is handled by using a reference counting mechanism. Whenever somebody copies a header of a Mat object, a counter is increased for the matrix. Whenever a header is cleaned this counter is decreased. When the counter reaches zero the matrix too is freed. Sometimes you will want to copy the matrix itself too, so OpenCV provides the cv::Mat::clone() and cv::Mat::copyTo() functions.

```
Mat F = A.clone();
Mat G;
A.copyTo(G);
```

Now modifying F or G will not affect the matrix pointed by the Mat header. What you need to remember from all this is that:

* Output image allocation for OpenCV functions is automatic (unless specified otherwise).
* You do not need to think about memory management with OpenCVs C++ interface.
* The assignment operator and the copy constructor only copies the header.
* The underlying matrix of an image may be copied using the cv::Mat::clone() and cv::Mat::copyTo() functions.

Storing methods

This is about how you store the pixel values. You can select the color space and the data type used. The color space refers to how we combine color components in order to code a given color. The simplest one is the gray scale where the colors at our disposal are black and white. The combination of these allows us to create many shades of gray.

For colorful ways we have a lot more methods to choose from. Each of them breaks it down to three or four basic components and we can use the combination of these to create the others. The most popular one is RGB, mainly because this is also how our eye builds up colors. Its base colors are red, green and blue. To code the transparency of a color sometimes a fourth element: alpha (A) is added.

There are, however, many other color systems each with their own advantages:

* RGB is the most common as our eyes use something similar, however keep in mind that OpenCV standard display system composes colors using the BGR color space (a switch of the red and blue channel).
* The HSV and HLS decompose colors into their hue, saturation and value/luminance components, which is a more natural way for us to describe colors. You might, for example, dismiss the last component, making your algorithm less sensible to the light conditions of the input image.
* YCrCb is used by the popular JPEG image format.
* CIE L*a*b* is a perceptually uniform color space, which comes handy if you need to measure the distance of a given color to another color.

Each of the building components has their own valid domains. This leads to the data type used. How we store a component defines the control we have over its domain. The smallest data type possible is char, which means one byte or 8 bits. This may be unsigned (so can store values from 0 to 255) or signed (values from -127 to +127). Although in case of three components this already gives 16 million possible colors to represent (like in case of RGB) we may acquire an even finer control by using the float (4 byte = 32 bit) or double (8 byte = 64 bit) data types for each component. Nevertheless, remember that increasing the size of a component also increases the size of the whole picture in the memory.

Creating a Mat object explicitly

In the Load, Modify, and Save an Image tutorial you have already learned how to write a matrix to an image file by using the cv::imwrite() function. However, for debugging purposes it's much more convenient to see the actual values. You can do this using the << operator of Mat. Be aware that this only works for two dimensional matrices.

Although Mat works really well as an image container, it is also a general matrix class. Therefore, it is possible to create and manipulate multidimensional matrices. You can create a Mat object in multiple ways:

* cv::Mat::Mat Constructor

>    
>   ```
>   Mat M(2,2, CV_8UC3, Scalar(0,0,255));
>   cout << "M = " << endl << " " << M << endl << endl;
>   ```

![](https://docs.opencv.org/4.1.0/MatBasicContainerOut1.png)

For two dimensional and multichannel images we first define their size: row and column count wise.

Then we need to specify the data type to use for storing the elements and the number of channels per matrix point. To do this we have multiple definitions constructed according to the following convention:

CV_[The number of bits per item][Signed or Unsigned][Type Prefix]C[The channel number]
For instance, CV_8UC3 means we use unsigned char types that are 8 bit long and each pixel has three of these to form the three channels. This are predefined for up to four channel numbers. The cv::Scalar is four element short vector. Specify this and you can initialize all matrix points with a custom value. If you need more you can create the type with the upper macro, setting the channel number in parenthesis as you can see below.

Use C/C++ arrays and initialize via constructor

    int sz[3] = {2,2,2};
    Mat L(3,sz, CV_8UC(1), Scalar::all(0));
The upper example shows how to create a matrix with more than two dimensions. Specify its dimension, then pass a pointer containing the size for each dimension and the rest remains the same.

cv::Mat::create function:

    M.create(4,4, CV_8UC(2));
    cout << "M = "<< endl << " "  << M << endl << endl;
MatBasicContainerOut2.png
You cannot initialize the matrix values with this construction. It will only reallocate its matrix data memory if the new size will not fit into the old one.

MATLAB style initializer: cv::Mat::zeros , cv::Mat::ones , cv::Mat::eye . Specify size and data type to use:

    Mat E = Mat::eye(4, 4, CV_64F);
    cout << "E = " << endl << " " << E << endl << endl;
    Mat O = Mat::ones(2, 2, CV_32F);
    cout << "O = " << endl << " " << O << endl << endl;
    Mat Z = Mat::zeros(3,3, CV_8UC1);
    cout << "Z = " << endl << " " << Z << endl << endl;
MatBasicContainerOut3.png
For small matrices you may use comma separated initializers or initializer lists (C++11 support is required in the last case):

    Mat C = (Mat_<double>(3,3) << 0, -1, 0, -1, 5, -1, 0, -1, 0);
    cout << "C = " << endl << " " << C << endl << endl;
    C = (Mat_<double>({0, -1, 0, -1, 5, -1, 0, -1, 0})).reshape(3);
    cout << "C = " << endl << " " << C << endl << endl;
MatBasicContainerOut6.png
Create a new header for an existing Mat object and cv::Mat::clone or cv::Mat::copyTo it.

    Mat RowClone = C.row(1).clone();
    cout << "RowClone = " << endl << " " << RowClone << endl << endl;
MatBasicContainerOut7.png
Note
You can fill out a matrix with random values using the cv::randu() function. You need to give the lower and upper value for the random values:
    Mat R = Mat(3, 2, CV_8UC3);
    randu(R, Scalar::all(0), Scalar::all(255));
Output formatting
In the above examples you could see the default formatting option. OpenCV, however, allows you to format your matrix output:

Default
    cout << "R (default) = " << endl <<        R           << endl << endl;
MatBasicContainerOut8.png
Python
    cout << "R (python)  = " << endl << format(R, Formatter::FMT_PYTHON) << endl << endl;
MatBasicContainerOut16.png
Comma separated values (CSV)
    cout << "R (csv)     = " << endl << format(R, Formatter::FMT_CSV   ) << endl << endl;
MatBasicContainerOut10.png
Numpy
    cout << "R (numpy)   = " << endl << format(R, Formatter::FMT_NUMPY ) << endl << endl;
MatBasicContainerOut9.png
C
    cout << "R (c)       = " << endl << format(R, Formatter::FMT_C     ) << endl << endl;
MatBasicContainerOut11.png
Output of other common items
OpenCV offers support for output of other common OpenCV data structures too via the << operator:

2D Point
    Point2f P(5, 1);
    cout << "Point (2D) = " << P << endl << endl;
MatBasicContainerOut12.png
3D Point
    Point3f P3f(2, 6, 7);
    cout << "Point (3D) = " << P3f << endl << endl;
MatBasicContainerOut13.png
std::vector via cv::Mat
    vector<float> v;
    v.push_back( (float)CV_PI);   v.push_back(2);    v.push_back(3.01f);
    cout << "Vector of floats via Mat = " << Mat(v) << endl << endl;
MatBasicContainerOut14.png
std::vector of points
    vector<Point2f> vPoints(20);
    for (size_t i = 0; i < vPoints.size(); ++i)
        vPoints[i] = Point2f((float)(i * 5), (float)(i % 7));
    cout << "A vector of 2D Points = " << vPoints << endl << endl;
MatBasicContainerOut15.png
Most of the samples here have been included in a small console application. You can download it from here or in the core section of the cpp samples.

You can also find a quick video demonstration of this on YouTube.

