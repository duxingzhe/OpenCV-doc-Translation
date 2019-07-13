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

为了解决这个问题，OpenCV引入了引用计数系统。这个解决方案由以下算法实现，Mat对象有自身的头部，但是矩阵本身可以被两个实例共享，共享的方式是他们的矩阵指针指向同一个地址。还有，复制操作符将会只是复制头部和指向大型矩阵的指针，而不是数据本身。

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

你可能会问，如果矩阵自身属于多个Mat对象，那么当矩阵不再被使用的时候，谁负责清理？答案很简单：最后一个使用它的人。这也就是应用到了引用计数机制。只要建立了一个新的Mat对象头部引用，矩阵的计数便会增加一。当头部引用被清除以后，矩阵计数减一。当引用计数变成了零，矩阵便会被清除。有时候，你也希望复制整个矩阵，所以OpenCV提供了cv::Mat::clone()和cv::Mat::copyTo()函数。

```
Mat F = A.clone();
Mat G;
A.copyTo(G);
```

那么现在，如果你修改了F或者G的中的矩阵数据，对于A指针指向的数据而言是不受影响的。无论何时何地，你都要记住以下几个要点：

* 由OpenCV函数输出的图像，其内存空间由OpenCV自动负责。（除非文档特别指明的特殊情况。）
* 如果你用了OpenCV的C++接口，你完全不用担心内存管理问题。
* 分配操作符和复制构建函数只复制头部数据。
* cv::Mat::clone()和cv::Mat::copyTo()会复制图形的底层矩阵数据。

存储方式

这里将告诉你如何存储像素信息。你可以选择你想使用的色彩空间和数据类型。色域表示的是我们对给定颜色的描述方式。最简单的色域便是灰阶，在这个色彩空间中，我们只用黑和白作为基本颜色来表示颜色的深浅。黑白的混合就产生了各种各样的灰色。

身处这个五彩斑斓的世界，我们有许多方式来表示现实世界的颜色。每个色彩空间中，我们都会定义三或四个基本元素，然后我们便通过这三四种颜色的组合来表示其他颜色。最出名的色域便是RGB，这是因为这就是我们所见到的颜色。它基于红色、绿色和蓝色的混合。为了表示颜色的透明程度，我们会加上第四个元素：alpha（A）。

然而，不同的色域有着自身独特的优势：

* RGB是最常用的，因为他们就是我们眼睛所见的色域。但是，请记住一点，OpenCV标准显示系统使用BGR色域来组成颜色的（该色域将红和蓝通道进行了对调）。
* HSV和HLS将颜色分解为色度、饱和度、亮度等元素，因为这是我们日常生活中描述颜色的方式。比如说，你可能会忽略最后一个元素，这样可以使你的算法忽视图像亮度的改变。
* 较为流行的JPEG图像格式使用的是YCrCb色域。
* CIE L\*a\*b\* 是一个感性的统一色域，他主要是在你需要描述两种颜色值差距的时候使用，而且非常方便。

每一种色域都有自己擅长的区域。这取决于程序员使用的数据类型。我们存储数据的方式决定了我们使用的色域类型。最有可能的最简单数据类型可能是char，这个类型只有1个字节或8个位。可能是无符号类型（所以我们可以存储0-255）或者有符号类型（-127-127）。尽管在之前的三种色域都能支持1600万种颜色（比如RGB），我们可能会要求的更为严格，以便更好地控制颜色的输出。这时，我们会使用float（4字节=32位）或者double（8字节=64位）来表示各个元素。无论如何都要记住，增加元素的存储体积就会增加整个图片在内存中的体积。

显式地创建Mat对象

在加载、修改并保存图像的教程里，你已经知道通过cv::imwrite()函数将一个矩阵中所包含的值写入一个图形文件里。然而，在调试的过程当中，我们也需要看到运行当中的真实数值，这会让我们的程序调试更加方便。你可以通过使用Mat的<<操作符来实现这个任务。但是需要注意的是，这个操作符只能用在二维矩阵中。

尽管Mat作为一个图形容器时运行的效果还是不错的，但是他也是同样是一个通用的矩阵类。因此，我们可以用他来生成多维矩阵。你可以用很多方法来生成矩阵：

* cv::Mat::Mat Constructor

>    
>   ```
>   Mat M(2,2, CV_8UC3, Scalar(0,0,255));
>   cout << "M = " << endl << " " << M << endl << endl;
>   ```

![](https://docs.opencv.org/4.1.0/MatBasicContainerOut1.png)

对于二维并且多通道图形，我们首先要定义它的大小：按行数和列数记。

接着我们应该指明需要使用的数据类型，这样我们才能正确存储每个矩阵点的元素和通道的数量。为了实现这一目的，我们通过以下规范来对图形类型进行多重定义：

```
CV_[每一个元素的位数][有符号或无符号][类型前缀]C[通道数]
```

例如，CV_8UC3表示我们使用的是无符号数据类型，8位长，每个像素有3个通道。这个可以最多定义4个像素通道。cv::Scalar是四元素短向量。指明这一点，你就能将所有的矩阵点初始化成一个特定的值。如果你需要更多的信息，你可以创建大写宏的类型，就像插入语那样设置通道数。如下所示：

通过构造函数和C/C++数组进行初始化

```
int sz[3] = {2,2,2};
Mat L(3,sz, CV_8UC(1), Scalar::all(0));
```

上面的例子说明了如何创建一个大于二维的矩阵。先指明维度，再传入一个包含每个维度大小的指针，其他的则保持一致。

* cv::Mat::create函数:

```
M.create(4,4, CV_8UC(2));
cout << "M = "<< endl << " "  << M << endl << endl;
```

![](https://docs.opencv.org/4.1.0/MatBasicContainerOut2.png)

你不能通过这个命令来对矩阵的值进行初始化。只有当矩阵大小已经无法满足原来的容量的时候，OpenCV才会对矩阵空间进行重分配。

* MATLAB风格的初始化函数: cv::Mat::zeros , cv::Mat::ones , cv::Mat::eye . Specify size and data type to use:

```
Mat E = Mat::eye(4, 4, CV_64F);
cout << "E = " << endl << " " << E << endl << endl;
Mat O = Mat::ones(2, 2, CV_32F);
cout << "O = " << endl << " " << O << endl << endl;
Mat Z = Mat::zeros(3,3, CV_8UC1);
cout << "Z = " << endl << " " << Z << endl << endl;
``` 

![](https://docs.opencv.org/4.1.0/MatBasicContainerOut3.png)

* 对于小型的矩阵，你可以使用逗号分隔的初始化函数和初始化列表来创建矩阵（C++11标准支持最后一个用法）：

```
Mat C = (Mat_<double>(3,3) << 0, -1, 0, -1, 5, -1, 0, -1, 0);
cout << "C = " << endl << " " << C << endl << endl;
C = (Mat_<double>({0, -1, 0, -1, 5, -1, 0, -1, 0})).reshape(3);
cout << "C = " << endl << " " << C << endl << endl;
```

![](https://docs.opencv.org/4.1.0/MatBasicContainerOut6.png)

* Create a new header for an existing Mat object and cv::Mat::clone or cv::Mat::copyTo it.
为一个已经存在的Mat对象创建一个新的头部，并使用cv::Mat::clone或者cv::Mat::copyTo来创建它。

```
Mat RowClone = C.row(1).clone();
cout << "RowClone = " << endl << " " << RowClone << endl << endl;
```

![](https://docs.opencv.org/4.1.0/MatBasicContainerOut7.png)

> 注意
>
> 你可以通过使用cv::randu()函数来创建一个有随机值的矩阵。你可以给出矩阵值的最大值和最小值,以限制矩阵元素值的范围。

```
Mat R = Mat(3, 2, CV_8UC3);
randu(R, Scalar::all(0), Scalar::all(255));
```

对输出数据进行格式化处理

上面的例子当中，你看到了默认的格式化输出选项。但是，OpenCV则允许你对你的矩阵输出进行自定义设置：

* Default

```
cout << "R (default) = " << endl <<        R           << endl << endl;
```

![](https://docs.opencv.org/4.1.0/MatBasicContainerOut8.png)

* Python

```
cout << "R (python)  = " << endl << format(R, Formatter::FMT_PYTHON) << endl << endl;
```

![](https://docs.opencv.org/4.1.0/MatBasicContainerOut16.png)

* Comma separated values (CSV)

```
cout << "R (csv)     = " << endl << format(R, Formatter::FMT_CSV   ) << endl << endl;
```

![](https://docs.opencv.org/4.1.0/MatBasicContainerOut10.png)

* Numpy

```
cout << "R (numpy)   = " << endl << format(R, Formatter::FMT_NUMPY ) << endl << endl;
```

![](https://docs.opencv.org/4.1.0/MatBasicContainerOut9.png)

* C

```
cout << "R (c)       = " << endl << format(R, Formatter::FMT_C     ) << endl << endl;
```

![](https://docs.opencv.org/4.1.0/MatBasicContainerOut11.png)

Output of other common items

OpenCV offers support for output of other common OpenCV data structures too via the << operator:

* 二维空间的点

```
Point2f P(5, 1);
cout << "Point (2D) = " << P << endl << endl;
```

![](https://docs.opencv.org/4.1.0/MatBasicContainerOutMatBasicContainerOut12.png）

* 三维空间的点

```
Point3f P3f(2, 6, 7);
cout << "Point (3D) = " << P3f << endl << endl;
```

![](https://docs.opencv.org/4.1.0/MatBasicContainerOutMatBasicContainerOut13.png)

* 通过cv::Mat创建std::vector

```
vector<float> v;
v.push_back( (float)CV_PI);   v.push_back(2);    v.push_back(3.01f);
cout << "Vector of floats via Mat = " << Mat(v) << endl << endl;
```

![](https://docs.opencv.org/4.1.0/MatBasicContainerOutMatBasicContainerOut14.png)

* OpenCV点类型的std::vector

```
vector<Point2f> vPoints(20);
for (size_t i = 0; i < vPoints.size(); ++i)
    vPoints[i] = Point2f((float)(i * 5), (float)(i % 7));
cout << "A vector of 2D Points = " << vPoints << endl << endl;
```

![](https://docs.opencv.org/4.1.0/MatBasicContainerOutMatBasicContainerOut15.png)

这里的大多数例子都写在一个控制台程序里。你可以从这里下载，或从OpenCV的cpp例子中找到这个程序。

你可以在Youtube上看到关于这个教程的演示视频。

