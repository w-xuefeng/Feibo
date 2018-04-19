

# 目录
<ul class="toc">
  <li>
    <a href="#目录">目录</a>
  </li>
  <li>
    <a href="#1简介">1.简介</a>
  </li>
  <li>
    <a href="#2引用头文件">2.引用头文件</a>
  </li>
  <li>
    <a href="#3定义结构体">3.定义结构体</a>
  </li>
  <li>
    <a href="#4函数声明清单">4.函数声明清单</a>
  </li>
  <li>
    <a href="#5博客链接">5.博客链接</a>
  </li>	
</ul>

---
# 1.简介
- 目的：使用```分治法```和```大整数```求斐波那契数列的前n项
- 思路：使用数组存储大整数，将求斐波那契数列的第n项转换为求矩阵幂的运算。

# 2.引用头文件
```
#include <stdio.h>
#include <stdlib.h>
#include <malloc.h>
```

# 3.定义结构体
```C++
typedef struct largeNumber{//大整数结构体
	
	int length;//大整数的长度

	int *number;//使用数组存储大整数

}largeNum;

typedef struct matrix{//2*2矩阵结构体

	largeNum RowCol00,RowCol01,RowCol10,RowCol11;//由4个大整数组成

}Matrix;

```
# 4.函数声明清单
```CPP
     int max(int,int);
    //返回两个整数中较大的值
    
    int min(int,int);
    //返回两个整数中较小的值
    
    int sub(int,int);
    //返回两个整数的差的绝对值
    
    largeNum largeNumApplyRoom(largeNum);
    //为一个大整数申请空间，使用前必须定义长度
    
    largeNum initlargeNumber(int,int *);
    //初始化一个大整数
    
    largeNum initlargeNumToZero(largeNum,int);
    //初始化一个定长的大整数且各个数位都为0
    
    largeNum formatAddZero(largeNum,int);
    //格式话较短的大整数在其前补0到定长
    
    largeNum maxLargeNumLength(largeNum,largeNum);
    //返回两个大整数中长度较长的大整数
    
    largeNum minLargeNumLength(largeNum,largeNum);
    //返回两个大整数中长度较短的大整数
    
    largeNum largeNumAdd(largeNum,largeNum);
    //大整数的加法运算
    
    largeNum largeNumMultiply(largeNum, largeNum);
    //大整数的乘法运算
    
    int *largeNumCheckNum(int *,int);
    //检测大整数进位
    
    void largeNumPrintf(largeNum);
    //输出大整数
    
    Matrix initMatrix(largeNum,largeNum,largeNum,largeNum);
    //初始化一个2*2的矩阵
    
    Matrix matrixMultiply(Matrix,Matrix);
    //定义矩阵的乘法
    
    Matrix matrixPower(matrix,int);
    //矩阵的幂运算
    
    void matrixPrintf(Matrix);
    //输出矩阵

```
# 5.博客链接
 
↖(^ω^)↗ Welcome to https://blog.wangxuefeng.com.cn/archives/feibo.html
