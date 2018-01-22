# cout
cout一般语句格式为：
```cpp
cout << var1 << var2 << var3;
```

## 格式控制
必须包含 `iomanip`和`iostream`头文件。

### setprecision(n)
**控制输出流显示浮点数的数字个数**，会有四舍五入发生。

```cpp
cout << setprecision(4) << 50.499 << endl;
cout << setprecision(4) << 50.49 << endl;
```

输出：
```cpp
50.5
50.49
```

小数末尾的0将被省略

cout 的默认数字个数是6（setprecision(6)）

### showpoint
如果想要完整显示`setprecision`中设置的字符数，可以使用`showpoint`
```cpp
cout << showpoint << setprecision(4) << 50.499 << endl;
cout << showpoint << setprecision(8) << 50.12309 << endl;
```

输出：
```cpp
50.50
50.123090
```

### fixed
如果想保留几位小数，使用`fixed`和`setprecision`
```cpp
cout << fixed << setprecision(4) << 50.499 << endl;
cout << fixed << setprecision(4) << 50.12309 << endl;
```

输出：
```cpp
50.4990
50.1231
```

### setw
指定显示的区域宽度

```cpp
cout << setw(12) << setprecision(4) << 50.499 << endl;
```

输出：
```cpp
        50.5
```

### right / left
右对齐，左对齐

```cpp
cout << left << setw(12) << setprecision(4) << 50.499 << endl;
```

输出：
```cpp
50.5
```