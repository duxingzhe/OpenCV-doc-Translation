目的：

我们将在这篇教程中解决以下问题：

* 我们将如何遍历一张图片的每一个像素？
* OpenCV矩阵值具体是如何存储的？
* 如何衡量我们的算法性能？
* 什么是查找表？如何使用查找表？

测试用例

我们先来分析一下一个简单的颜色消除算法。矩阵中的元素数据是按照C和C++的无符号字符类型存储，每一个像素的通道有256种不同的值。对一个有三个通道的图像来说，我们可以生成许多种颜色（大概有1600万颜色）。对这么多的颜色进行处理会严重降低算法的性能。然而，有时候如果我们能对这个图像的部分数据进行舍弃的话，我们依旧可以达到我们想要的效果。

在这个例子当中，我们会使用色域消减的方法来提高性能。也就是说，我们将色域分解，然后设定范围，让当前色域的值设置成一个新值，从而减少颜色的处理规模。这个例子里，0-9的值，我们会赋值为0；10-19的值，我们就赋值为10。

当你区分uchar（unsigned char，其值在0-255之间）和int类型的值时，程序会自动转换为char类型。这些值同样会变成char类型。因此，值域范围会逐步缩小。使用这个功能之后，uchar的值的改变是由以下公式决定的：

![](http://latex.codecogs.com/gif.latex?I_{new}=(\frac{I_{old}}{10})*10)

一个简单的色域消除算法只是将图形矩阵的每一个像素传入这个公式中在进行处理。我们如果只是简单地执行乘除运算是没有多少意义的。这些运算对于机器而言十分耗性能。如果我们尽可能避免这种方法，而采用一些简单的运算方式，比如转化成加减法、或者转化成更为简单的任务。另外，我们要注意到，输入值是有一个非常明显的上界。在这个例子中，uchar最多有256个值。

因此，对于一些体积更大的图像而言，最好的方式是将所有可能的值在处理之前先算出来，通过查找表的时候只做任务分配。查找表是一个简单的数组（有一维和多维之分），表的内容是将输入值变量和最终输出值一一对应。他的优点就在于我们根据这张表格来查找计算结果，而不是每次都需要计算数据。

我们的这个测试程序（就是我们目前展示的这个）将实现以下功能：在控制台上用命令读取一张图片，图片是命令行的一个参数（可能是多种颜色的，也有可能是灰阶——同样也是命令行参数的形式）并且通过命令行参数的形式传递一个整数值来指明降低的程度。在OpenCV库中，目前有三种方式来处理图片中的各个像素点。为了让这三种扫描图形方法的效果变得更加直观有趣，我们会将每个方法的运行时间打印出来。

你可以在这儿下载本文所涉及的全部源代码或者在OpenCV的源代码samples目录中C++代码部分找到本文代码。它的基本用法是：

```
how_to_scan_images imageName.jpg intValueToReduce [G]
```

最后一个命令是可选的。如果指明了这个选项，图像是以灰阶方式加载的，否则就按照BGR色域方式加载图像。我们要做的第一件事便是计算查询表。

```
int divideWith = 0; // convert our input string to number - C++ style
stringstream s;
s << argv[2];
s >> divideWith;
if (!s || !divideWith)
{
    cout << "Invalid number entered for dividing. " << endl;
    return -1;
}
uchar table[256];
for (int i = 0; i < 256; ++i)
   table[i] = (uchar)(divideWith * (i/divideWith));
```


这里，第一步，我们先使用了C++中的stringsream类来将第三个命令行参数转化为整型。接着我们再简单看看上面提到的公式，这个公式用来生成查询表。我们并没有使用到OpenCV的功能。

Another issue is how do we measure time? Well OpenCV offers two simple functions to achieve this cv::getTickCount() and cv::getTickFrequency() . The first returns the number of ticks of your systems CPU from a certain event (like since you booted your system). The second returns how many times your CPU emits a tick during a second. So to measure in seconds the number of time elapsed between two operations is easy as:

```
double t = (double)getTickCount();
// do something ...
t = ((double)getTickCount() - t)/getTickFrequency();
cout << "Times passed in seconds: " << t << endl;
```

How is the image matrix stored in memory?

As you could already read in my Mat - The Basic Image Container tutorial the size of the matrix depends on the color system used. More accurately, it depends from the number of channels used. In case of a gray scale image we have something like:

![](https://docs.opencv.org/4.1.0/tutorial_how_matrix_stored_1.png)

For multichannel images the columns contain as many sub columns as the number of channels. For example in case of an BGR color system:

![](https://docs.opencv.org/4.1.0/tutorial_how_matrix_stored_2.png)

Note that the order of the channels is inverse: BGR instead of RGB. Because in many cases the memory is large enough to store the rows in a successive fashion the rows may follow one after another, creating a single long row. Because everything is in a single place following one after another this may help to speed up the scanning process. We can use the cv::Mat::isContinuous() function to ask the matrix if this is the case. Continue on to the next section to find an example.

The efficient way

When it comes to performance you cannot beat the classic C style operator[] (pointer) access. Therefore, the most efficient method we can recommend for making the assignment is:

```
Mat& ScanImageAndReduceC(Mat& I, const uchar* const table)
{
    // accept only char type matrices
    CV_Assert(I.depth() == CV_8U);
    int channels = I.channels();
    int nRows = I.rows;
    int nCols = I.cols * channels;
    if (I.isContinuous())
    {
        nCols *= nRows;
        nRows = 1;
    }
    int i,j;
    uchar* p;
    for( i = 0; i < nRows; ++i)
    {
        p = I.ptr<uchar>(i);
        for ( j = 0; j < nCols; ++j)
        {
            p[j] = table[p[j]];
        }
    }
    return I;
}
```

Here we basically just acquire a pointer to the start of each row and go through it until it ends. In the special case that the matrix is stored in a continuous manner we only need to request the pointer a single time and go all the way to the end. We need to look out for color images: we have three channels so we need to pass through three times more items in each row.
