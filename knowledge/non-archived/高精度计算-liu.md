# 高精度计算-liu
## 前言  
由于计算机运算是有模运算,数据范围的表示有一定限制，如整型int(C++中int 与long相同)表达范围是(-2^31~2^31-1)，unsigned long(无符号整数)是(0~2^32-1)，都约为几十亿。如果采用实数型,则能保存最大的double只能提供15~16位的有效数字，即只能精确表达数百万亿的数。因此，在计算位数超过十几位的数时，不能采用现有类型，只能自己编程计算。 

高精度计算通用方法:高精度计算时一般用一个数组来存储一个数,数组的一个元素对应于数的一位，表示时,由于数计算时可能要进位，因此为了方便，将数由低位到高位依次存在数组下标对应由低到高位置上。另外，我们申请数组大小时，一般考虑了最大的情况，在很多情况下，表示有富余，即高位有很多0，可能造成无效的运算和判断，因此，我们一般将数组的第0个下标对应位置来存储该数的位数。  

高精度运算涉及到的问题：  

（1） 数据的输入  
（2） 数据的存储  
（3）数据的运算：进位和借位  
（4）结果的输出：小数点的位置和处于多余的0  
 

## 高精度数存储  
对数采用字符串输入，再转换成整型数组：  
```cpp
#include <iostream> 
#include <cstring>  
  
using namespace std;  

const int N=100;//最多100位  

int main()  
{  
	int a[N+1],i;  
	string s1;  
	cin>>s1;//数s1  
	memset(a,0,sizeof(a)); //数组清0  
	a[0]=s1.length(); //位数  
	for(i=1;i<=a[0];i++)  
		a[i]=s1[a[0]-i]-'0';//将字符转为数字并倒序存储．  
	return 0;  
}  
```
## 高精度数比较  

```cpp
int compare(int a[],int b[])   //比较a和b的大小关系，若a>b则为1，a<b则为-1,a=b则为0  
{  
	int i;  
	if (a[0]>b[0])   
		return 1;//a的位数大于b则a比b大  
 	if (a[0]<b[0])  
		return -1;//a的位数小于b则a比b小  
	for(i=a[0];i>0;i--)  //从高位到低位比较  
    {  
		if (a[i]>b[i])   
			return 1;  
    	if (a[i]<b[i])  
			return -1;  
	}  
	return 0;//各位都相等则两数相等。  
}  
```

## 高精度加法  
  
```cpp
int plus(int a[],int b[]) //计算a=a+b  
{  
	int i,k;  
	k=a[0]>b[0]?a[0]:b[0]; //k是a和b中位数最大的一个的位数  
	for(i=1;i<=k;i++)  
    {  
		a[i+1]+=(a[i]+b[i])/10;  //若有进位，则先进位  
    	a[i]=(a[i]+b[i])%10;  
	}  //计算当前位数字,注意：这条语句与上一条不能交换。  
	if(a[k+1]>0)  
		a[0]=k+1;  //修正新的a的位数（a+b最多只能的一个进位）  
    else  
		a[0]=k;  
	return 0;  
}  
```

## 高精度减法  

和高精度加法相比，减法在差为负数时处理的细节更多一点：当被减数小于减数时，差为负数，差的绝对值是减数减去被减数；  
```cpp
int gminus(int a[],int b[]);//计算a=a-b，返加符号位0:正数 1:负数  
{  
	int flag,i;  
	flag=compare(a,b); //调用比较函数判断大小  
	if (falg==0)//相等  
	{  
		memset(a,0,sizeof(a));  
		return 0;  
	}  //若a=b，则a=0,也可在return前加一句a[0]=1,表示是 1位数0  
	if(flag==1) //大于  
	{  
		for(i=1;i<=a[0];i++)  
    	{ 
			if(a[i]<b[i])  
			{  
				a[i+1]--;  
				a[i]+=10;  
			} //若不够减则向上借一位  
        	a[i]=a[i]-b[i];}  
     	while(a[a[0]]==0)  
			a[0]--; //修正a的位数  
    	return 0;  
	}  
	if (flag==-1)//小于  则用a=b-a,返回-1  
    {  
		for(i=1;i<=b[0];i++)  
		{  
			if(b[i]<a[i])  
  			{  
				b[i+1]--;  
				b[i]+=10;  
			} //若不够减则向上借一位  
        	a[i]=b[i]-a[i];  
		}  
    	a[0]=b[0];  
    	while(a[a[0]]==0)  
			a[0]--; //修正a的位数  
   	return -1;  
	}  
}  
```

## 高精度乘法  

高精度乘以低精度；
```cpp
int multi1(int a[],long  key) //a=a*key,key是单精度数  
{  
	int i,k;  
	if (key==0)  
	{  
		memset(a,0,sizeof(a));  
		a[0]=1;  
		return 0;  
	} //单独处理key=0  
	for(i=1;i<=a[0];i++)  
		a[i]=a[i]*key;//先每位乘起来  
	for(i=1;i<=a[0];i++)  
	{  
		a[i+1]+=a[i]/10;  
		a[i]%=10;  
	} //进位  
	//注意上一语句退出时i=a[0]+1  
	while(a[i]>0)  
	{  
		a[i+1]=a[i]/10;  
		a[i]=a[i]%10;  
		i++;  
		a[0]++];  
	}  //继续处理超过原a[0]位数的进位,修正a的位数  
	return 0;  
}  
```

高精度乘以高精度；  
用数组保存两个高精度数，然后逐位相乘，注意考虑进位和总位数。  
```cpp
#include<iostream>  

using namespace std;  

int main()  
{  
  	int  a[240] = {0}, b[240] = {0}, c[480] = {0};  
  	int  i, j, ka, kb, k;  
  	char  a1[240], b1[240];  
  	gets(a1);   
  	ka = strlen(a1);  
  	gets(b1);   
  	kb = strlen(b1);  
  	k = ka + kb;  
  	for(i = 0; i < ka; i++)  a[i] = a1[ka-i-1] - '0';  
  	for(i = 0; i < kb; i++)  b[i] = b1[kb-i-1] - '0';  
  	for(i = 0; i < ka; i++)  
    	for(j = 0; j < kb; j++)  
    	{  
      		c[i + j] = c[i + j] + a[i] * b[j];  
      		c[i + j +1] = c[i + j +1] + c[i + j]/10;  
      		c[i + j] = c[i + j] % 10;  
    	}  
  	if(!c[k])  
		k--;  
  	for(i = k-1; i >= 0; i--)  
  		cout<<c[i];   
}  
```

## 高精度除法  

高精度除以低精度；  
按照从高位到低位的顺序，逐位相除。在除到第j位时，该位在接受了来自第j+1位的余数后与除数相除，如果最高位为零，则商的长度减一。  
```cpp
#include  <stdio.h>  
#define   N  500  

main()  
{  
  	int  a[N] = {0}, c[N] = {0};  
  	int  i, k, d, b;  
  	char  a1[N];  
  	printf("Input 除数:");  
  	scanf("%d", &b);  
  	printf("Input 被除数:");  
  	scanf("%s", a1);  
  	k = strlen(a1);  
  	for(i = 0; i < k; i++)  
		a[i] = a1[k - i - 1] - '0';  
  	d = 0;  
  	for(i = k - 1; i >= 0 ; i--)  
  	{  
  		d = d * 10 + a[i];  
    	c[i] = d / b;  
    	d = d % b;      
  	}   
  	while(c[k - 1] == 0 && k > 1)  k--;  
  	printf("商=");  
 	for(i = k - 1; i >= 0; i--)  
	 	printf("%d", c[i]);  
 	printf("\n余数=%d", d);   
}  
```

高精度除以高精度；  
用计算机模拟手算除法，把除法试商转化为连减。  
```cpp
#include<iostream>  
#include<cstdio>  
#include<cstring>  

using namespace std;  

int a[301],b[301],c[301];  

void prin(int a[])  
{  
  	if(a[0]==0)  
	{  
		cout<<0<<endl;  
		return;  
	}  
	for(int i=a[0];i>=1;i--)  
		cout<<a[i];  
	cout<<endl;  
}  

void init(int a[])  
{  
	string s;  
	cin>>s;  
	a[0]=s.size();  
	for(int i=1;i<=a[0];i++)  
		a[i]=s[a[0]-i]-'0';	 
}  

int comp(int a[],int b[])  
{  
	if(a[0]>b[0])  
		return 1;  
	else if(a[0]<b[0])  
		return -1;  
	else  
	{  
		for(int i=a[0];i>0;i--)  
		{  
			if(a[i]>b[i])  
				return 1;  
			if(a[i]<b[i])  
				return -1;  
		}  
	}  
	return 0;  
}  

void jian(int a[],int b[])  
{  
	int flag;  
	flag=comp(a,b);  
	if(flag==0)  
	{  
		a[0]=0;  
		return;  
	}  
	if(flag==1)  
	{  
		for(int i=1;i<=a[0];i++)  
		{  
			if(a[i]<b[i])  
			{  
				a[i+1]--;  
				a[i]+=10;  
			}  
			a[i]-=b[i];  
		}  
		while(a[0]>0&&a[a[0]]==0)  
			a[0]--;  
	}	 
	return;  
}  

void cpy(int a[],int b[],int num)  
{  
	for(int j=1;j<=a[0];j++)  
			b[j+num-1]=a[j];  
	b[0]=a[0]+num-1;  
}  

void chu(int a[],int b[],int c[])  
{  
	int temp[301];  
	int i;  
	c[0]=a[0]-b[0]+1;  
	for(i=c[0];i>0;i--)  
	{  
		memset(temp,0,sizeof(temp));  
		cpy(b,temp,i);  
		while(comp(a,temp)>=0)  
		{  
			c[i]++;  
			jian(a,temp);  
		}  
	}  
	while(c[0]>0&&c[c[0]]==0)  
		c[0]--;  
	return;  
}  

int main()  
{  
	memset(a,0,sizeof(a));  
	memset(b,0,sizeof(b));  
	memset(c,0,sizeof(c));  
	//输入除数与被除数并初始化///  
	init(a);  
	getchar();  
	init(b);  
	//a除b，商存在c中，余数存在c中///  
	chu(a,b,c);  
	prin(c);  
	prin(a);  
  	return 0;  
 }  
```