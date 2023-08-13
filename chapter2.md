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

编写代码分别改变指针的值以及指针所指对象的值。

```c++
#include <iostream>
int main()
{
    int a = 1;
    int b = 2;

    int* a_p =&a;
    std::cout << a << std::endl;
    std::cout << *a_p << std::endl;

    a_p = &b;
    std::cout << *a_p << std::endl;
    *a_p = 3;
    std::cout << *a_p << std::endl;

    return 0;
}
```

## 2.19

说明指针和引用的主要区别

引用是另一个对象的别名，而指针本身就是一个对象。 引用必须初始化，并且一旦定义了引用就无法再绑定到其他对象。而指针无须在定义时赋初值，也可以重新赋值让其指向其他对象。

**引用不是对象**

## 2.20

请叙述下面这段代码的作用。

```c++
int i = 42;
int *p1 = &i; 
*p1 = *p1 * *p1;
```

计算了i的平方

## 2.21

请解释下述定义。在这些定义中有非法的吗？如果有，为什么？

```c++
int i = 0;
```

- (a) double* dp = &i; //非法，类型不匹配
- (b) int *ip = i; //非法，没有用取址符
- (c) int *p = &i; //合法

## 2.22

假设 p 是一个 int 型指针，请说明下述代码的含义。

```c++
if (p) // ...如果p不是空指针，则继续
if (*p) // ...如果p指向的内容不是空指针，则继续
```

## 2.23

给定指针 p，你能知道它是否指向了一个合法的对象吗？如果能，叙述判断的思路；如果不能，也请说明原因。

能，可以使用try catch的异常处理来分辨指针p是否指向一个合法的对象，但通过普通控制结构无法实现。？？

## 2.24

在下面这段代码中为什么 p 合法而 lp 非法？

```c++
int i = 42;
void *p = &i;
long *lp = &i;
```

**`void *`是从C语言那里继承过来的，可以指向任何类型的对象。 而其他指针类型必须要与所指对象严格匹配。**

## 2.25

说明下列变量的类型和值。

```c++
(a) int* ip, i, &r = i; //ip是int型指针，i是int型变量，r是i的引用
(b) int i, *ip = 0; //i是int型变量， ip是int空指针
(c) int* ip, ip2; //ip是int指针，ip2是整型
```

## 2.26

下面哪些语句是合法的？如果不合法，请说明为什么？

解：

```c++
const int buf;      // 不合法, const 对象必须初始化
int cnt = 0;        // 合法
const int sz = cnt; // 合法
++cnt; ++sz;        // 不合法, const 对象不能被改变
```

## 2.27

下面的哪些初始化是合法的？请说明原因。

解：

```c++
int i = -1, &r = 0;         // 不合法, r 必须引用一个对象
int *const p2 = &i2;        // 合法，常量指针
const int i = -1, &r = 0;   // 合法
const int *const p3 = &i2;  // 合法
const int *p1 = &i2;        // 合法
const int &const r2;        // 不合法, r2 是引用, 引用自带顶层 const, 第二个const写法多余但合法, 但引用需要初始化.
const int i2 = i, &r = i;   // 合法
```

## 2.28

说明下面的这些定义是什么意思，挑出其中不合法的。

解：

```c++
int i, *const cp;       // 不合法, const 指针必须初始化
int *p1, *const p2;     // 不合法, const 指针必须初始化
const int ic, &r = ic;  // 不合法, const int 必须初始化
const int *const p3;    // 不合法, const 指针必须初始化
const int *p;           // 合法. 一个指针，指向 const int
```

## 2.29

假设已有上一个练习中定义的那些变量，则下面的哪些语句是合法的？请说明原因。

解：

```c++
i = ic;     // 合法, 常量赋值给普通变量
p1 = p3;    // 不合法, p3 是const指针不能赋值给普通指针
p1 = &ic;   // 不合法, 普通指针不能指向常量
p3 = &ic;   // 不合法, p3 是常量指针且指向常量, 故p3 不能被修改, 本句赋值语句正在修改
p2 = p1;    // 不合法, p2是常量指针, 有顶层const, 不能被修改
ic = *p3;   // 不合法, 对 p3 取值后是一个 int 然后赋值给 ic, 但ic是常量不能被修改
```

## 2.30

对于下面的这些语句，请说明对象被声明成了顶层const还是底层const？

```c++
const int v2 = 0; int v1 = v2;// v2 top-level, v1 not const
int *p1 = &v1, &r1 = v1; // p1 not const, r1 not const
const int *p2 = &v2, *const p3 = &i, &r2 = v2; //p2 low-level, p3 low and top-level, r2 low
```

**const int 是类型符，对整行定义有效**

## 2.31

假设已有上一个练习中所做的那些声明，则下面的哪些语句是合法的？请说明顶层const和底层const在每个例子中有何体现。

解：

```c++
r1 = v2; 
p1 = p2; 
p2 = p1; 
p1 = p3; 
p2 = p3; 
```

## 2.32

下面的代码是否合法？如果非法，请设法将其修改正确。

```c++
int null = 0, *p = null;
```

非法，即使int的值恰好是0，也不能直接给指针赋值int变量。应改为

```c++
int null = 0, *p = &null;
```

而且应该注意到，null都是小写，并不是关键字或者预处理变量。

## 2.33

利用本节定义的变量，判断下列语句的运行结果。

解：

```c++
a=42; // a 是 int
b=42; // b 是一个 int,(ci的顶层const在拷贝时被忽略掉了)
c=42; // c 也是一个int
d=42; // d 是一个 int *,所以语句非法
e=42; // e 是一个 const int *, 所以语句非法
g=42; // g 是一个 const int 的引用，引用都是底层const，所以不能被赋值
```

## 2.34

基于上一个练习中的变量和语句编写一段程序，输出赋值前后变量的内容，你刚才的推断正确吗？如果不对，请反复研读本节的示例直到你明白错在何处为止。

## 2.35

判断下列定义推断出的类型是什么，然后编写程序进行验证。

```c++
const int i = 42;
auto j = i; const auto &k = i; auto *p = &i; 
const auto j2 = i, &k2 = i;
```

j: int

k: const int &

p: const int *

j2: const int 

k2: const int &

## 2.36

关于下面的代码，请指出每一个变量的类型以及程序结束时它们各自的值。

```c++
int a = 3, b = 4;
decltype(a) c = a;
decltype((b)) d = a;
++c;
++d;
```

## 2.37

## 2.38

