# 01背包问题
## 问题描述  
一个背包总容量为`V`，现在有`N`个物品，第`i`个 物品体积为`weight[i]`，价值为`value[i]`，现在往背包里面装东西，怎么装能使背包的内物品价值最大？  

## 解决思路 
动态规划先找出子问题，我们可以这样考虑：在物品比较少，背包容量比较小时怎么解决？用一个数组`f[i][j]`表示，在只有`i`个物品，容量为j的情况下背包问题的最优解，那么当物品种类变大为`i+1`时，最优解是什么？第`i+1`个物品可以选择放进背包或者不放进背包（这也就是0和1），假设放进背包（前提是放得下），那么`f[i+1][j]=f[i][j-weight[i+1]+value[i+1]`；如果不放进背包，那么`f[i+1][j]=f[i][j]`。  

这就得出了状态转移方程：  
`f[i+1][j]=max(f[i][j],f[i][j-weight[i+1]+value[i+1])。`  

可以进一步优化内存使用。上面计算`f[i][j]`可以看出，在计算`f[i][j]`时只使用了`f[i-1][0……j]`，没有使用其他子问题，因此在存储子问题的解时，只存储`f[i-1]`子问题的解即可。这样可以用两个一维数组解决，一个存储子问题，一个存储正在解决的子问题。  
再进一步思考，计算`f[i][j]`时只使用了`f[i-1][0……j]`，没有使用`f[i-1][j+1]`这样的话，我们先计算j的循环时，让`j=M……1`，只使用一个一维数组即可。  

`for i=1……N`  
`for j=M……1`  
`f[j]=max(f[j],f[j-weight[i]+value[i])`   

## 实现代码  
优化前：
```cpp
#include<iostream>   
using namespace std;  
#define  V 1500  
unsigned int f[10][V];//全局变量，自动初始化为0  
unsigned int weight[10];  
unsigned int value[10];  
#define  max(x,y)	(x)>(y)?(x):(y)  
int main()  
{  
	int N,M;  
	cin>>N;//物品个数  
	cin>>M;//背包容量  
	for (int i=1;i<=N; i++)  
	{  
		cin>>weight[i]>>value[i];  
	}  
	for (int i=1; i<=N; i++)  
		for (int j=1; j<=M; j++)  
		{  
			if (weight[i]<=j)   
				f[i][j]=max(f[i-1][j],f[i-1][j-weight[i]]+value[i]);  
			else  
				f[i][j]=f[i-1][j];  
		}  
	cout<<f[N][M]<<endl;//输出最优解  
}  
```

优化后：
```cpp
#include<iostream>  
using namespace std;  
#define  V 1500  
unsigned int f[V];//全局变量，自动初始化为0  
unsigned int weight[10];  
unsigned int value[10];  
#define  max(x,y)	(x)>(y)?(x):(y)  
int main()  
{  
	int N,M;  
	cin>>N;//物品个数  
	cin>>M;//背包容量  
	for (int i=1;i<=N; i++)  
	{  
		cin>>weight[i]>>value[i];  
	}  
	for (int i=1; i<=N; i++)  
		for (int j=M; j>=1; j--)  
			if (weight[i]<=j)  
			{  
				f[j]=max(f[j],f[j-weight[i]]+value[i]);  
			}			
	cout<<f[M]<<endl;//输出最优解  
}  
```