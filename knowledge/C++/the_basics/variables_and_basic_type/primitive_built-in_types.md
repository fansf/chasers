# 基本内置类型
**数据类型是程序的基础：它告诉我们数据的意义以及我们能够在数据上执行的操作**  
*Types are fundamental to any program: They tell us what our data mean and what operations we can perform on those data*

C++定义了一套基本数据类型，包括算术类型和空类型（void）。  
*C++ defines a set of primitive types that include the arithmetic types and a special type named void*

## 算术类型（arithmetic type）
算术类型可分为两类：**整型** 和 **浮点型**  
*The arithmetic types are divided into two categories: integral types (which include character and boolean types) and floating-point types*

### 整型
|类型|含义|最小尺寸（该类型数据所占的比特数）|
|---|---|---|
|`bool`|布尔类型|未定义|
|`char`|字符|8位|
|`short`|短整型|16位|
|`int`|整型|16位|
|`long`|长整形|32位|
|`long long`|长整形|64位|

### 浮点型

|类型|含义|最小尺寸（该类型数据所占的比特数）|  
|---|---|---|
|`float`|单精度浮点数|6位有效数字|  
|`double`|双精度浮点数|10位有效数字|

### 带符号类型和无符号类型
整型可以划分为**带符号的（singed）** 和 **无符号的（unsigned）** 两种。  
*the integral types may be signed or unsigned* 

带符号类型可以表示正数、负数或0，无符号类型则仅能表示大于0的值。  
*A signed type represents negative or positive numbers (including zero); an unsigned type represents only values greater than or equal to zero*

类型`int`，`short`，`long` 和`long long`都是带符号的，通过在这些类型名前添加`unsigned`就可以得到无符号类型，比如`unsigned long`。  
*The types int, short, long, and long long are all signed We obtain the corresponding unsigned type by adding unsigned to the type, such as unsigned long*

类型`unsigned int`可以被缩写成`unsigned`。  
*The type unsigned int may be abbreviated as unsigned*

与其他类型不同，字符型被分为了三种：`char`，`signed char` 和 `unsigned char`。  [为什么？](/knowledge/C++/补充内容/为什么char有三种类型.md)  
*Unlike the other integer types, there are three distinct basic character types: char,
signed char, and unsigned char*

[建议：如何选择类型](/knowledge/C++/建议/如何选择类型.md)

## 空类型（void）
空类型不对应具体的值，仅用于一些特殊的场合，最常见的就是作为函数的返回类型。  
*The void type has no associated values and can be used in only a few circumstances, most commonly as the return type for functions that do not return a value*

## 类型转换
对象类型定义了对象能够包含的数据和能参与的运算。  
*The type of an object defines the data that an object might contain and what operations that object can perform*

其中一种运算被大多数类型支持，就是将对象从一种给定的类型**转换(convert)** 为另一种类型。  
*Among the operations that many types support is the ability to convert objects of the given type to other, related types*

当在程序的某处我们使用了一种类型而其实对象应该取另一种类型时，程序会自动进行类型转换，我们将在[这儿](/knowledge/C++/the_basics/expressions/type_conversions.md)对类型转换做更详细的介绍。  
*Type conversions happen automatically when we use an object of one type where an object of another type is expected*

此处，有必要说明当给某种类型的对象强行赋了另一种类型的值时，到底会发生什么。  
*now it is useful to understand what happens when we assign a value of one type to an object of another type*

例如：
```cpp
bool b = 42;             //b为真
int i = b;               //i的值为1
i = 3.14;                //i的值为3
double pi = i;           //pi的值为3.0
unsigned char c = -1;    //假设char占8比特，c的值为255
signed char c2 = 256;    //假设char占8比特，c2的值是未定义的
```

类型所能表示的值的范围决定了转换的过程：  
*what happens depends on the range of the values that the types permit:*

> - 当我们把一个非布尔类型的算术值赋给布尔类型时，初始值为`0`则结果为`false`，否则为`true`。  
> *When we assign one of the nonbool arithmetic types to a bool object, the result is false if the value is 0 and true otherwise*  
> - 当我们把一个布尔值赋给整数类型时，初始值为`false`则结果为`0`，初始值为`true`则结果为`1`。  
>*When we assign a bool to one of the other arithmetic types, the resulting value is 1 if the bool is true and 0 if the bool is false*  
> - 当我们把一个浮点数赋给整数类型时，进行了近似处理。结果值将仅保留浮点数中小数点之前的部分。  
>*When we assign a floating-point value to an object of integral type, the value is truncated. The value that is stored is the part before the decimal point*  
> - 当我们把一个整数值赋给浮点类型时，小数部分记为`0`。如果给整数所占的空间超过了浮点类型的容量，精度可能有损失。  
>*When we assign an integral value to an object of floating-point type, the fractional part is zero. Precision may be lost if the integer has more bits than the floating-point object can accommodate*  
> - 当我们赋给无符号类型一个超出它表示范围的值时，结果是初始值对无符号类型表示数值总数取模后的余数。  
>*If we assign an out-of-range value to an object of unsigned type, the result is the remainder of the value modulo the number of values the target type can hold*  
> - 当我们赋给带符号类型一个超出它表示范围的值时，结果是**未定义的(undefined)**。此时，程序可能继续工作，可能崩溃，也可能生成垃圾数据。  
>*If we assign an out-of-range value to an object of signed type, the result is undefined. The program might appear to work, it might crash, or it might produce garbage values*  

### 含有无符号类型的表达式
尽管我们不会故意给无符号类型对象赋一个负值，却可能（特别容易）写出这么做的代码。  
*Although we are unlikely to intentionally assign a negative value to an object of unsigned type, we can (all too easily) write code that does so implicitly*

```cpp
unsigned u = 10;
int i = -42;
std::cout << i + i << std::endl;    //输出-48
std::cout << u + i << std::endl;    //如果int占32位，输出4294967264
```

当一个算数表达式中既有无符号数又有`int`值时，那个`int`值就会转换为无符号数。  
*if we use both unsigned and int values in an arithmetic expression, the int value ordinarily is converted to unsigned*

把负数转换为无符号数类似于直接给无符号数赋一个负值，结果等于这个负数加上无符号数的模。  
*Converting an int to unsigned executes the same way as if we assigned the int to an unsigned*

当从无符号数中减去一个值时，不管这个值是不是无符号数，我们都必须确保结果不可能是一个负数。  
*Regardless of whether one or both operands are unsigned, if we subtract a value from an unsigned, we must be sure that the result cannot be negative*

无符号数不小于`0`这一事实同样关系到循环的写法。例如我们打算输出`10`到`0`的递减序列。  
*The fact that an unsigned cannot be less than zero also affects how we write loops, For example, you were to write a loop that used the decrement operator to print the numbers from 10 down to 0*

```cpp
for (int i = 10;i >= 0;--i)
    std::cout << i << std::endl;
```

可能你觉得反正也不打算输出负数，可以用无符号数来重写这个循环。然而，这个不经意的改变却意味着死循环：  
*We might think we could rewrite this loop using an unsigned. After all, we don’t plan to print negative numbers. However, this simple change in type means that our loop will never terminate*

```cpp
//错误：变量u永远也不会小于0，循环条件一直成立
for (unsigned u = 10;u >= 0;--u)
    std::cout << u << std::endl;
```

当`u` 等于`-1`时会被转换成一个合理的无符号数。  
*That expression, --u, subtracts 1 from u. That result, -1, won’t fit in an unsigned value. As with any other out-of-range value, -1 will be transformed to an unsigned value*

我们可以使用`while`语句代替`for`语句，因为前者能够让我们在输出变量之前（而非之后）先减去`1`：  
*One way to write this loop is to use a while instead of a for. Using a while lets us decrement before (rather than after) printing our value*

```cpp
unsigned u = 11;
while (u > 0){
    --u;
    std::cout << u << std::endl;
}
```

## 字面值常量
一个形如`56`的值被称作**字面值常量(literal)**，这样的值一望而知。每个字面值常量都对应一种数据类型，字面值常量的形式和值决定了它的数据类型。  
*A value, such as 56, is known as a literal because its value self-evident. Every literal has a type. The form and value of a literal determine its type*

### 整型和浮点型字面值
我们可以将整型字面值写作十进制数、八进制数或十六进制数的形式。  以`0`开头的整数代表八进制，以`0x`或`0X`开头的代表十六进制数。  
*We can write an integer literal using decimal, octal, or hexadecimal notation. Integer literals that begin with 0 (zero) are interpreted as octal. Those that begin with either 0x or 0X are interpreted as hexadecimal*

例如，我们可以用下面任意一种形式来表示数值`20`：  
*For example, we can write the value 20 in any of the following three ways*

> - 20        // 十进制
> - 024      //八进制
> - 0x14    //十六进制

整型字面值具体的数据类型由它的值和符号决定。  
*The type of an integer literal depends on its value and notation*

默认情况下，十进制字面值是带符号数，八进制和十六进制字面值即可能是带符号的也可能是无符号的。  
*By default, decimal literals are signed whereas octal and hexadecimal literals can be either signed or unsigned types*

十进制字面值的类型是`int`，`long` 和 `long long`中尺寸最小的那个，前提是这种类型可能容纳下当前的值。  
*A decimal literal has the smallest type of int, long, or long long in which the literal’s value fits*

八进制和十六进制的字面值类型是能容纳其数值的`int`，`unsigned int`，`long`，`unsigned long`，`long long`和`unsigned long long`中尺寸最小者。  
*Octal and hexadecimal literals have the smallest type of int, unsigned int, long, unsigned long, long long, or unsigned long long in which the literal’s value fits*

`short`没有对应的字面值。  
*There are no literals of type short*

我们亦可以在字面值加后缀代表字面值类型。  
*we can override these defaults by using a suffix*

尽管整型字面值可以存储在带符号数据类型中，但严格来说，十进制字面值不会是负数。如果我们使用了一个形如`-42`的负十进制字面值，那个负号并不在字面值内，它的作用仅仅是对字面值取负值而已。  
*Although integer literals may be stored in signed types, technically speaking, the value of a decimal literal is never a negative number. If we write what appears to be a negative decimal literal, for example, -42, the minus sign is not part of the literal. The minus sign is an operator that negates the value of its (literal) operand*

浮点型字面值表现为一个小数或者以科学计数法表示的指数，其中指数部分用`E`或`e`标识。  
*Floating-point literals include either a decimal point or an exponent specified using scientific notation. Using scientific notation, the exponent is indicated by either E or e*

`3.14    1,34E0    0.    0e0    .001`

默认的，浮点类型字面值是一个`double`，我们同样可以使用后缀来表示其他浮点型。  
*By default, floating-point literals have type double. We can override the default using a suffix*

### 字符和字符串字面值
由单引号括起来的字符称为`char`型字面值，双引号括起来的零个或多个字符则构成字符串型字面值。  
*A character enclosed within single quotes is a literal of type char. Zero or more characters enclosed in double quotation marks is a string literal*

```cpp
'a'    //字符字面值
"Hello World!"    //字符串字面值
```

字符串字面值的类型实际上是由字符串常量构成的数组（array），该类型在[这儿 ](/knowledge/C++/the_basics/strings_vectors_arrays/arrays.md)做更多的介绍。编译器会在每个字符串的结尾处添加一个空字符（`'\0'`），因此，字符串字面值的实际长度要比它的内容多`1`。  
*The type of a string literal is array of constant chars, The compiler appends a null character (’\0’) to every string literal. Thus, the actual size of a string literal is one more than its apparent size*

如果两个字符串字面值紧邻且仅由空格、缩进和换行符分隔，则它们实际上是一个整体。  
*Two string literals that appear adjacent to one another and that are separated only by spaces, tabs, or newlines are concatenated into a single literal*

```cpp
// 分多行写的字符串字面值
std::cout << "a really, really long string literal "
             "that spans two lines " << std::endl;
```

### 转义序列
有两类字符我们不能直接使用：  
*Our programs cannot use any of these characters directly*  
> - 不可打印的字符：如退格或其他控制字符，因为它们没有可视的图符。  
>*backspace or control characters, have no visible image. Such characters are nonprintable*
> - 在C++中有特殊含义的字符（单引号、双引号、问号、反斜线）  
>*Other characters (single and double quotation marks, question mark, and backslash) have special meaning in the language*

如果要使用在C++这些字符，我们需要用到**转义序列(escape sequence)**，转义序列均以反斜线作为开始，C++语言规定的转义序列包括：  
*Instead, we use an escape sequence to represent such characters. An escape sequence begins with a backslash. The language defines several escape sequences*  
> - `\n` 换行符
> - `\t` 横向制表符
> - `\a` 报警符
> - `\v` 纵向制表符
> - `\b` 退格符
> - `\"` 双引号
> - `\\` 反斜线
> - `\?` 问号
> - `\'` 单引号
> - `\r` 回车符
> - `\f` 进纸符

### 指定字面值的类型
前面谈到，我们可以添加后缀来指定整型、浮点型和字符型的字面值的默认类型。  
*We can override the default type of an integer, floating- point, or character literal by supplying a suffix or prefix*

`42ULL` 无符号整型字面值，类型是`unsigned long long`

### 布尔字面值和指针字面值
`true`和`false`是布尔类型的字面值  
*The words true and false are literals of type bool*