[TOC]

# C2.6 自定义数据结构
从最基本的层面理解，数据结构是把一组相关的数据元素组织起来然后使用它们的策略和方法。

## 定义Sales_data类型
尽管我们还 写不出一个完整的`Sales_item`类，但是可以尝试把这些元素组织到一起形成一个简单点儿的类。初步的想法是用户能够直接访问其中的数据元素，也能实现一些基本的操作。

既然我们筹划的这个数据结构不带有任何运算功能，不妨把它命名为`Sales_data`以示与`Sales_item`的区别。  
`Sales_data`初步定义如下：
```cpp
struct Sales_data{
    std::string boolNo;
    usingned units_sold = 0;
    double revenue = 0.0;
};
```

我们的类以关键字`struct`开始，紧跟着类名和类体（其中类体可以分为空）。类体由花括号包围形成了一个新的作用域。类内部定义的名字必须唯一，但是可以与类外部定义的名字重复。

类体右侧的表示结束的花括号后必须写一个分号，这是因为类体后面可以紧跟变量名以示对该类型对象的定义，所以分号必不可少：
```cpp
struct Sales_data {/* ... */} accum,trans, *salesptr;
//与上一条语句等价，但可能更好一些
struct Sales_data {/* ... */};
Sales_data accum, trans, *salesptr;
```

分号表示声明符（通常为空）的结束。一般来说，最好不要把对象的定义和类的定义放在一起。这么做无异于把两种不同实体的定义混在了一条语句里，一会儿定义类，一会儿又定义变量，显然这是一种不被建议的行为。

### 类数据成员
类体定义类的**成员**，我们的类只有**数据成员（data member）**。类的数据成员定义了类的对象的具体内容，每个对象有自己的一份数据成员拷贝。修改一个对象的数据成员，不会影响其他`Sales_data`的对象。

定义数据成员的方法和定义普通变量一样：首先说明一个基本类型，随后紧跟一个或多个声明符。

C++ 11新标准规定，可以为数据成员提供一个**类内初始值（in-class initializer）**。创建对象时，类初始值将用于初始化数据成员。没有初始值的成员将被默认初始化。

### 使用Sales_data类
和`Sales——item`类不同的是，我们自定义的`Sales_data`类没有提供任何操作，`Sales_data`类的使用这如果想执行什么操作就必须自己动手实现。

```cpp
double price = 0;
std::cin >> data1.bookNo >> data1.units_sold >> price;
data1.revenue = data1.units_sold * price;
```

## 编写自己的头文件
在函数体内定义类，但是这样的类毕竟受到了一些限制。所以，类一般都不定义在函数体内。当在函数体外部定义类时，在各个指定的源文件中可能只有一处该类的定义。而且，如果要在不同文件中使用同一个类，类的定义就必须保持一致。

为了确保各个文件中类的定义一致，类通常被定义在头文件中，而且类所在头文件的名字应与类的名字一样。

### 预处理器概述
确保头文件多次包含仍能安全工作的常用技术是**预处理器（preprocessor）**，它由C++言语从C语言继承而来。预处理器是在编译之前执行的一段程序，可以部分地改变我们所写的程序。

C++ 程序还会用到的一项预处理功能是**头文件保护符（header guard）**，头文件保护符依赖于预处理变量。  
预处理变量有两种状态：已定义和未定义。  
`#define`指令把一个名字设定为预处理变量，另外两个指令则分别检查某个指令的预处理变量是否已经定义：
> - `#ifdef`当且仅当变量以定义时为真
> - `#ifndef` 当且仅当变量未定义时为真

一旦检查结果为真，则执行后继续操作直到遇到`#endif`指令为止。
```cpp
#ifndef SALES_DATA_H
#define SALES_DATA_H
#include <string>
struct Sales_data {
    std::string boolNo;
    unsigned units_sold = 0;
    double revenue = 0.0;
};
#endif
```

整个程序中的预处理变量包括头文件保护符必须唯一，通常的做法是基于头文件中类的名字来构建保护符的名字，以确保其唯一性。为了避免与程序中的其他实体发生名字冲突，一般把预处理变量的名字全部大写。
