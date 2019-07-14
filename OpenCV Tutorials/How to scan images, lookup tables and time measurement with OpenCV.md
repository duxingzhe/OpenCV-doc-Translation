Goal

We'll seek answers for the following questions:

* How to go through each and every pixel of an image?
* How is OpenCV matrix values stored?
* How to measure the performance of our algorithm?
* What are lookup tables and why use them?

Our test case

Let us consider a simple color reduction method. By using the unsigned char C and C++ type for matrix item storing, a channel of pixel may have up to 256 different values. For a three channel image this can allow the formation of way too many colors (16 million to be exact). Working with so many color shades may give a heavy blow to our algorithm performance. However, sometimes it is enough to work with a lot less of them to get the same final result.

In this cases it's common that we make a color space reduction. This means that we divide the color space current value with a new input value to end up with fewer colors. For instance every value between zero and nine takes the new value zero, every value between ten and nineteen the value ten and so on.

When you divide an uchar (unsigned char - aka values between zero and 255) value with an int value the result will be also char. These values may only be char values. Therefore, any fraction will be rounded down. Taking advantage of this fact the upper operation in the uchar domain may be expressed as:
