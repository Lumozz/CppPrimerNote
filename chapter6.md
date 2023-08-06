## Chapter 6

## 6.3

常规写法：

```c++
#include <iostream>

int fact(int value) {

    int res = 1;

    while (value > 0) {
        res = res * value;
        value--;
    }

    return res;
}

int main() {
    int value = 5;
    std::cout << fact(value) << std::endl;
    return 0;
}
```

递归写法：

```c++
#include <iostream>

int fact(int value) {

    return value>0 ? value * fact(value-1) : 1;
}

int main() {
    int value = 5;
    std::cout << fact(value) << std::endl;
    return 0;
}
```



本答案没有考虑计算结果超出int存储长度的情况。

## 6.4

```c++
#include <iostream>
#include <string>

int fact(int i)
{
    return i > 1 ? i * fact(i - 1) : 1;
}

void interactive_fact()
{
    std::string const warning("Lenth out of range!");
    std::string const tips("please enter a number in [1,13)");

    // 这样写的好处是，输入std::cin >> value成功时，value会拿到输入
    // 失败时，for循环会自动退出
    for(int value=1; std::cout << tips, std::cin >> value;){ 
        if(value >= 13 || value <= 0){
            std::cout << warning;
            continue;
        } else {
            std::cout << fact(value) << std::endl;
        }
    }
}

int main()
{
    interactive_fact();
    return 0;
}
```

**Note**

```c++
#include<iostream>

int main() {
    for (int i=0; std::cout << i << std::endl, ++i; ++i){
        if (i >=10) break;
    }
}
```

执行结果为：

```c++
`0`
`2`
`4`
`6`
`8`
`10`
```

c++的for循环中，条件判断部分可以写表达式，并且表达式的结果对内存变量有影响。该例中，变量i先在条件判断中输出，然后在条件判断中自加1。两条表达式用逗号隔开。

## 6.5

```c++
#include <iostream>

int abs(int value){
    return value > 0 ? value : -value;
}

int main()
{
    int value = 0;
    std::cin >> value;
    std::cout << abs(value) << std::endl;
    return 0;
}
```

## 6.6

形参：在函数形参列表里面的为形参；

局部变量：在函数体中定义的变量为局部变量，函数执行结束后销毁；

局部静态变量：程序运行到创建该类对象时初始化，直到程序终止才被销毁，即使是定义在函数体中。

```c++
#include <iostream>

void count_add(int input){ //input为形参
	
    int local_var = input; //local_var位局部变量
    static int runtimes; //runtimes为静态局部变量

    std::cout << "local variable " << local_var << std::endl;
    std::cout << "static variable " << runtimes << std::endl;

    runtimes++;
}

int main()
{
    for(int i=0; i<3; i++) count_add(i);
    return 0;
}
```

```
local variable 0
static variable 0
local variable 1
static variable 1
local variable 2
static variable 2
```



## 6.7

```c++
#include <iostream>

int count_function(){

    static int runtimes(0);

   return runtimes++; //++在右侧，先返回，后运算，++在左侧，先运算，后返回。
}

int main()
{
    for(int i=0; i<3; i++) {
        std::cout << count_function() << std::endl;
    }
    return 0;
}
```

**Note** //++在右侧，先返回，后运算，++在左侧，先运算，后返回。

## 6.8

Chapter6.h中：

```c++
#ifndef CHAPTER6_CHAPTER6_H // 头文件保护符防止重复include
#define CHAPTER6_CHAPTER6_H

int fact(int);

#endif //CHAPTER6_CHAPTER6_H
```

main.cpp中：

```c++
#include <iostream>
#include "Chapter6.h"

int main() {
    int value = 5;
    std::cout << fact(value) << std::endl;
    return 0;
}

int fact(int value) {

    int res = 1;

    while (value > 0) {
        res = res * value;
        value--;
    }

    return res;
}
```

main.cpp中，fact函数定义在main函数后面，如果在main之前声明fact，将会报错。此时在Chapter6.h中声明fact函数，然后#include "Chapter6.h"就相当于在main函数之前生命了fact函数。

## 6.9

命令行操作g++，跳过

## 6.10

```c++
#include <iostream>

void swap(int *a, int *b){
    int mid = *a;
    *a = *b;
    *b = mid;
}

int main() {
    int a, b;
    if(std::cin >> a >> b) {
        swap(&a, &b);
        std::cout << a << b << std::endl;
    }
    return 0;
}
```

## 6.11

```c++
#include <iostream>

void reset(int &i){
    i = 0;
}

int main() {
    int i;
    if(std::cin >> i) {
        std::cout << i << std::endl;
        reset(i);
        std::cout << i << std::endl;
    }
    return 0;
}
```

# 6.12

```c++
#include <iostream>

void swap(int &a, int &b){
    int mid = a;
    a = b;
    b = mid;
}

int main() {
    int a, b;
    if(std::cin >> a >> b) {
        swap(a, b);
        std::cout << a << b << std::endl;
    }
    return 0;
}
```

c++中，最好使用传引用而不是传指针，因为指针需要在函数体中校验空指针。

## 6.13

```c++
void f(T) //传值

void f(&T) //传引用
```

## 6.14

**Note** ：

- 左值：C/C++语言中可以放在赋值符号左边的变量，即具有对应的可以由用户访问的[存储单元](https://baike.baidu.com/item/存储单元/8727749?fromModule=lemma_inlink)，并且能够由用户去改变其值的量。[左值](https://baike.baidu.com/item/左值/2327412?fromModule=lemma_inlink)表示存储在[计算机内存](https://baike.baidu.com/item/计算机内存/9021807?fromModule=lemma_inlink)的对象，而不是常量或计算的结果。或者说左值是代表一个内存地址值，并且通过这个内存地址，就可以对内存进行读并且写（主要是能写）操作；这也就是为什么左值可以被赋值的原因了。
- [右值](https://baike.baidu.com/item/右值/6187364?fromModule=lemma_inlink)：当一个符号或者常量放在[操作符](https://baike.baidu.com/item/操作符/8978896?fromModule=lemma_inlink)右边的时候，计算机就读取他们的“右值”，也就是其代表的真实值。
- 简单来说就是，左值相当于地址值，右值相当于数据值。右值指的是引用了一个存储在某个内存地址里的数据。 

```c++
#include <iostream>

void swap(const int &a, const int &b){
    int mid = a;
    a = b; //错误，常量整形不能修改
    b = mid;
}

int main() {
    int a, b;
    const int c=1;
    const int d=2;
    if(std::cin >> a >> b) {
        swap(1, 2);
        std::cout << a << b << std::endl;
    }
    return 0;
}
```

## 6.15

1. `s` 为`const string&` 类型，是为了避免拷贝，减少内存和时间消耗，且常量引用避免了意外改动；

2. `occurs`为`string::size_type&`类型，是为了像函数外传递结果；

3. `c`为普通传值，我们不能把const对象，字面值或需要类型转换的对象传递给普通的引用形参，但常量引用可以；

4. 如果`s`是普通引用，也可能会意外改变原来字符串的内容；

5. `occurs`如果是常量引用，那么意味着不能改变它的值；

## 6.16



