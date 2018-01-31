# 子集生成
本节介绍子集生成算法：给定一个集合，枚举所有可能的子集。

为了简单起见，本节讨论的集合中没有重复元素。

## 增量构造法
一. 输出由1 ~ n组成的集合的子集。

每次选出一个元素放入序列中，与前一小节一样，集合由序列完全表示。

```cpp
void subset(int a[],int n,int cur)
{
    for (int i = 0;i < cur;++i)
        cout << a[i] << ' ';
    cout << endl;

    int s = cur ? a[cur - 1] + 1 : 0;        //判断当前的输出位置
    for (int i = s;i < n;++i){
        a[cur] = i;
        subset(a,n,cur + 1);
    }
}
```

二. 输出某一集合的子集。

当我们手写一个集合的子集时，总按照以下规律书写：
- 从第一个数开始，依次从剩下的数中挑选一个数组成一个子集（比如：1,2 1,3 1,4）
- 挑选上一次选择的数后面的数加入子集，形成新的子集（比如：1,2,3 1,3,5 1,4,6）
- 重复，直到数没有剩余

所以，我们的代码可以写成：
```cpp
void subset(int p[],int a[],int n,int cur,int m)
{
    for (int i = 0;i < cur;++i)
        cout << a[i] << ' ';
    cout << endl;

    for (int i = m;i < n;++i){
        a[cur] = p[i];
        subset(p,a,n,cur + 1,i + 1);
    }
}
```

`cur`表示当前将填入输出序列的位置。  
`m`表示“选择从m位置开始的数”。如果集合中还有可以加入的数，那么这个数总是位于m位置之后。  

由于在函数调用时，`cur`和`m`总是`0`，所以我们可以把函数声明为`void subset(int p[],int a[],int n,int cur = 0,int m = 0`，使用默认实参可以免去填写`0`的麻烦，这时我们只需调用`subset(p,a,n)`即可。

## 位向量法
第二种方法时构造一个位向量`b[i]`，而不是直接构造子集a本身，当且仅当`p[i]`在子集内时，`b[i]`等于`true`。

```cpp
void subset(int p[],bool b[],int n,int cur)
{
    if (cur == n){
        for (int i = 0;i < cur;++i){
            if (b[i])
                cout << p[i] << ' ';
        }
        cout << endl;
        return;
    }
    b[cur] = true;
    subset(p,b,n,cur + 1);
    b[cur] = false;
    subset(p,b,n,cur +1);
}
```

只有当确定所有元素“是否选择”后，我们确定一个完整的子集。因此，只有当`cur == n`时，我们才能打印一个子集。