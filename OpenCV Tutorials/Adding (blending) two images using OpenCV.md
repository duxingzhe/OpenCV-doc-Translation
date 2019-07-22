目的

本教程你将学到

* 解释线性混合并解释为什么他很有效果
* 使用addWeighted()函数添加两张图片

原理

> 注意
>
> 以下用例来自书籍Richard Szeliski《机器视觉：算法和应用》

在我们之前的教程中，我们已经知道了一系列像素操作。线性混合操作是比较有趣的二阶（双输入）操作：

![](http://latex.codecogs.com/gif.latex?g(x)=(1-\alpha)f_{0}(x)+\alphaf_{1}(x))

将α的值在0到1之间变换，这个操作可以实现两张图片或视频之间短暂的交叉溶解效果，就好像幻灯片和电影中常见的一种特效（很酷，不是吗？）

> 注意
>
> 我们不能随意添加两张图片，两张图片的长宽和图像类型必须一致。

现在我们就来生成一张通过公式得到的g(x)图片。在这个例子中，addWeighted()使用非常简单：

```
beta = ( 1.0 - alpha );
addWeighted( src1, alpha, src2, beta, 0.0, dst);
```

addWeighted()的公式如下:

![](http://latex.codecogs.com/gif.latex?dst=α⋅src1+β⋅src2+γ)

这个例子中，gamma在代码里的值为0。

然后，我们创建一个窗口，显示图片，让用户结束这个程序。

```
imshow( "Linear Blend", dst );
waitKey(0);
```

具体代码请参考原教程
