# C6.7 函数指针
函数指针指向的是函数而非对象。和其他指针一样，函数指针指向某种特定类型。函数的类型由它的返回类型和形参类型共同决定，与函数名无关。

例如：
```cpp
bool lengthCompare(const string &,const string &);
```

该函数的类型是`bool(const string&,const string&)`。要想声明一个可以指向该函数的指针，只需要用指针替换函数名即可：
```cpp
bool (*pf)(const string &,const string &);
```

从我们声明的名字开始，`pf`前面有一个`*`，因此`pf`是指针；右侧是形参列表，表示`pf`指向的是函数；在观察左侧，发现函数的返回类型是布尔值。因此，`pf`就是一个指向函数的指针，其中该函数的参数是两个`const string`的引用，返回值是`bool`类型。

Note：  
> `*pf`两端的括号比不可少。如果不写这对括号，则`pf`是一个返回值为`bool`指针的函数  
> `bool *pf(const string &,const string &);`

**使用函数指针**

当我们把函数名作为一个值使用时，该函数自动地转换成指针。

例如，按照如下形式我们可以将`lengthCompare`的地址赋给`pf`：
```cpp
pf = lengthCompare;            //pf指向名为lengthCompare的函数
pf = &lengthCompare;           //等价的赋值语句：取地址符是可选的
```

此外，我们还可以直接使用指向函数的指针调用该函数，无须提前解引用指针：
```cpp
bool b1 = pf("hello","it's me");            //调用lengthCompare
bool b2 = (*pf)("hello","it's me");         //一个等价的调用
bool b3 = lengthCompare("hello","it's me"); //另一个等价的调用
```

在指向不同函数类型的指针之间不存在转换规则。但是和往常一样，我们可以为函数指针赋一个`nullptr`或者值为0的整型常量表达式，表示该指针没有指向任何一个函数：
```cpp
int size();
bool cstringCompare(const char*,const char*);
pf = 0;                 //正确：pf不指向任何函数
pf = size();            //错误：返回类型不匹配
pf = cstringCompare;    //错误：形参类型不匹配
pf = lengthCompare;     //正确：函数和指针的类型精确匹配
```

**重载函数的指针**  

当我们使用重载函数时，上下文必须清晰地界定到底该选择哪个函数。

如果定义了指向重载函数的指针：
```cpp
void ff(int*);
void ff(unsigned int);

void (*pf1)(unsigned int) = ff;    //pf1指向ff(unsigned)
```

编译器通过指针类型确定选用哪个函数，指针类型必须与重载函数中的某一个精确匹配：
```cpp
void (*pf2)(int) = ff;             //错误：没有任何一个ff与该形参列表匹配
double (*pf3)(int*) = ff;          //错误：ff和pf3的返回类型不匹配
```

**函数指针形参**

和数组类似，虽然不能定义函数类型的形参，但是形参可以是指向函数的指针。此时，形参看起来是函数类型，实际上却是当成指针使用：
```cpp
void useBigger(const string &s1,const string &s2,bool pf(const string &,const string &));
//等价的声明：显式的将形参定义成指向函数的指针
void useBigger(const string &s1,const string &s2,bool (*pf)(const string &,const string &));
```

我们可以直接把函数作为实参使用，此时它会自动转换成指针：
```cpp
useBigger(s1,s2,lengthCompare);
```

我们可以使用类型别名和`decltype`简化函数指针的代码：
```cpp
//func和func2是函数类型
typedef bool func(const string&,const string&);
typedef decltype(lengthCompare) func2;

//funcp和funcp2是指向函数的指针
typedef bool(*funcp)(const string&,const string&);
typedef decltype(lengthCompare) *funcp2;
```

**返回指向函数的指针**

和数组类似，虽然不能返回一个函数，但是能返回指向函数类型的指针。然而，我们必须把返回类型写成指针形式，编译器不会自动地将函数返回类型当成对应的指针类型处理。和往常一样，要想声明一个返回函数指针的函数，最简单的办法是使用类型别名：
```cpp
using F = int(int*,int);            //F是函数类型
using FP = int(*)(int *,int);       //FP是指针类型
```

必须时刻注意的是，和函数类型的形参不一样，返回类型不会自动地转换成指针。我们必须显式的将返回类型指定为指针：
```cpp
FP f1(int);                         //正确：FP是指向函数的指针，f1返回指向函数的指针
F *f1(int);                         //正确：显式的指定返回类型是指向函数的指针
F f1(int);                          //错误：F是函数类型，f1不能返回一个函数
```

当然，我们也能用下面的形式直接声明`f1`：
```cpp
int (*f1(int))(int *,int);
```

按照由内向外的顺序阅读这条声明语句：我们看到`f1`有形参列表，所以`f1`是个函数；`f1`前面有`*`，所以`f1`返回一个指针；进一步观察，指针类型本身也包含形参列表，因此指针指向函数，该函数的返回类型是`int`。

我们也可以使用尾置返回类型的方式声明一个返回函数指针的函数：
```cpp
auto f1(int) -> int (*)(int*,int);
```