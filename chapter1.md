# chapter 1

## 1.1

查阅你使用的编译器的文档，确定它所使用的文件名约定。编译并运行第2页的main程序。

- `g++ --std=c++11 ch1.cpp -o main`
- `./main`

## 1.2

改写程序，让它返回-1。返回值-1通常被当做程序错误的标识。重新编译并运行你的程序，观察你的系统如何处理main返回的错误标识。

解：

在ubuntu下，使用g++，返回-1，./main没有发现任何异常。

## 1.3

编写程序，在标准输出上打印Hello, World。

解：

```c++
#include <iostream>

int main()
{
	std::cout << "Hello, World" << std::endl;
	return 0;
}
```

## 1.4

我们的程序使用加法运算符`+`来将两个数相加。编写程序使用乘法运算符`*`，来打印两个数的积。

解：

```c++
#include <iostream>

int main()
{
    std::cout << "Enter two numbers:" << std::endl;
    int v1 = 0, v2 = 0;
    std::cin >> v1 >> v2;
    std::cout << "The product of " << v1 << " and " << v2
              << " is " << v1 * v2 << std::endl;
}   
```

## 1.5

我们将所有的输出操作放在一条很长的语句中，重写程序，将每个运算对象的打印操作放在一条独立的语句中。

解：

```c++
#include <iostream>

int main()
{
    std::cout << "Enter two numbers:" << std::endl;
        int v1 = 0, v2 = 0;
        std::cin >> v1 >> v2;
        std::cout << "The product of ";
        std::cout << v1;
        std::cout << " and ";
        std::cout << v2;
        std::cout << " is ";
        std::cout << v1 * v2;
        std::cout << std::endl;
}    
```

## 1.6


解释下面程序片段是否合法。

```c++
std::cout << "The sum of " << v1;
          << " and " << v2;
          << " is " << v1 + v2 << std::endl;
```



如果程序是合法的，它的输出是什么？如果程序不合法，原因何在？应该如何修正？

非法，第二行的语句输出符号没有传递到`std::cout`对象上，因为上一行的分号已经意味着上一条语句完结了。

## 1.7

```c++
#include <iostream>

int main(){
    /* this is cpp
     /*
     */
     */
    std::cout << 2 << std::endl;

    return 0;
}
```

输出：

```c++
/home/lumos/learnshs/c++/chapter6/test2.cpp:9:7: error: expected primary-expression before ‘/’ token
    9 |      */
      |       ^
ninja: build stopped: subcommand failed.
```

编译器报错；

## 1.8

指出下列哪些输出语句是合法的（如果有的话）：

```c++
std::cout << "/*";
std::cout << "*/";
std::cout << /* "*/" */;
std::cout << /* "*/" /* "/*" */;
```

预测编译这些语句会产生什么样的结果，实际编译这些语句来验证你的答案(编写一个小程序，每次将上述一条语句作为其主体)，改正每个编译错误。

第三条是非法的；应该为

```c++
std::cout << /* "*/" */";
```

## 1.9

编写程序，使用`while`循环将50到100整数相加。

```c++
#include <iostream>

int main(){
    int i = 50;
    int sum = 0;

    while(i<=100){
        sum += i;
        ++i;
    }
    std::cout << sum << std::endl;
    return 0;
}
```

## 1.10

除了`++`运算符将运算对象的值增加1之外，还有一个递减运算符`--`实现将值减少1.编写程序与，使用递减运算符在循环中按递减顺序打印出10到0之间的整数。

```c++
#include <iostream>

int main()
{
    int val = 10;
    while (val >= 0){
        std::cout << val << " ";
        --val;
    }
    std::cout << std::endl;
}  
```

## 1.11

编写程序，提示用户输入两个整数，打印出这两个整数所指定的范围内的所有整数。

解：

```c++
#include <iostream>

int main()
{
    int start = 0, end = 0;
    std::cout << "Please input two num: ";
    std::cin >> start >> end;
    if (start <= end) {
        while (start <= end){
            std::cout << start << " ";
            ++start;
        }
        std::cout << std::endl;
    }
    else{
        std::cout << "start should be smaller than end !!!";
    }
}  
```

## 1.12

下面的for循环完成了什么功能？sum的终值是多少？

```c++
int sum = 0;
for (int i = -100; i <= 100; ++i)
	sum += i;
```

实现了-100到100的相加

## 1.13

使用for循环重做1.4.1节中的所有练习（练习1.9到1.11）。

解：

1.9

```c++
#include <iostream>

int main()
{
    int sum = 0;
    for (int val = 50; val <= 100; ++val){
        sum += val;
    }
    std::cout << "Sum of 50 to 100 inclusive is "
              << sum << std::endl;
}    
```

1.10

```c++
#include <iostream>

int main()
{
    for (int val = 10; val >=0; --val){
        std::cout << val << " ";
    }
    std::cout << std::endl;
}  
```

1.11

```c++
#include <iostream>

int main()
{
    int start = 0, end = 0;
    std::cout << "Please input two num: ";
    std::cin >> start >> end;
    if (start <= end) {
        for (; start <= end; ++start){
            std::cout << start << " ";
        }
        std::cout << std::endl;
    }
    else{
        std::cout << "start should be smaller than end !!!";
    }
}  
```

## 1.14 

## 1.15

编写程序，包含第14页“再探编译”中讨论的常见错误。熟悉编译器生成的错误信息。

解：

编译器可以检查出的错误有：

- 语法错误
- 类型错误
- 声明错误

## 1.16

编写程序，从cin读取一组数，输出其和。

```c++
#include <iostream>

int main()
{
    int sum = 0;
    int i = 0;
    while(std::cin >> i){
        sum += i;
        i = 0;
    }

    std::cout << sum;
} 
```

## 1.17

如果输入的所有值都是相等的，本节的程序会输出什么？如果没有重复值，输出又会是怎样的？

## 1.18

编译并运行本节的程序，给它输入全都相等的值。再次运行程序，输入没有重复的值。

解：

全部重复：

```
1 1 1 1 1 
1 occurs 5 times 
```



没有重复：

```c++
1 2 3 4 5
1 occurs 1 times 
2 occurs 1 times 
3 occurs 1 times 
4 occurs 1 times 
5 occurs 1 times 
```

## 1.19

修改你为1.4.1节练习1.11（第11页）所编写的程序（打印一个范围内的数），使其能处理用户输入的第一个数比第二个数小的情况。

解：

```c++
#include <iostream>

int main()
{
    int start = 0, end = 0;
    std::cout << "Please input two num: ";
    std::cin >> start >> end;
    if (start <= end) {
        while (start <= end){
            std::cout << start << " ";
            ++start;
        }
        std::cout << std::endl;
    }
    else{
        std::cout << "start should be smaller than end !!!";
    }
}  
```

## 1.20

在网站http://www.informit.com/title/032174113 上，第1章的代码目录包含了头文件 Sales_item.h。将它拷贝到你自己的工作目录中。用它编写一个程序，读取一组书籍销售记录，将每条记录打印到标准输出上。

解：

```c++
#include <iostream>
#include "Sales_item.h"

int main()
{
	for (Sales_item item; std::cin >> item; std::cout << item << std::endl);
	return 0;
}
```

## 1.21

编写程序，读取两个 ISBN 相同的 Sales_item 对象，输出他们的和。

解：

```c++
#include <iostream>
#include "Sales_item.h"

int main()
{
    Sales_item item_1;
    Sales_item item_2;
    std::cin >> item_1;
    std::cout << item_1 << std::endl;
    std::cin >> item_2;
    std::cout << item_2 << std::endl;
    std::cout << "sum of sale items: " << item_1 + item_2 << std::endl;
	return 0;
}
```

## 1.22

编写程序，读取多个具有相同 ISBN 的销售记录，输出所有记录的和。

解：

```c++
#include <iostream>
#include "Sales_item.h"

int main()
{
    Sales_item sum_item;
    std::cin >> sum_item;
    std::cout << sum_item << std::endl;
    for (Sales_item item; std::cin >> item; std::cout << item << std::endl){
        sum_item += item;
    }
    std::cout << "sum of sale items: " << sum_item << std::endl;
	return 0;
}
```

## 1.23 1.24

输入表示多个 ISBN 的多条销售记录来测试上一个程序，每个 ISBN 的记录应该聚在一起。

解：

```c++
#include <iostream>
#include "Sales_item.h"

int main()
{
    Sales_item total;
    if (std::cin >> total){
        Sales_item trans;
        while (std::cin >> trans){
            if (total.isbn() == trans.isbn()) {
                total += trans;
            }
            else {
                std::cout << total << std::endl;
                total = trans;
            }
        }
        std::cout << total << std::endl;
    }
    else {
        std::cerr << "No data?!" << std::endl;
        return -1;
    }
    return 0;
}
```