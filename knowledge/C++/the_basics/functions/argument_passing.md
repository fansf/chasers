# C6.2 参数传递
每次调用函数时都会重新传递创建它的形参，并用传入实参对形参进行初始化。

和其他变量一样，形参的类型决定了形参和实参交互的方式。如果形参是引用类型，它将绑定到对应的实参上；否则，将实参的值拷贝后赋给形参。

当形参是引用类型时，我们说它对应的实参被**引用传递（passed by reference）** 或者函数被**传引用调用（passed by reference）** 。和其他引用一样，引用形参也是它绑定的对象的别名；也就是说，引用形参是它对应的实参的别名。

当实参的值被拷贝给形参时，形参和实参是两个相互独立的对象。我们说这样的实参被**值传递（passed by value）** 或者函数被**传值调用（called by value）**。

## 传值参数
当初始化一个非引用类型的变量时，初始值被拷贝给变量。此时，对变量的改动不会影响初始值：
```cpp
int n = 0;
int i = n;    //i是n的副本
i = 42;       //i的值改变；n的值不变
```

传值参数的机理完全一样，函数对形参做的所有操作都不会影响实参。

### 指针形参
指针的行为和其他非引用类型一样。当执行指针拷贝操作时，拷贝的是指针的值。拷贝之后，两个指针是不同的指针。因为指针使我们可以间接地访问它所指的对象，所以通过指针可以修改它所指对象的值：
```cpp
int n = 0,i = 42;
int *p = &n,*q = &i;
*p = 42;                //n的值改变；p不变
p = q;                  //p现在指向i；但是i和n的值都不变
```

指针形参的行为与之类似：
```cpp
void reset(int *p)
{
    *p = 0;
    p = 0;    //只改变了p的局部拷贝，实参未被改变
}
```

调用`reset`函数之后，实参所指的对象被置为`0`，但是实参本身并没有改变：
```cpp
int i = 42;
reset(&i);
```

## 传引用参数
对于引用的操作实际上是作用在引用所引的对象上，引用形参的行为与之类似。通过使用引用形参，允许函数改变一个或多个实参的值。

```cpp
void reset(int &i)
{
    i = 0;
}
```

和引用一样，引用形参绑定初始化它的对象。当调用这一版本的`reset`函数时，`i`绑定我们传给函数的`int`对象，此时改变`i`所引对象的值。

```cpp
int j = 42;
reset(j);
```

在上述调用中，形参`i`仅仅是`j`的又一个名字，在`reset`内部对`i`的使用即是对`j`的使用。

### 使用引用避免拷贝
拷贝大的类类型对象或者容器对象比较低效，甚至有的类类型（包括IO类型在内）根本不支持拷贝操作。当某种类型不支持拷贝操作时，函数只能通过引用形参访问该类型的对象。

### 使用引用形参返回额外信息
一个函数只能返回一个值，然而有时函数需要同时返回多个值，引用形参为我们一次返回多个结果提供了有效的途径。

举个例子，我们定义一个名为`find_char`的函数，它返回在`string`对象中某个指定字符第一次出现的位置。同时，我们也希望函数能返回该字符出现的总次数。

我们可以给函数传入一个额外的引用实参，令其保存字符出现的次数

```cpp
string::size_type find_char(const string &s,char c,string::size_type &occurs)
{
    auto ret = s.size();
    occurs = 0;
    for (decltype(ret) i = 0;i != s.size();++i){
        if (s[i] == c){
            if (ret == s.size())
                ret = i;
            ++occurs;
        }
    }
    return ret;
}
```

## const形参和实参
当形参是`const`时，必须要注意关于顶层`const`的讨论。如前所述，顶层`const`作用于对象本身：
```cpp
const int ci = 42;    //不能改变ci，const时顶层的
int i = ci;           //正确：当拷贝ci时，忽略了它的顶层const
int *const p = &i;    //const时顶层的，不能给p赋值
*p = 0;               //正确：通过p改变对象的内容是允许的，现在i变成了0
```

和其他初始化一样，当用实参初始化形参时会忽略调顶层`const`。换句话说，形参的顶层`const`被忽略掉了。当形参有顶层`const`时，传给它常量对象或者非常量对象都是可以的：
```cpp
void fcn(const int i){/*fcn可以读取i，但是不能向i写值*/}
```

调用`fcn`函数时，既可以传入`const int`也可以传入`int`。忽略掉形参的顶层`const`可能产生意想不到的结果：
```cpp
void fcn(const int i) {}
void fcn(int i){}        //错误：重复定义了fcn(int)
```

在C++语言中，允许我们定义若干个具有相同名字的函数，不过前提是不同函数的形参列表应该有明显的区别。因为顶层`const`被忽略掉了，所以在上面的代码中传入两个`fcn`函数的参数可以完全一样。因此第二个`fcn`是错误的，尽管形式上有差异，但实际上它的形参和第一个`fcn`的形参没有什么不同。

### 指针或引用形参与const
形参的初始化方式和变量的初始化方式是一样的，所以回顾通用的初始化规则有助于理解本节知识。我们可以使用非常量初始化一个底层`const`对象，但是反过来不行；同时一个普通的引用必须用同类型的对象初始化。
```cpp
int i = 42;
const int *cp = &i        //正确：但是cp不能改变i
const int &r = i;         //正确：但是r不能改变i
const int &r2 = 42;       //正确：
int *p = cp;              //错误：p的类型和cp的类型不匹配
int &r3 = r;              //错误：r3的类型和r不匹配
int &r4 = 42;             //错误：不能用字面值初始化一个非常量引用
```

将同样的初始化规则应用到参数传递上可得如下的形式：
```cpp
int i = 0;                    
const int ci = i;             
string::size_type ctr = 0;    
reset(&i);                    //调用形参类型是int*的reset函数
reset(&ci);                   //错误：不能用指向cosnt int对象的指针初始化int*
reset(i);                     //调用形参类型是int&的reset函数
reset(ci);                    //错误：不能把普通引用绑定在const对象ci上
reset(42);                    //错误：不能把普通类型绑定到字面值上
reset(ctr);                   //错误：类型不匹配，ctr是无符号类型
```

要向调用引用版本的`reset`，只能使用`int`类型的对象，而不能使用字面值、求值结果为`int`表达式、需要转换的对象或者`const int`类型的对象。类似的，要想调用指针版本的`reset`只能使用`int*`。另一方面，我们能传递一个字符串字面值作为`find_char`的第一个实参，这是因为该函数的引用形参是常量引用，而C++允许我们用字面值初始化常量引用。

### 尽量使用常量引用
把函数不会改变的形参定义成（普通的）的引用是一种比较常见的错误，这么做带给函数的调用者一种误导，即函数可以修改它的实参的值。此外，使用引用而非常量引用也会极大地限制函数所能接受的实参类型。

这种错误绝不像看起来那么简单，它可能造成出人意料的后果。我们以`find_char`函数为例，那个函数正确地将它的`string`类型的形参定义成常量引用。假如我们把它定义成普通的`string&`：
```cpp
string::size_type find_char(string &s,char c,string::size_type &occurs);
```

则只能将`find_char`函数作用于`string`对象。类似下面的调用：
```cpp
find_char("Hello world",'o',ctr);
```
将在编译时发生错误。

## 数组形参
数组的两个特殊性质对我们定义和使用作用在数组上的函数有影响，这两个性质分别是：
- 不允许数组拷贝
- 使用数组时（通常）会将其转换成指针。

因为不能拷贝数组，所以我们无法以值传递的方式使用数组参数。因为数组会被转换成指针，所以当我们为函数传递一个数组时，实际上传递的是指向数组首元素的指针。

尽管不能以值传递的方式传递数组，但是我们可以把形参写成类似数组的形式：
```cpp
void print(const int*);
void print(const int[]);
void print(const int[10]);    //这里的维度表示我们期望数组含有多少元素，实际不一定
```

尽管表现形式不同，但上面的三个函数时等价的；每个函数的唯一形参都是`const int*`类型的。当编译器处理`print`函数的调用时，只检查传入的参数是否是`const int*`类型：
```cpp
int i = 0,j[2] = {0,1};
print(&i);
print(j);                //正确：j转换成int*并指向j[0]
```

如果我们传给`print`函数的是一个数组，则实参自动地转换成指向数组首元素的指针，数组的大小对函数的调用没有影响。

因为数组是以指针形式传递给函数的，所以一开始函数并不知道数组的确切大小，调用者应该为此提供一些额外信息。管理指针形参有三种常用的技术。

### 使用标记指定数组长度
管理数组实参的第一种方法是要求数组本身包含一个结束标记，使用这种方法的典型示例是C风格字符串。C风格字符串存储在字符数组中，并且在最后一个字符后面跟这一个空白字符。函数在处理C风格字符串时遇见空白字符停止：
```cpp
void print(const char*cp)
{
    if (cp){
        while (*cp)
            cout << *cp++;
    }
}
```

这种方式适用于那些有明显结束标记且标记不会与普通数据混淆的情况，但是对于像`int`这样所有取值都是合法值的数据组就不太有效了。

### 使用标准库规范
管理数组实参的第二种技术是传递指向数组首元素和尾后元素的指针，这种方法受到了标准库技术的启发。我们可以按照如下格式输出元素的内容：
```cpp
void print(const int *beg,const int *end)
{
    while (beg != end)
        cout << *beg++ << endl;
}
```

为了调用这个函数，我们需要传入两个指针：
- 指向要输出的首元素
- 指向尾元素的下一个位置

```cpp
int j[2] = {0,1};
print(begin(j),end(j));
```

### 显式传递一个表示数组大小的形参
第三种管理数组实参的方法是专门定义一个表示数组大小的形参。
```cpp
void print(const int ia[],size_t size)
{
    for (size_t i = 0; i != size; ++i)
        cout << ia[i] << endl;
}
```

### 数组形参和const
我们的三个`print`函数都把数组形参定义成了指向`const`的指针，关于引用的讨论同样适用于指针。当函数不需要对数组元素执行写操作的时候，数组形参应该是指向`const`的指针。只有当函数确实要改变元素值的时候，才把形参定义成指向非常量的指针。

### 数组引用形参
c++语言允许将变量定义成数组的引用，给予同样的道理，形参也可以是数组的引用。此时，引用形参绑定到对应的实参上，也就是绑定到数组上:
```cpp
void print(int (&arr)[10])
{
    for (auto elem : arr)
        cout << elem << endl;
}
```

因为数组的大小是构成数组类型的一部分，所以只要不超过维度，在函数体内就可以放心地使用数组。但是，这以用法也无形中限制了`print`函数的可用性，我们只能将函数作用于大小为10的数组。

### 传递多维数组
和所有数组一样，当多维数组传递给函数时，真正传递的是指向数组首元素的指针。因为我们处理的是数组的数组，所以首元素本身就是一个数组，指针就是一个指向数组的指针。数组的第二维度（以及后面所有维度）的大小都是数组类型的一部分，不能省略：
```cpp
void print(int (*matrix)[10],int rowsize);
```
