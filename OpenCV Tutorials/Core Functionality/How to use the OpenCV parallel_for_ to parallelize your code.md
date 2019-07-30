目的

本教程主要目的是为了告诉你如何使用OpenCV的parallel_for_框架轻松地对你的代码进行并行化处理。为了介绍这一理论，我们将编写一个画曼德尔布罗特分形图的程序，这个程序会占用所有可用的CPU运算资源。本教程所涉及的代码都将在这里查阅。如果你希望知道更多有关多线程的知识，你需要查阅相关书籍或者课程，因为这些知识不再本教程范围之内，本教程只是简单的介绍相关知识点。

前提条件

第一个前提是，OpenCV的并行计算是构建在一个并行计算的框架上。自OpenCV3.2开始，OpenCV调用的并行框架按以下顺序进行：

1.英特尔多线程构建模块（第三方库，必须显式调用）

2.C=并行的C/C++编程语言扩展（第三方库，必须显式调用）

3.OpenMP （与编译器整合在一起，必须显式调用）

4.APPLE GCD （系统级别的框架，可直接使用（只能用在MacOS上））

5.Windows RT并行框架（系统级别的框架，可直接使用（只能用在Windows RT上））

6.Windows 并行框架（运行时的一部分，可直接使用（仅限Windows，MSVC++ >= 10））

7.Pthreads（在可用的情况下）

所以，正如你所看到的，大多数并行框架都能在OpenCV中使用到。一些框架是第三方控件，需要显式启用并在cmake中修改（比如，TBB和C=），其他的可以根据平台自动启用（例如，APPLE GCD）。使用的方式是你应该有足够大的权限可以直接访问到这些并行框架，或者在CMake的选项中启用，并重建OpenCV。

第二个（较弱的）前提条件是，你需要确保你的任务是符合并行处理的，换而言之并行处理取决于你的任务情况，并不是所有的任务都适合/能够改成并行处理行为。简单而言，那些可以分成多个基本操作步骤并与内存分配无关（没有潜在地条件竞争情况）的任务适合并行式处理。机器视觉因为大多数时候处理的是像素，而像素的处理与其他像素处理无关，因此是可以进行并行式操作的。

简单教程：画曼德尔布罗特分形图

在本篇教程中，我们将使用曼德尔布罗特分形图来作为例子，讲解如何将普通的时序性代码转换成并行计算的代码。

原理

曼德尔布罗特分形图是数学家阿德里安·杜阿德为了致敬著名数学家本华·曼德博而命名的图形。他之所以如此出名，是因为他的图形展示是一张分形图形，他是一个数学集合，每一部分在任何大小上都是自身的重复（甚至曼德尔布罗特集合是自相似的，图形的任何一个部分在任意大小上都是相似的）。如果你想了解更多信息，你可以查阅维基百科文档。这里我们只介绍相关公式，介绍如何绘制曼德尔布罗特分形图。

曼德尔布罗特集合是复平面上一系列复数点的集合，围绕着零点，根据下述二次映射来作图：

![](http://latex.codecogs.com/gif.latex?\begin{cases}z_0=0&z_{n+1}=z_n^2+c\end{cases})

该函数保持有界。也就是说，如果复数c是曼德尔布罗特集合中的，当![](http://latex.codecogs.com/gif.latex?z_{0}=0)时并且持续执行这个递归式，无论n取多大，![](http://latex.codecogs.com/gif.latex?z_{n})的绝对值始终有界。也就是可以表示成如下所示：

![](http://latex.codecogs.com/gif.latex?\limsup_{n\to\infty}|z_{n+1}|\leqslant2)

伪代码

一个简单生成曼德尔布罗特集合的算法叫做“逃离时间算法”。在这张生成的图片中，如果复数值已经有界或者还没有达到迭代次数的最大值，每一个像素我们都会使用递推关系来生成。像素不属于曼德尔布罗特集合部分的将会舍弃，因为我们要确定所有的像素都要在规定的迭代次数中形成。迭代次数越大将能生成越精细的图形，但是运行时间也由此增加。我们使用递归次数来描述图中的像素值。

```
For each pixel (Px, Py) on the screen, do:
{
  x0 = scaled x coordinate of pixel (scaled to lie in the Mandelbrot X scale (-2, 1))
  y0 = scaled y coordinate of pixel (scaled to lie in the Mandelbrot Y scale (-1, 1))
  x = 0.0
  y = 0.0
  iteration = 0
  max_iteration = 1000
  while (x*x + y*y < 2*2  AND  iteration < max_iteration) {
    xtemp = x*x - y*y + x0
    y = 2*x*y + y0
    x = xtemp
    iteration = iteration + 1
  }
  color = palette[iteration]
  plot(Px, Py, color)
}
```

为了让这些伪代码和理论有联系，我们有以下公式：

* ![](http://latex.codecogs.com/gif.latex?z=x+iy)
* ![](http://latex.codecogs.com/gif.latex?z^2=x^2+i2xy-y^2)
* ![](http://latex.codecogs.com/gif.latex?c=x_0+iy_0)

![](https://docs.opencv.org/4.1.0/how_to_use_OpenCV_parallel_for_640px-Mandelset_hires.png)

在这张图中，我们将复数的实部放在x轴上，虚部放在y轴上。如果我们放大特定的某一部分，你可以看到整个图片都在重复自己的某一部分。

算法实现

逃脱时间算法的实现

```
int mandelbrot(const complex<float> &z0, const int max)
{
    complex<float> z = z0;
    for (int t = 0; t < max; t++)
    {
        if (z.real()*z.real() + z.imag()*z.imag() > 4.0f) return t;
        z = z*z + z0;
    }
    return max;
}
```

在这里，我们是用了std::complex模板来表示一个复数。这个函数会检查像素是否在这个集合，并返回“escaped"的结果。

序列式曼德尔布罗特算法实现

```
void sequentialMandelbrot(Mat &img, const float x1, const float y1, const float scaleX, const float scaleY)
{
    for (int i = 0; i < img.rows; i++)
    {
        for (int j = 0; j < img.cols; j++)
        {
            float x0 = j / scaleX + x1;
            float y0 = i / scaleY + y1;
            complex<float> z0(x0, y0);
            uchar value = (uchar) mandelbrotFormula(z0);
            img.ptr<uchar>(i)[j] = value;
        }
    }
}
```

在这个实现当中，我们在像素上一步步递归并生成图形，判断像素是否在曼德尔布罗特集合当中。

另一件要做的事情是通过下列代码将像素协调至曼德尔布罗特集合空间当中：

```
Mat mandelbrotImg(4800, 5400, CV_8U);
float x1 = -2.1f, x2 = 0.6f;
float y1 = -1.2f, y2 = 1.2f;
float scaleX = mandelbrotImg.cols / (x2 - x1);
float scaleY = mandelbrotImg.rows / (y2 - y1);
```

最后，为了能够确定灰阶图中像素的值，我们有以下规定：

* 如果他满足迭代的最大值（像素点假设是在曼德尔布罗特集合当中），像素便是黑色的。
* 否则，我们就会根据逃离的迭代和规模来确定灰阶程度，最后定一个灰阶值。

```
int mandelbrotFormula(const complex<float> &z0, const int maxIter=500) {
    int value = mandelbrot(z0, maxIter);
    if(maxIter - value == 0)
    {
        return 0;
    }
    return cvRound(sqrt(value / (float) maxIter) * 255);
}
```

使用线性转换的方式并不能准确描述灰阶变化。为了解决这个问题，我们将使用平方根尺度变换来增强准确性。![](http://latex.codecogs.com/gif.latex?\(f\left(x\right)=\sqrt{\frac{x}{\text{maxIter}}}\times255\))

![](https://docs.opencv.org/4.1.0/how_to_use_OpenCV_parallel_for_sqrt_scale_transformation.png)

绿色的曲线跟线性尺度变换有关，而蓝色的线则与平方根尺度变换由管，当你观察那些倾斜的位置时，你可以的看到最低端的值有所增加。

并行曼德尔布罗特集合算法实现

当我们观察时序性曼德尔布罗特集合算法实现的时候，我们会观察到，每一个像素点的计算都是独立的。为了得到最优化的性能，我们可以并行地处理多个像素点的计算，这样我们就能利用现代处理器的多线程结构。为了能更方便的实现这一效果，我们使用OpenCV的cv::parallel_for_框架。

```
class ParallelMandelbrot : public ParallelLoopBody
{
public:
    ParallelMandelbrot (Mat &img, const float x1, const float y1, const float scaleX, const float scaleY)
        : m_img(img), m_x1(x1), m_y1(y1), m_scaleX(scaleX), m_scaleY(scaleY)
    {
    }
    virtual void operator ()(const Range& range) const CV_OVERRIDE
    {
        for (int r = range.start; r < range.end; r++)
        {
            int i = r / m_img.cols;
            int j = r % m_img.cols;
            float x0 = j / m_scaleX + m_x1;
            float y0 = i / m_scaleY + m_y1;
            complex<float> z0(x0, y0);
            uchar value = (uchar) mandelbrotFormula(z0);
            m_img.ptr<uchar>(i)[j] = value;
        }
    }
    ParallelMandelbrot& operator=(const ParallelMandelbrot &) {
        return *this;
    };
private:
    Mat &m_img;
    float m_x1;
    float m_y1;
    float m_scaleX;
    float m_scaleY;
};
```

第一件事是要声明一个继承于cv::ParallelLoopBody的类，并覆写虚拟函数void operator()(const cv::Range& range)常量。

operator()中的range变量表示像素的子集，并会在一个单独线程内进行处理。这项分治工作会自动处理，以将计算负担均匀分割到每一个线程中。我们将像素索引坐标转换为2D\[行，列\]坐标。同时，我们也会保持对mat图像的引用，其确保准确修改所引用的图片。

并行计算通过以下函数得到执行：

```
ParallelMandelbrot parallelMandelbrot(mandelbrotImg, x1, y1, scaleX, scaleY);
parallel_for_(Range(0, mandelbrotImg.rows*mandelbrotImg.cols), parallelMandelbrot);
```

在这里，rang表示图像中将要处理的像素的总数。你可以使用cv::setNumThreads来设置线程的数目。你可以用cv::parallel_for_中的nstripes变量来指明线程数。例如，如果你的处理器有四个线程，而你设置的是cv::setNumThreads(2)或者nstripes=3，那么默认情况下它将会使用所有可用的处理器线程，但是只会将工作负荷放到两个线程中。

> 注意
>
> C++11标准允许程序员通过不使用ParallelMandelbrot类而用lambda表达式来简化代码：

```
parallel_for_(Range(0, mandelbrotImg.rows*mandelbrotImg.cols), [&](const Range& range){
    for (int r = range.start; r < range.end; r++)
    {
        int i = r / mandelbrotImg.cols;
        int j = r % mandelbrotImg.cols;
        float x0 = j / scaleX + x1;
        float y0 = i / scaleY + y1;
        complex<float> z0(x0, y0);
        uchar value = (uchar) mandelbrotFormula(z0);
        mandelbrotImg.ptr<uchar>(i)[j] = value;
    }
});
```

结果

你可以在这里找到完整的代码。并行计算的性能取决于你用的电脑CPU。例如，在四核八线程CPU中，你会得到6.9倍的性能提升。有很多原因导致我们无法得到8倍的性能提升。主要原因可能有：

* 创建和管理线程需要有额外的开支，
* 在并行的时候有后台运行线程，
* 4个硬件处理器带有两个逻辑线程和8个硬件处理中只有一个线程是不一样的。

使用示例代码产生的图片如下所示（你可以更改代码从而设置更多的迭代，或者更改在溢出时间的图像像素的颜色，或者使用调色板来让你的图片更有美感）：

![](https://docs.opencv.org/4.1.0/how_to_use_OpenCV_parallel_for_Mandelbrot.png)

Mandelbrot set with xMin=-2.1, xMax=0.6, yMin=-1.2, yMax=1.2, maxIterations=500
