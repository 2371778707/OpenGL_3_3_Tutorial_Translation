﻿Math Cheatsheet
数学备忘单
三角学
Pi
正弦和余弦
单位圆
向量
齐次坐标
长度
叉乘
点乘
加法和减法
乘法
归一化
矩阵
矩阵与矩阵乘法
矩阵与向量乘法
常用的转换

三角学
Pi
const float pi = 3.14159265f; // but an infinity of digits in reality
正弦与余弦
(From http://commons.wikimedia.org/wiki/User:Geek3 , under GNU Free Documentation License )
单位圆
( Modified from http://en.wikipedia.org/wiki/User:Gustavb under Crative Commons 3.0 )
t表示按弧度算的一个角度
0 radians = 0 degrees
180 degrees = Pi radians
360 degrees ( 整圆) = 2*Pi radians
90 degrees = Pi/2 radians

向量
时刻清楚是在哪个坐标系下表示的向量。更多细节查看第三部分。
齐次坐标
一个三维向量是(x, y, z),但齐次坐标系下三维向量是(x, y, z, w).
w=0 : 表示一个方向
w=1 : 表示一个位置
else : 仍然可能正确，但最好知道你在做什么.
一个齐次坐标向量只能乘以4*4的矩阵。

长度
类似于笛卡尔距离：sqrt( x*x + y*y + z*z ). w不计算在内.

叉乘
( Modified from http://en.wikipedia.org/wiki/User:Acdx , former image under Creative Commons 3.0 )
叉乘符号用X。length( a x b ) == length(a)*length(b)，因此你可能想把结果normalize()。

点乘
( from http://en.wikipedia.org/wiki/File:Dot_Product.svg )
A.B = length(A)*length(B)*cos(Theta) , 但是更多时候计算为A.x*B.x +A.y*B.y +A.z*B.z

加法和减法
分量形式 :
res.x = A.x + B.x
res.y = A.y + B.y
...
 

乘法
分量形式:
res.x = A.x * B.x
res.y = A.y * B.y
...

归一化
将向量除以它的长度：
normalizedVector = vec * ( 1.0f / vec.length() )


矩阵
矩阵与矩阵乘法
用一个平移矩阵为例子：

矩阵向量乘法

常用变换


...但是在你的着色器中，也可以在切线空间中表示向量。当你做后期效果（post-effects）时也可以在图像空间中表示。
… but in your shaders, you can also represent your vectors in tangent space. And in image-space when you do post-effects.