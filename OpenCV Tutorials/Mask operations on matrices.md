在矩阵上使用蒙版矩阵操作是非常简单的。做法是，我们根据蒙版矩阵（被认为是一个核心）的方式对图形的每一个像素点进行重新计算，最后获得计算后的值。这个蒙版值会通过相邻像素值（和本身像素值）的互相影响程度来进行调整，最后产生一个新的值。在数学层面上而言，我们通过我们特定的值做了个加权平均的计算。

测试用例

让我们来考虑一下图像对比度增强算法的问题。首先，我们想让图形的每一个像素的值通过以下公式来改变：

![](http://latex.codecogs.com/gif.latex?I(i,j)=5*I(i,j)-[I(i-1,j)+I(i+1,j)+I(i,j-1)+I(i,j+1)])

![](http://latex.codecogs.com/gif.latex?\iffI(i,j)*M,\text{where}M=\bordermatrix{_i\backslash^j&-1&0&+1\cr-1&0&-1&0\cr0&-1&5&-1\cr+1&0&-1&0\cr})

The first notation is by using a formula, while the second is a compacted version of the first by using a mask. You use the mask by putting the center of the mask matrix (in the upper case noted by the zero-zero index) on the pixel you want to calculate and sum up the pixel values multiplied with the overlapped matrix values. It's the same thing, however in case of large matrices the latter notation is a lot easier to look over.

具体代码请参考原教程
