# cin的用法
cin是c++语言中的标准输入流对象，即`istream`。负责从标准输入中读取数据。

## cin的常用读取方法
常见的读取方法有：
- `cin >>`
- `cin.get`
- `cin.getline`

### cin >> 的用法
`cin >>` 可以根据变量类型从标准输入中读取数据，并将数据存入变量中。

这些数据以

- 空格(space)
- 制表符(tab)
- 换行符(enter)

作为分隔符。

**注意**：  
当调用`cin >>`方法时，`cin >>`会读取并舍弃流中最开始出现的连续的分隔符，直到遇见其它字符为止。类似于执行以下逻辑：
```cpp
while (space || tab || enter)
    read and discard;//读取并舍弃空格、制表符、回车符

while (!space && !tab && !enter)
    read;//读取，直到遇见空格、制表符或回车符

store into variable;
```
该操作成功读取数据后，数据后面的内容仍在标准输入流中，没有任何改动。

比如，对于
```cpp
#include <iostream>
#include <string>
using namespace std;

int main()
{
    int a;
    double b;
    string c;

    cin >> a >> b >> c;
    cout << a << ' ' << b << endl;
    cout << "begin" << c << "end" << endl;
    return 0;
}
```

这段程序来说，我们输入＂　`　　1　　2　　hello`＂后，变量`a`、`b`、`c`会被分别赋值为`1`,`2.33`,`hello`。

我们可以使用`getline`函数来验证`cin >>`的这个特性。`getline`是`<string>`中声明的函数，会将读取到的所有数据存入字符串。

仍是这段程序，不过我们使用`getline`读取字符串`c`
```cpp
#include <iostream>
#include <string>
using namespace std;

int main()
{
    int a;
    double b;
    string c;

    cin >> a >> b;
    getline(cin,c);
    
    cout << a << ' ' << b << endl;
    cout << "begin" << c << "end" << endl;
    return 0;
}
```

仍输入＂　`　　1　　2　　hello`＂，此时，变量`a`、`b`、`c`会被分别赋值为`1`,`2.33`和"　`　　hello`"。

### cin.get
有些时候我们需要读取一定长度的字符串（包含空格和制表符），我们可以使用`cin.get`。

常用的`cin.get`有三种形式：
- `istream& get(char& c);`
- `istream& get(char* s,streamsize n);`
- `istream& get(char* s,streamsize n,char delim);`

第一种形式常用于舍弃输入流中不需要的字符（比如`cin >>`读取数据成功后残留的分隔符）。  
第二种形式会读取`n - 1`个字符或者遇到`\n`，并存储为c类型字符串（以`\0`结尾的字符数组）。注意，该方法仍会将`\n`残留在标准输入中。  
第三种形式在第二种形式的基础上指定分隔符，该分隔符不会被读入c类型字符串，会残留在标准输入中。

### cin.getline
`cin.getline`也能够读取包含空格、制表符（换行符也可以）在内的一串字符。

`cin.getline`有两个形式：
- `istream& getline(char* s,streamsize n);`
- `istream& getline(char* s,streamsize n,char delim);`

`cin.getline`与`cin.get`唯一的区别是，`cin.getline`会将默认的换行符或指定的分隔符读取并舍弃掉，不会让它残留在缓冲区中。