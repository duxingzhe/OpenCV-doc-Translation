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

另外一个问题是，我们应该如何确定程序最终运行所需要的时间？对于这个问题，OpenCV提供了cv::getTickCount()和cv::getTickfrequency()两个简单的函数来处理。第一个返回在特定情况下你的CPU时钟数（比如，你开机以后）。第二个函数返回你的CPU每秒滴答的次数。所以，在这两者之间计算程序运行的时长只需要通过以下简单的方法即可实现：

```
double t = (double)getTickCount();
// do something ...
t = ((double)getTickCount() - t)/getTickFrequency();
cout << "Times passed in seconds: " << t << endl;
```

图形矩阵在内存中所占用的内存大小

正如之前的《Mat——基本图像容器》教程所提及的，矩阵的大小决定于使用的色域。更加精确的色域取决于使用的通道数。在一张灰阶的图形中，我们会得到下面的矩阵：

![](https://docs.opencv.org/4.1.0/tutorial_how_matrix_stored_1.png)

对于多通道图像而言，通道中的小通道才是图像的通道数。例如在BGR图像系统中：

![](https://docs.opencv.org/4.1.0/tutorial_how_matrix_stored_2.png)

需要注意的是，我们所用的图像系统和我们认知的图像系统是不一样的：BGR，而不是RGB。因为在许多情况下，内存是可以以列排序的方式容纳下许多密集的数据，这样数据一个接一个紧密相连，从形成简单的长列。因为在许多时候，所有的数据都放在同一个地方，紧紧排列会帮助加快扫描进度。所以，我们便可以使用cv::Mat::isContinous()函数来判断矩阵是否满足之前说的条件。我们会在下一个章节介绍使用的方法。

有效的方法

当谈到有关性能问题的时候，C语言永远是第一名，传统的C指针风格访问形式永远是最优的。因此，我们在处理OpenCV数据的时候，最有效的方法便是下面的代码：

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

在这里，我们直接简单的使用了一个指针，这个指针从每一行的开始指向这一行的结束。在这个比较特别的测试集中矩阵是存储在一个连续的地方，所以我们只需要让指针指向数据集一次即可，然后从头读到尾。我们需要考虑到彩色图片：我们有三个通道，也就意味着我们需要在每一行的处理时间要是原来的三倍多。

当然，我们还可以用其他方法实现。Mat数据中的data部分返回了指向第一行第一列的指针。如果指针为空，则说明在Mat并没有合适的数据。检查数据是否为空的最最简单方法便是检查你的图片是否加载成功。在本例子中，数据的存储是连续的，因此我们用一个指针遍历所有数据。在一张灰阶图片里，程序应该是这样写的：

```
uchar* p = I.data;
for( unsigned int i =0; i < ncol*nrows; ++i)
    *p++ = table[*p];
```

你可以在你的程序中得到同样的结果。然而，接下来的代码会变的相当复杂。如果你使用了更多的高级特性，代码会变得更加难懂。另外，实际上，你只能得到同样的性能（因为实际情况下，大多数近期的编译器都会对你的代码进行微小的优化，这让你的代码运行起来相差无几。）

迭代器方法（安全）

In case of the efficient way making sure that you pass through the right amount of uchar fields and to skip the gaps that may occur between the rows was your responsibility. The iterator method is considered a safer way as it takes over these tasks from the user. All you need to do is ask the begin and the end of the image matrix and then just increase the begin iterator until you reach the end. To acquire the value pointed by the iterator use the * operator (add it before it).

```
Mat& ScanImageAndReduceIterator(Mat& I, const uchar* const table)
{
    // accept only char type matrices
    CV_Assert(I.depth() == CV_8U);
    const int channels = I.channels();
    switch(channels)
    {
    case 1:
        {
            MatIterator_<uchar> it, end;
            for( it = I.begin<uchar>(), end = I.end<uchar>(); it != end; ++it)
                *it = table[*it];
            break;
        }
    case 3:
        {
            MatIterator_<Vec3b> it, end;
            for( it = I.begin<Vec3b>(), end = I.end<Vec3b>(); it != end; ++it)
            {
                (*it)[0] = table[(*it)[0]];
                (*it)[1] = table[(*it)[1]];
                (*it)[2] = table[(*it)[2]];
            }
        }
    }
    return I;
}
```

In case of color images we have three uchar items per column. This may be considered a short vector of uchar items, that has been baptized in OpenCV with the Vec3b name. To access the n-th sub column we use simple operator[] access. It's important to remember that OpenCV iterators go through the columns and automatically skip to the next row. Therefore in case of color images if you use a simple uchar iterator you'll be able to access only the blue channel values.

On-the-fly address calculation with reference returning
The final method isn't recommended for scanning. It was made to acquire or modify somehow random elements in the image. Its basic usage is to specify the row and column number of the item you want to access. During our earlier scanning methods you could already observe that is important through what type we are looking at the image. It's no different here as you need to manually specify what type to use at the automatic lookup. You can observe this in case of the gray scale images for the following source code (the usage of the + cv::Mat::at() function):

```
Mat& ScanImageAndReduceRandomAccess(Mat& I, const uchar* const table)
{
    // accept only char type matrices
    CV_Assert(I.depth() == CV_8U);
    const int channels = I.channels();
    switch(channels)
    {
    case 1:
        {
            for( int i = 0; i < I.rows; ++i)
                for( int j = 0; j < I.cols; ++j )
                    I.at<uchar>(i,j) = table[I.at<uchar>(i,j)];
            break;
        }
    case 3:
        {
         Mat_<Vec3b> _I = I;
         for( int i = 0; i < I.rows; ++i)
            for( int j = 0; j < I.cols; ++j )
               {
                   _I(i,j)[0] = table[_I(i,j)[0]];
                   _I(i,j)[1] = table[_I(i,j)[1]];
                   _I(i,j)[2] = table[_I(i,j)[2]];
            }
         I = _I;
         break;
        }
    }
    return I;
}
```

The functions takes your input type and coordinates and calculates on the fly the address of the queried item. Then returns a reference to that. This may be a constant when you get the value and non-constant when you set the value. As a safety step in debug mode only* there is performed a check that your input coordinates are valid and does exist. If this isn't the case you'll get a nice output message of this on the standard error output stream. Compared to the efficient way in release mode the only difference in using this is that for every element of the image you'll get a new row pointer for what we use the C operator[] to acquire the column element.

If you need to do multiple lookups using this method for an image it may be troublesome and time consuming to enter the type and the at keyword for each of the accesses. To solve this problem OpenCV has a cv::Mat_ data type. It's the same as Mat with the extra need that at definition you need to specify the data type through what to look at the data matrix, however in return you can use the operator() for fast access of items. To make things even better this is easily convertible from and to the usual cv::Mat data type. A sample usage of this you can see in case of the color images of the upper function. Nevertheless, it's important to note that the same operation (with the same runtime speed) could have been done with the cv::Mat::at function. It's just a less to write for the lazy programmer trick.
