# C2.2 变量
变量提供一个具名的、可供程序操作的存储空间。  
*A variable provides us with named storage that our programs can manipulate*

C++ 中每个变量都有其数据类型，数据类型决定着变量所占用内存空间的大小和布局方式、该空间能存储的值的范围，以及变量能够参与的运算。  
*Each variable in C++ has a type. The type determines the size and layout of the variable’s memory, the range of values that can be stored within that memory, and the set of operations that can be applied to the variable*

对C++ 程序员来说，”变量（variable）“ 和 ”对象（object）”一般可以相互使用。  
*C++ programmers tend to refer to variables as “variables” or “objects” interchangeably*

## 变量定义
变量定义的基本形式是：首先是**类型说明符（type specifier）**，随后紧跟由一个或多个变量名组成的列表，其中变量名一逗号分隔，最后以分号结束。  
*A simple variable definition consists of a type specifier, followed by a list of one or more variable names separated by commas, and ends with a semicolon*

列表中的每个变量名的类型由类型说明符指定，定义时还可以为一个或多个变量赋初值：  
*Each name in the list has the type defined by the type specifier. A definition may (optionally) provide an initial value for one or more of the names it defines*

```cpp
int sum = 0,value,
    units_sold = 0;    //sum,value和units_sold都是int
std::string str;            //str的类型是string
std::string book("1984");   //book 通过一个string字面值初始化
```

### 初始值
当对象在创建时获得了一个特定的值，我们说这个对象被**初始化（initialized）**了。  
*An object that is initialized gets the specified value at the moment it is created*

用于初始化变量的值可以是任意复杂的表达式。  
*The values used to initialize a variable can be arbitrarily complicated expressions*

当一次定义了两个或多个变量时，对象的名字随着定义也就马上可以使用了，因此在同一条定义语句中，可以用先定义的变量值去初始化后定义的其他变量。  
*When a definition defines two or more variables, the name of each object becomes visible immediately. Thus, it is possible to initialize a variable to the value of one defined earlier in the same definition*

```cpp
double price = 109.99,discount = price * 0.16;
double salePrice = applyDiscount(price,discount);
```

在C++ 语言中，初始化和赋值是两个完全不同的操作，但它们的区别非常小，我们特别容易将二者混为一谈。  
> 记住：初始化不是赋值，初始化的含义是创建变量时赋予其一个初始值，而赋值的含义是把对象的当前值擦除，而以一个新值来替代。  
>*Initialization is not assignment. Initialization happens when a variable is given a value when it is created. Assignment obliterates an object’s current value and replaces that value with a new one*

### 列表初始化
C++ 语言提供了定义初始化的好几种不同形式，这也是初始化问题复杂性的一个体现。  
*One way in which initialization is a complicated topic is that the language defines several different forms of initialization*

```cpp
int num = 0;
int num = {0};
int num(0);
int num{0};
```

在C++ 11中，用花括号来初始化变量得到了全面应用。这种初始化的形式被称为**列表初始化（list initialization）**。  
*The generalized use of curly braces for initialization was introduced as part of the new standard. this form of initialization is referred to as list initialization*

当作用于内置类型的变量时，这种初始化形式有一个重要的特点：如果我们使用列表初始化且初始值存在丢失信息的风险，则编译器将报错：  
*When used with variables of built-in type, this form of initialization has one important property: The compiler will not let us list initialize variables of built-in type if the initializer might lead to the loss of information*

```cpp
long double ld = 3.1415926536;
int a{ld},b = {ld};            //错误：转换未执行，因为存在丢失信息的危险
int c(ld),d = ld;              //正确：转换执行，且确实丢失了部分值
```

### 默认初始化
如果定义变量时没有指定初值，则变量被**默认初始化（default initialization）**，此时变量被赋予了“默认值”。   
*When we define a variable without an initializer, the variable is default initialized. Such variables are given the “default” value*

默认值到底是什么由变量类型决定，同时定义变量的位置也会对此有影响。  
*What that default value is depends on the type of the variable and may also depend on where the variable is defined*

如果是内置类型的变量未被初始化，它的值有定义的位置决定。  
*The value of an object of built-in type that is not explicitly initialized depends on where it is defined*

定义于任何函数体之外的变量被初始化为`0`。  
*Variables defined outside any function body are initialized to zero*

除了一种例外，定义在函数体内部的内置变量将**不被初始化（uninitialized）**。  
*With one exception, variables of built-in type defined inside a function are uninitialized*

一个未被初始化的内置类型变量的值是未定义的，如果试图拷贝或以其他形式访问此类值将引发错误。  
*The value of an uninitialized variable of built-in type is undefined (§ 2.1.2, p. 36). It is an error to copy or otherwise try to access the value of a variable whose value is undefined*

每个类各自决定其初始化对象的方式。而且，是否允许不经过初始化就定义对象也由类自己决定。  
*Each class controls how we initialize objects of that class type. In particular, it is up to the class whether we can define objects of that type without an initializer*

绝大多数类都支持无需显式初始化而定义对象，这样的类提供了一个合适的默认值。  
*Most classes let us define objects without explicit initializers. Such classes supply an appropriate default value for us*

一些类要求每个对象都显式初始化，此时如果创建了一个该类的对象而未对其做明确的初始化操作，将引发错误。  
*Some classes require that every object be explicitly initialized. The compiler will complain if we try to create an object of such a class with no initializer*

## 变量声明与定义的关系
为了允许把程序拆分成多个逻辑部分来编写，C++ 语言支持**分离式编译（separate compilation）**机制，该机制允许将程序分割为若干个文件，每个文件可被独立编译。  
*To allow programs to be written in logical parts, C++ supports what is commonly known as separate compilation. Separate compilation lets us split our programs into several files, each of which can be compiled independently* 

如果程序被分为多个文件，则需要有在文件间共享代码的方法。一个实际例子是`std::cout`和`std::cin`，它们定义于标准库，却能被我们写的程序使用。  
*When we separate a program into multiple files, we need a way to share code across those files. As a concrete example, consider std::cout and std::cin. These are objects defined somewhere in the standard library, yet our programs can use these objects*

为了支持分离式编译，C++ 语言将声明和定义区分开来。  
*To support separate compilation, C++ distinguishes between declarations and definitions*

**声明（declaration）**使得名字为程序所知，一个文件如果想使用别处定义的名字则必须包含对那个名字的声明。  
*A declaration makes a name known to the program. A file that wants to use a name defined elsewhere includes a declaration for that name*

**定义（definition）**负责创建与名字关联的实体。  
*A definition creates the associated entity*

变量声明规定了变量的类型和名字，在这一点上与定义相同。  
*A variable declaration specifies the type and name of a variable. A variable definition is a declaration*

定义既声明了变量的类型和名字，又为变量申请了存储空间，也有可能为变量赋一个初始值。  
*In addition to specifying the name and type, a definition also allocates storage and may provide the variable with an initial value*

如果想声明一个变量而非定义它，就在变量名前添加关键字`extern`，而且不要显式地初始化变量。  
*To obtain a declaration that is not also a definition, we add the extern keyword and may not provide an explicit initializer*

```cpp
extern int i;    //声明i而非定义i
int j;           //声明并定义j
```

任何包含了显式初始化的声明即成为定义。  
*Any declaration that includes an explicit initializer is a definition*

在函数体内部，如果试图初始化一个由`extern`关键字标记的变量将会引发错误。  
*It is an error to provide an initializer on an extern inside a function*

> 变量只能被定义一次，但是可以被多次声明。  
> *Variables must be defined exactly once but can be declared many times.*

## 标识符
C++ 标识符（identifier）由字母、数字和下划线组成，其中必须以字母或下划线开头。标识符的长度没有限制，但对大小写敏感：  
*Identifiers in C++ can be composed of letters, digits, and the underscore character. The language imposes no limit on name length. Identifiers must begin with either a letter or an underscore. Identifiers are case-sensitive; upper- and lowercase letters are distinct*

```cpp
//几种不同的int变量
int somename,someName,SomeName,SOMENAME
```

C++ 语言保留了一些名字供语言本身使用，这些名字不能被用作标识符。  
*The language reserves a set of names*

**C++关键字**  

|1|2|3|4|5|
|---|---|---|---|---|
|`alignas`|`continue`|`friend`|`register`|`true`|
|`alignof`|`decltype`|`goto`|`reinterpret_case`|`try`|
|`asm`|`default`|`if`|`return`|`typedef`|
|`auto`|`delete`|`inline`|`short`|`typeid`|
|`bool`|`do`|`int`|`signed`|`typename`|
|`break`|`double`|`long`|`sizeof`|`union`|
|`case`|`dynamic_case`|`mutable`|`static`|`unsigned`|
|`catch`|`else`|`namespace`|`static_assert`|`using`|
|`char`|`enum`|`new`|`static_case`|`virtual`|
|`char16_t`|`explicit`|`noexcept`|`struct`|`void`|
|`char32_t`|`export`|`nullptr`|`switch`|`volatile`|
|`class`|`extern`|`operator`|`template`|`wchar_t`|
|`const`|`false`|`private`|`this`|`while`|
|`constexpr`|`float`|`protected`|`thread_local`|`const_cast`|
|`for`|`public`|`throw`|

**C++操作符替代名**  

|1|2|3|4|5|6|
|-|-|-|-|-|-|
|`and`| `bitand`|`compl`|`not_eq`|`or_eq`|`xor_eq`|
|`and_eq`|`bitor`|`not`|`or`|`xor`||

### 变量命名规范
变量命名许多约定俗成的规范，下面的这些规范能有效提高程序的可读性：  
*There are a number of generally accepted conventions for naming variables. Following these conventions can improve the readability of a program*  

- 标识符要体现实际含义  
*An identifier should give some indication of its meaning*  
- 变量名一般用小写字母，如`index`，不要使用`Index`或`INDEX`。  
*Variable names normally are lowercase—index, not Index or INDEX.*  
- 用户自定义的类名一般以大写字母开头，如`Sales_item`。  
*Like Sales_item, classes we define usually begin with an uppercase letter*  
- 如果标识符由多个单词组成，则单词间应有明显区分，如`student_loan`或`studenLoan`，不要使用`studentoan`。  
*Identifiers with multiple words should visually distinguish each word, for
example, student_loan or studentLoan, not studentloan*

## 名字的作用域
无论是在程序的什么位置，使用到的每一个名字都会指向一个特定的实体：变量、函数、类型等。  
*At any particular point in a program, each name that is in use refers to a specific entity—a variable, function, type, and so on*

然而，同一个名字如果出现在程序的不同位置，也可能指向的是不同的实体。  
*However, a given name can be reused to refer to different entities at different points in the program*

**作用域（scope）**是程序的一部分，在其中名字有其特定的含义。  
*A scope is a part of the program in which a name has a particular meaning*

C++ 语言中大多数作用域都以花括号分隔。  
*Most scopes in C++ are delimited by curly braces.*

同一名字在不同的作用域中可能指向不同的实体。  
*The same name can refer to different entities in different scopes*

名字的有效区间始于名字的声明语句，以声明语句所在的作用域末端为结束。  
*Names are visible from the point where they are declared until the end of the scope in which the declaration appears*

### 嵌套的作用域
作用域能彼此包含，被包含（或者说被嵌套）的作用域称为**内层作用域（inner scope）**，包含着别的作用域的作用域被称为**外层作用域（outer scope）**  
*Scopes can contain other scopes. The contained (or nested) scope is referred to as an inner scope, the containing scope is the outer scope.*

作用域中一旦声明了某个名字，它所嵌套着的所有作用域中都能访问该名字。  
*Once a name has been declared in a scope, that name can be used by scopes nested inside that scope. *

同时，允许在内存作用域中重新定义外层作用域已有的名字。  
*Names declared in the outer scope can also be redefined in an inner scope*

> 如果函数有可能使用到某全局变量，则不宜再定义一个同名的局部变量。  
> *It is almost always a bad idea to define a local variable with the same name
as a global variable that the function uses or might use*
