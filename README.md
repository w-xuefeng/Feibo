

# 目录
[TOC]

---
# 1.简介
- 目的：使用```分治法```和```大整数```求斐波那契数列的第n项
- 思路：使用数组存储大整数，将求斐波那契数列的第n项转换为求矩阵幂的运算。
- 公式如下：
```math
\left[
\begin{matrix}
f(n) & f(n-1) \\
f(n-1) & f(n-2) 
\end{matrix}
\right]
=
\left[
\begin{matrix}
1 & 1 \\
1 & 0 
\end{matrix}
\right]^{n-1}

```
# 2.引用头文件
```
#include <stdio.h>
#include <stdlib.h>
#include <malloc.h>
```

# 3.定义结构体
```
typedef struct largeNumber{//大整数结构体
	
	int length;//大整数的长度

	int *number;//使用数组存储大整数

}largeNum;

typedef struct matrix{//2*2矩阵结构体

	largeNum RowCol00,RowCol01,RowCol10,RowCol11;//由4个大整数组成

}Matrix;

```
# 4.函数的声明与定义
- 函数声明清单
    ```
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
- 函数的定义
    ```
    largeNum largeNumApplyRoom(largeNum a){
    //为一个大整数申请空间，使用前必须定义长度
    
    	a.number = (int *)malloc(sizeof(int)*a.length);
    
    	return a;
    
    }
    
    largeNum initlargeNumber(int length,int *number){
    //初始化一个大整数
    
    	largeNum a;
    	
    	a.length = length;
    
    	a = largeNumApplyRoom(a);
    
    	for (int i = 0; i < a.length; i++){
    		
    		a.number[i] = number[i];
    
    	}
    
    	return a;
    
    }
    
    largeNum initlargeNumToZero(largeNum a,int length){
    //初始化一个定长的大整数且各个数位都为0
    
    	a.length = length;
    
    	a = largeNumApplyRoom(a);
    
    	for (int i = 0; i < a.length; i++)	{
    		
    		a.number[i] = 0;
    
    	}
    
    	return a;
    
    }
    
    largeNum formatAddZero(largeNum a,int length){
    //格式话较短的大整数在其前补0到定长
    
    	if (length <= a.length ){
    
    		return a;
    
    	}
    
    	largeNum resault;
    
    	resault.length = length;
    
    	resault = initlargeNumToZero(resault,resault.length);
    
    	for (int i = length-1,j = a.length-1; j >= 0 ; i--,j--){
    
    		resault.number[i] = a.number[j];
    
    
    	}
    
    	return resault;
    
    }
    
    int max(int a,int b){
    //返回两个整数中较大的值
    
    	return a >= b ? a : b ;
    
    }
    
    
    int min(int a,int b){
    //返回两个整数中较小的值
    
    	return a <= b ? a : b ;
    
    }
    
    int sub(int a,int b){
    //返回两个整数的差的绝对值
    
    	return max(a,b) - min(a,b);
    
    }
    
    largeNum maxLargeNumLength(largeNum a,largeNum b){
    //返回两个大整数中长度较长的大整数
    
    	return a.length >= b.length? a : b ; 
    
    }
    
    largeNum minLargeNumLength(largeNum a,largeNum b){
    //返回两个大整数中长度较短的大整数
    
    	return a.length <= b.length? a : b ; 
    
    }
    
    largeNum largeNumAdd(largeNum a,largeNum b){
    //大整数的加法运算
    
    	largeNum sum;
    
    	if (a.length == b.length){
    
    		sum.length = a.length + 1;
    
    		sum = initlargeNumToZero(sum,sum.length);
    
    		for (int i = a.length - 1; i >= 0; i--){
    
    			sum.number[i+1] += (a.number[i]+ b.number[i]);
    
    			if (sum.number[i+1] > 9){				
    
    				sum.number[i] +=  (sum.number[i+1]/10);
    
    				sum.number[i+1] %= 10;
    
    			}
    			
    		}
    
    		return sum;		
    	
    	}else{
    
    		return largeNumAdd(formatAddZero(minLargeNumLength(a,b),max(a.length,b.length)),maxLargeNumLength(a,b));
    
    	}
    
    }
    
    int *largeNumCheckNum(int *a,int length){
    //检测大整数进位
    
    	for (int i = length-1; i >= 1; i--){		
    
    		if (a[i] > 9){	
    
    			a[i-1] += (a[i]/10);
    
    			a[i] %= 10;
    
    		}		
    
    	}
    	
    	if (a[0] > 9){
    
    			int *b;
    
    			b = (int*)malloc(sizeof(int)*(length+1));
    
    			b[0] = a[0]/10;
    			b[1] = a[0]%10;
    
    			for (int j = 2; j < length+1; j++){
    				
    				b[j] = a[j-1];
    
    			}
    
    			free(a);
    
    			return b;
    	}	
    
    	return a;
    
    }
    
    largeNum largeNumMultiply(largeNum a, largeNum b){
    //大整数的乘法
    
    	largeNum product;	
    
    	product.length = a.length + b.length;
    
    	int **tempProduct;
    
    	tempProduct = (int**)malloc(sizeof(int*)*a.length);
    
        for (int t = 0; t < a.length; t++){
    
            tempProduct[t] = (int*)malloc(sizeof(int)*b.length);
    
        }
    
    	product = initlargeNumToZero(product,product.length);
    
    	for (int j = b.length - 1; j >= 0; j--){
    		
    		for (int i = a.length - 1; i >= 0; i--){
    			
    			tempProduct[i][j] = a.number[i] * b.number[j];
    
    			product.number[i+j+1] += tempProduct[i][j];
    
    		}
    
    	}
    
    	product.number = largeNumCheckNum(product.number,product.length);
    
    	return product;
    }
    
    void largeNumPrintf(largeNum a){
     //输出大整数
    
    	int sign = 0;
    
    	for (int i = 0; i < a.length; i++){
    
    		if (a.number[i] != 0){
    
    			sign = 1;			
    
    		}	
    
    		if(!sign){			
    
    			if (i+1 == a.length){
    
    				printf("0");break;
    			}
    
    			continue;
    
    		}
    
    		printf("%d", a.number[i]);
    
    	}
    
    }
    
    
    
    
    Matrix initMatrix(largeNum a,largeNum b,largeNum c,largeNum d){
    //初始化一个2*2的矩阵
    	Matrix matrix;
    
    	matrix.RowCol00 = a;
    	matrix.RowCol01 = b;
    	matrix.RowCol10 = c;
    	matrix.RowCol11 = d;
    
    	return matrix;
    }
    
    void matrixPrintf(Matrix a){
    //输出矩阵
    
    	printf("|");largeNumPrintf(a.RowCol00);printf("\t");largeNumPrintf(a.RowCol01);printf("|\n");
    
    	printf("|");largeNumPrintf(a.RowCol10);printf("\t");largeNumPrintf(a.RowCol11);printf("| ");
    
    }
    
    
    Matrix matrixMultiply(Matrix a,Matrix b){
    //定义矩阵的乘法
    
    	Matrix product;
    
    	product.RowCol00 = largeNumAdd(largeNumMultiply(a.RowCol00,b.RowCol00),largeNumMultiply(a.RowCol01,b.RowCol10));
    	product.RowCol01 = largeNumAdd(largeNumMultiply(a.RowCol00,b.RowCol01),largeNumMultiply(a.RowCol01,b.RowCol11));
    	product.RowCol10 = largeNumAdd(largeNumMultiply(a.RowCol10,b.RowCol00),largeNumMultiply(a.RowCol11,b.RowCol10));
    	product.RowCol11 = largeNumAdd(largeNumMultiply(a.RowCol10,b.RowCol01),largeNumMultiply(a.RowCol11,b.RowCol11));
    
    	return product;
    }
    
    
    Matrix matrixPower(matrix a,int n){
    //定义矩阵的幂运算，这里用到了分治思想
    
    	if (n==1){
    
    		return a;
    
    	}else if (n==2){
    
    		return matrixMultiply(a,a);
    
    	}else if(n > 2 && n % 2 == 0){
    
    		return matrixMultiply(matrixPower(a,n/2),matrixPower(a,n/2));
    
    	}else{
    
    		return matrixMultiply(matrixMultiply(matrixPower(a,(n-1)/2),matrixPower(a,(n-1)/2)),a);
    
    	}
    
    }    
    ```
# 5.主函数
```
int main(){

	largeNum a,b,c,d;
	Matrix resault;
	int n = 100;

	int Anumber[1] = {1};int Bnumber[1] = {1};
	int Cnumber[1] = {1};int Dnumber[1] = {0};


	a = initlargeNumber(1,Anumber);b = initlargeNumber(1,Bnumber);
	c = initlargeNumber(1,Cnumber);d = initlargeNumber(1,Dnumber);
    
        //初始化四个大整数1，1，1，0
    
    
	Matrix matrix = initMatrix(a,b,c,d);
	 //初始化矩阵
```	
```math
matrix =
\left[
\begin{matrix}
1 & 1 \\
1 & 0 
\end{matrix}
\right]
```
```
    resault = matrixPower(matrix,n-1);
             //计算矩阵的（n-1）次方
    

	matrixPrintf(matrix);printf("^");printf("(%d-1)\t", n);printf("=\n\n");

	matrixPrintf(resault);

	printf("\n\n");

	printf("f(%d)=",n);

	largeNumPrintf(resault.RowCol00);

	printf("\n\n");

	return 0;
}


```


