# 枚举排列
一个经常遇见的问题是，输入整数n，按字典序从小到大的循序输出前n个数的所有排列。如果你不知道什么是字典序，可以到[这儿](/knowledge/C++/the_basics/strings_vectors_arrays/library_string_type.md)查看（在 比较string对象小节）
## 生成1 ~ n的排列
我们尝试用递归地思想解决问题：先输出所有以1开头的排列（递归调用），然后输出以2开头的排列（又是递归调用），接着是以3开头的排列...，最后是以n开头的排列。

以1开头的排列特点是：第一位是1，后面是2 ~ 9的排列。2 ~ 9也需要“按照字典序输出排列”，不过需要注意的是，在输出时，每个排列的最前面要加上“1”。

这样，递归函数需要一下参数：
- 已经确定的“前缀”序列，以便于输出。
- 需要进行全排列的元素集合，以便依次选做第一个元素。

我们可以得到一个伪码：
```cpp
void permutation(序列A,集合B)
{
    if (B为空)
        输出序列A;
    else 按照从小到大的顺序一次考虑B的每个元素v，每一次递归添加一个元素
    {
        permutation(在A的末尾添加v后得到的新序列,B - {v});
    }
}
```

下面考虑程序实现。我们用一个数组A表示序列，而集合B不用保存，因为集合B可以由序列A确定（序列A含有集合B中的所有元素，只不过每个序列的元素顺序不一样而已），我们从1遍历到n，表示我们将填入序列的集合，只要是序列A中没有出现的元素，都是可以填入序列A的。我们使用n表示数组元素个数，用cur表示当前序列的长度（元素个数）。

```cpp
void permutation(int a[],int n,int cur)
{
    if (cur == n){                                //递归边界，此时集合为空，输出序列A
        for (int i = 0;i < n;++i)
            cout << a[i];
        cout << endl;
    }
    else{
        for (int i = 1;i <= n;++i){               //每次递归向序列中放入一个元素，用循环表示集合1 ~ n，如果该元素未在序列中出现，则填入序列末尾（a[cur]）。
            bool ok = true;
            for (int j = 0;j < cur;++j){          //如果该元素在序列中出现过，则不能再选该元素
                if (a[j] == i){
                    ok = false;
                    break;
                }
            }
            if (ok){
                a[cur] = i;
                permutation(a,n,cur + 1);        //递归调用
            }
        }
    }
}
```

声明一个足够大的数组a，然后调用`permutation(a,n,0)`，即可按字典序输出1 ~ n的所有排列。

## 生成可重集的排列
上面的程序只能生成1 ~ n的所有排列，但有些时候我们需要指定一个输入集合，然后输出该集合的所有排列。为了达到该目的，我们只需将p加入到函数的参数列表中，并将`a[j] == i`改写成`a[j] == p[i]`，`a[cur] = i`改写成`a[cur] = p[i]`。因为在上面的程序中，变量`i`相当于是集合中的元素（巧合的是，元素和数组的地址值一一对应）。当我们需要输入一个集合p后，每一个元素的值应该为`p[i]`而非`i`。这样，只要把p中所有元素按从小到大排序，然后调用`permutation(p,a,n,0)`即可。

```cpp
void permutation(int p[],int a[],int n,int cur)
{
    if (cur == n){                                //递归边界，此时集合为空，输出序列A
        for (int i = 0;i < n;++i)
            cout << a[i];
        cout << endl;
    }
    else{
        for (int i = 0;i < n;++i){               //每次递归向序列中放入一个元素，用数组p表示集合，用p[i]访问集合中的元素，如果该元素未在序列中出现，则填入序列末尾（a[cur]）。
            bool ok = true;
            for (int j = 0;j < cur;++j){          //如果该元素在序列中出现过，则不能再选该元素
                if (a[j] == p[i]){
                    ok = false;
                    break;
                }
            }
            if (ok){
                a[cur] = p[i];
                permutation(p,a,n,cur + 1);        //递归调用
            }
        }
    }
}
```

可惜的是，上面的方法有一个小问题，程序无法输出完整的含有重复元素的排列。例如如1 1 1，程序将没有任何输出。因为程序禁止在数组a中出现重复的元素（判断重复和判断是否出现过 在逻辑上冲突）。

为了解决这个问题，我们可以判断序列中`p[i]`出现的次数`c1`和集合中`p[i]`出现的次数`c2`，只要`c1`小于`c2`，那么就可以把当前元素插入序列中：
```cpp
else{
    for (int i = 0;i < n;++i){
        int c1 = 0,c2 = 0;
        for (int j = 0;j < cur;++j){
            if (a[j] == p[i])
                ++c1;
        }
        for (int j = 0;j < n;++j){
            if (p[j] == p[i])
                ++c2;
        }
        if (c1 < c2){
            a[cur] = p[i];
            permutation(p,a,n,cur + 1);
        }
    }
}
```

不过又出现一个小问题：重复。序列`1 1` 和 序列`1 1`（两个1交换位置）是相同的。  

因为集合p已经排好序，所以我们只需检查`p[i]`是否和`p[i - 1]`相同就行了。如果相同，就说明将发生重复。

```cpp
else{
    for (int i = 0;i < n;++i){
        if (!i || p[i] != p[i - 1]){                //检查p[i]是否和p[i - 1]相同
            int c1 = 0,c2 = 0;
            for (int j = 0;j < cur;++j){
                if (a[j] == p[i])
                    ++c1;
            }
            for (int j = 0;j < n;++j){
                if (p[j] == p[i])
                    ++c2;
            }
            if (c1 < c2){
                a[cur] = p[i];
                permutation(p,a,n,cur + 1);
            }
        }
    }
}
```