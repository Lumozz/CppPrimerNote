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

### Note：

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
