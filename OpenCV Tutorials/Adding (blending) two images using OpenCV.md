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

> Warning
> Since we are adding src1 and src2, they both have to be of the same size (width and height) and type.

Now we need to generate the g(x) image. For this, the function addWeighted() comes quite handy:

```
beta = ( 1.0 - alpha );
addWeighted( src1, alpha, src2, beta, 0.0, dst);
```

since addWeighted() produces:

![](http://latex.codecogs.com/gif.latex?dst=α⋅src1+β⋅src2+γ)

In this case, gamma is the argument 0.0 in the code above.

Create windows, show the images and wait for the user to end the program.

```
imshow( "Linear Blend", dst );
waitKey(0);
```

具体代码请参考原教程
