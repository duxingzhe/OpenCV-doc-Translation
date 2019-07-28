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

We will use the example of drawing a Mandelbrot set to show how from a regular sequential code you can easily adapt the code to parallelize the computation.

Theory

The Mandelbrot set definition has been named in tribute to the mathematician Benoit Mandelbrot by the mathematician Adrien Douady. It has been famous outside of the mathematics field as the image representation is an example of a class of fractals, a mathematical set that exhibits a repeating pattern displayed at every scale (even more, a Mandelbrot set is self-similar as the whole shape can be repeatedly seen at different scale). For a more in-depth introduction, you can look at the corresponding Wikipedia article. Here, we will just introduce the formula to draw the Mandelbrot set (from the mentioned Wikipedia article).

The Mandelbrot set is the set of values of c in the complex plane for which the orbit of 0 under iteration of the quadratic map

![](http://latex.codecogs.com/gif.latex?\begin{cases}z_0=0\\z_{n+1}=z_n^2+c\end{cases})

remains bounded. That is, a complex number c is part of the Mandelbrot set if, when starting with z0=0 and applying the iteration repeatedly, the absolute value of zn remains bounded however large n gets. This can also be represented as

![](http://latex.codecogs.com/gif.latex?\limsup_{n\to\infty}|z_{n+1}|\leqslant2)

Pseudocode

A simple algorithm to generate a representation of the Mandelbrot set is called the "escape time algorithm". For each pixel in the rendered image, we test using the recurrence relation if the complex number is bounded or not under a maximum number of iterations. Pixels that do not belong to the Mandelbrot set will escape quickly whereas we assume that the pixel is in the set after a fixed maximum number of iterations. A high value of iterations will produce a more detailed image but the computation time will increase accordingly. We use the number of iterations needed to "escape" to depict the pixel value in the image.

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

To relate between the pseudocode and the theory, we have:

* ![](http://latex.codecogs.com/gif.latex?z=x+iy)
* ![](http://latex.codecogs.com/gif.latex?z^2=x^2+i2xy-y^2)
* ![](http://latex.codecogs.com/gif.latex?c=x_0+iy_0)

![](https://docs.opencv.org/4.1.0/how_to_use_OpenCV_parallel_for_640px-Mandelset_hires.png)

On this figure, we recall that the real part of a complex number is on the x-axis and the imaginary part on the y-axis. You can see that the whole shape can be repeatedly visible if we zoom at particular locations.

Implementation

Escape time algorithm implementation

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

Here, we used the std::complex template class to represent a complex number. This function performs the test to check if the pixel is in set or not and returns the "escaped" iteration.

Sequential Mandelbrot implementation

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

In this implementation, we sequentially iterate over the pixels in the rendered image to perform the test to check if the pixel is likely to belong to the Mandelbrot set or not.

Another thing to do is to transform the pixel coordinate into the Mandelbrot set space with:

```
Mat mandelbrotImg(4800, 5400, CV_8U);
float x1 = -2.1f, x2 = 0.6f;
float y1 = -1.2f, y2 = 1.2f;
float scaleX = mandelbrotImg.cols / (x2 - x1);
float scaleY = mandelbrotImg.rows / (y2 - y1);
```

Finally, to assign the grayscale value to the pixels, we use the following rule:

* a pixel is black if it reaches the maximum number of iterations (pixel is assumed to be in the Mandelbrot set),
* otherwise we assign a grayscale value depending on the escaped iteration and scaled to fit the grayscale range.

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

Using a linear scale transformation is not enough to perceive the grayscale variation. To overcome this, we will boost the perception by using a square root scale transformation (borrowed from Jeremy D. Frens in his blog post): f(x)=xmaxIter−−−−−√×255

![](https://docs.opencv.org/4.1.0/how_to_use_OpenCV_parallel_for_sqrt_scale_transformation.png)

The green curve corresponds to a simple linear scale transformation, the blue one to a square root scale transformation and you can observe how the lowest values will be boosted when looking at the slope at these positions.

Parallel Mandelbrot implementation

When looking at the sequential implementation, we can notice that each pixel is computed independently. To optimize the computation, we can perform multiple pixel calculations in parallel, by exploiting the multi-core architecture of modern processor. To achieve this easily, we will use the OpenCV cv::parallel_for_ framework.

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

The first thing is to declare a custom class that inherits from cv::ParallelLoopBody and to override the virtual void operator ()(const cv::Range& range) const.

The range in the operator () represents the subset of pixels that will be treated by an individual thread. This splitting is done automatically to distribute equally the computation load. We have to convert the pixel index coordinate to a 2D [row, col] coordinate. Also note that we have to keep a reference on the mat image to be able to modify in-place the image.

The parallel execution is called with:

```
ParallelMandelbrot parallelMandelbrot(mandelbrotImg, x1, y1, scaleX, scaleY);
parallel_for_(Range(0, mandelbrotImg.rows*mandelbrotImg.cols), parallelMandelbrot);
```

Here, the range represents the total number of operations to be executed, so the total number of pixels in the image. To set the number of threads, you can use: cv::setNumThreads. You can also specify the number of splitting using the nstripes parameter in cv::parallel_for_. For instance, if your processor has 4 threads, setting cv::setNumThreads(2) or setting nstripes=2 should be the same as by default it will use all the processor threads available but will split the workload only on two threads.

> Note
>
> C++ 11 standard allows to simplify the parallel implementation by get rid of the ParallelMandelbrot class and replacing it with lambda expression:

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

Results

You can find the full tutorial code here. The performance of the parallel implementation depends of the type of CPU you have. For instance, on 4 cores / 8 threads CPU, you can expect a speed-up of around 6.9X. There are many factors to explain why we do not achieve a speed-up of almost 8X. Main reasons should be mostly due to:

* the overhead to create and manage the threads,
* background processes running in parallel,
* the difference between 4 hardware cores with 2 logical threads for each core and 8 hardware cores.
* The resulting image produced by the tutorial code (you can modify the code to use more iterations and assign a pixel color depending on the escaped iteration and using a color palette to get more aesthetic images):

![](https://docs.opencv.org/4.1.0/how_to_use_OpenCV_parallel_for_Mandelbrot.png)

Mandelbrot set with xMin=-2.1, xMax=0.6, yMin=-1.2, yMax=1.2, maxIterations=500
