# chapter2

## 2.1

类型 int、long、long long 和 short 的区别是什么？无符号类型和带符号类型的区别是什么？float 和 double的区别是什么？

解：

C++ 规定 short 和 int 至少16位，long 至少32位，long long 至少64位。 带符号类型能够表示正数、负数和 0 ，而无符号类型只能够表示 0 和正整数。浮点数的取值范围和精度不同，计算效率也有差异。

## 2.2

计算按揭贷款时，对于利率、本金和付款分别应选择何种数据类型？说明你的理由。

解：

使用`double`。需要进行浮点计算

## 2.3

读程序写结果。

```c++
unsigned u = 10, u2 = 42;
std::cout << u2 - u << std::endl; //32
std::cout << u - u2 << std::endl; //xx
int i = 10, i2 = 42;
std::cout << i2 - i << std::endl; //32
std::cout << i - i2 << std::endl; //-32
std::cout << i - u << std::endl; //0
std::cout << u - i << std::endl; //0
```

## 2.4

编写程序检查你的估计是否正确，如果不正确，请仔细研读本节直到弄明白问题所在。

## 2.5

指出下述字面值的数据类型并说明每一组内几种字面值的区别：

```c++
// char 宽字符16位 字符数组 宽字符数组字面值
(a) 'a', L'a', "a", L"a"
// int， unsigned， long unsigned， long， 八进制整形， 十六进制整形
(b) 10, 10u, 10L, 10uL, 012, 0xC
// double, float, long double
(c) 3.14, 3.14f, 3.14L
// int 十进制无符号， double, double
(d) 10, 10u, 10., 10e-2
```

## 2.6

下面两组定义是否有区别，如果有，请叙述之：

```c++
int month = 9, day = 7;
int month = 09, day = 07;
```

第一行是十进制整型

第二行是八进制整型，但是八进制不能写09

## 2.7

下述字面值表示何种含义？它们各自的数据类型是什么？

```c++
(a) "Who goes with F\145rgus?\012" //字符串数组，
(b) 3.14e1L //long double
(c) 1024f //无效，因为后缀f只能用于浮点字面量，而1024是整型
(d) 3.14L //long double
```

## 2.8

请利用转义序列编写一段程序，要求先输出 2M，然后转到新一行。修改程序使其先输出 2，然后输出制表符，再输出 M，最后转到新一行。

```c++
#include <iostream>
int main()
{
    std::cout << 2 << "\115\012";
    std::cout << 2 << "\t\115\012";
    return 0;
}
```

## 2.9

解释下列定义的含义，对于非法的定义，请说明错在何处并将其改正。

- (a) std::cin >> int input_value;
- (b) int i = { 3.14 };
- (c) double salary = wage = 9999.99;
- (d) int i = 3.14;

a 错误，应先定义后使用

b 错误，列表初始化严格检查类型对应情况

c wage使用之前未定义

d 正确，但是丢精度

## 2.10

下列变量的初值分别是什么？

```c++
std::string global_str;
int global_int;
int main()
{
    int local_int;
    std::string local_str;
}
```

**`global_str`和`global_int`是全局变量，所以初值分别为空字符串和0。 `local_int`是局部变量并且没有初始化，它的初值是未定义的。 `local_str` 是 `string` 类的对象，它的值由类确定，为空字符串。**

## 2.11

指出下面的语句是声明还是定义：

- (a) extern int ix = 1024; 定义
- (b) int iy; 定义
- (c) extern int iz; 声明

**Note** 声明与定义的区别，extern与是否显式地初始化。

## 2.12

请指出下面的名字中哪些是非法的？

- (a) int double = 3.14;
- (b) int _;
- (c) int catch-22;
- (d) int 1_or_2 = 1;
- (e) double Double = 3.14;

解：a, c, d, 非法

## 2.13

 下面程序中`j`的值是多少？

```c++
int i = 42;
int main()
{
    int i = 100;
    int j = i;
}
```

100

## 2.14

下面的程序合法吗？如果合法，它将输出什么？

```c++
int i = 100, sum = 0;
for (int i = 0; i != 10; ++i)
    sum += i;
std::cout << i << " " << sum << std::endl;
```

合法，输出100，45

## 2.15

下面的哪个定义是不合法的？为什么？

- (a) int ival = 1.01; 合法，但是丢精度
- (b) int &rval1 = 1.01;  非法，初值不能为字面值
- (c) int &rval2 = ival; 合法
- (d) int &rval3; 非法，引用必须初始化

## 2.16

考察下面的所有赋值然后回答：哪些赋值是不合法的？为什么？哪些赋值是合法的？它们执行了哪些操作？

```c++
int i = 0, &r1 = i; 
double d = 0, &r2 = d;
```

- (a) r2 = 3.14159; 合法
- (b) r2 = r1; 合法，但是数据类型会转换
- (c) i = r2;  合法，但是double转int会丢精度
- (d) r1 = d; 合法，但是double转int会丢精度

## 2.17

执行下面的代码段将输出什么结果？

```c++
int i, &ri = i;
i = 5; ri = 10;
std::cout << i << " " << ri << std::endl;
```

10 10

## 2.18

