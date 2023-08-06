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

