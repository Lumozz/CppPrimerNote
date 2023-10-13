# chapter 8

## 8.1

编写函数，接受一个`istream&`参数，返回值类型也是`istream&`。此函数须从给定流中读取数据，直至遇到文件结束标识时停止。它将读取的数据打印在标准输出上。完成这些操作后，在返回流之前，对流进行复位，使其处于有效状态。

```c++
std::istream& func(std::istream &is)
{
    int buf;
    while(is >> buf){
        std::cout << buf << std::endl;
    }
    is.clear();
    return is;
}
```

## 8.2

测试函数，调用参数为cin

```c++
#include<iostream>

std::istream& func(std::istream &is)
{
    int buf;
    while(is >> buf){
        std::cout << buf << std::endl;
    }
    is.clear();
    while(is >> buf){
        std::cout << buf << std::endl;
    }

    return is;
}

int main(){
    std::istream& is = func(std::cin);
    std::cout <<"status: " << is.rdstate() << std::endl;
}
```

## 8.3

什么情况下，下面的`while`循环会终止？

如`badbit`、`failbit`、`eofbit` 的任一个被置位，那么检测流状态的条件会失败。

```
`i: 123`
`o: 123`
`i: number`
`o: status: 4`
```

输入数字int时，程序正常运行，输入string是，is流异常。

然后调用`is.clear()`来清除输入流的错误标志，包括EOF标志。这使得输入流重新变得可用，可以再次从中读取数据。

但是，当试图再次从`std::cin`读取数据，此时已经发送了EOF信号，所以输入流已经没有更多数据可以读取了。因此，第二个`while(is >> buf)`循环将立即结束，因为它无法从`std::cin`读取任何数据。

对于`std::cin`，一旦发送EOF，你就无法再从中读取更多数据，除非你再次提供更多输入。

如果你想测试`is.clear()`的效果，可以尝试从文件而不是`std::cin`读取数据

通常，is.rdstate() 返回的状态值位的含义如下所示，但是不同系统或编译器中可能有所不同。

```
std::ios_base::goodbit: 0000
std::ios_base::eofbit: 0100
std::ios_base::failbit: 0010
std::ios_base::badbit: 0001
```

## 8.4

编写函数，以读模式打开一个文件，将其内容读入到一个`string`的`vector`中，将每一行作为一个独立的元素存于`vector`中。



`read`函数使用`while(is >> line)`读取文件。这会读取并尝试分割空格分隔的字符串，而不是读取整行。如果你希望按行读取文件，应该使用`std::getline()`函数，如`while(std::getline(is, line))`。

```c++
#include<iostream>
#include<fstream>
#include<vector>
#include<string>

std::ifstream& read(std::ifstream& is, std::vector<std::string>& buf){

    std::string line;
    while(std::getline(is, line)) {
        buf.push_back(line);
    }

    return is;
}

int main(){
    std::vector<std::string> buf;
    std::ifstream file_input("./file");


    if (!file_input.is_open()) {
        std::cerr << "Failed to open file  " << file_input.rdstate() << std::endl;
        return 1;
    }

    std::ifstream& is = read(file_input, buf);
    file_input.close();

    std::cout << buf.size() << std::endl;
    std::cout << "file content" << std::endl;
    for(std::string s: buf){
        std::cout << s << std::endl;
    }

}
```

## 8.5

重写上面的程序，将每个单词作为一个独立的元素进行存储。 解：

```c++
#include<iostream>
#include<fstream>
#include<vector>
#include<string>

std::ifstream& read(std::ifstream& is, std::vector<std::string>& buf){

    std::string line;
    while(i) {
        buf.push_back(line);
    }

    return is;
}

int main(){
    std::vector<std::string> buf;
    std::ifstream file_input("./file");


    if (!file_input.is_open()) {
        std::cerr << "Failed to open file  " << file_input.rdstate() << std::endl;
        return 1;
    }

    std::ifstream& is = read(file_input, buf);
    file_input.close();

    std::cout << buf.size() << std::endl;
    std::cout << "file content" << std::endl;
    for(std::string s: buf){
        std::cout << s << std::endl;
    }

}
```

## 8.6

重写7.1.1节的书店程序，从一个文件中读取交易记录。将文件名作为一个参数传递给`main`

```c++
// main.cpp
#include <iostream>
#include <fstream>
#include "main.h"

int main(int argc, char **argv){
    std::ifstream input(argv[1]);
    Sales_data total;

    if (read(input, total)){
        Sales_data trans;
        while(read(input, trans)){
            if (total.isbn() == trans.isbn())
                total.combine(trans);
            else{
                print(std::cout, total) << std::endl;
                total = trans;
            }
        }
        print(std::cout, total) << std::endl;
    }
    else {
        std::cerr << "No data" << std::endl;
    }
}
```

```c++
// main.h

#ifndef CHAPTER8_CHAPTER8_H
#define CHAPTER8_CHAPTER8_H

#include <string>

class Sales_data {
public:
    // 构造函数
    Sales_data() = default;
    Sales_data(const std::string &s): bookNo(s) { }
    Sales_data(const std::string &s, unsigned n, double p):
            bookNo(s), units_sold(n), revenue(n*p) { }

    // 成员函数
    double avg_price() const {
        if (units_sold)
            return revenue/units_sold;
        else
            return 0;
    }
    std::string isbn() const { return bookNo; }
    Sales_data& combine(const Sales_data&);

    // 数据成员
    std::string bookNo;
    unsigned units_sold = 0;
    double revenue = 0.0;
};

// Sales_data的非成员函数
Sales_data add(const Sales_data &lhs, const Sales_data &rhs) {
    Sales_data sum = lhs;  // 把lhs的内容拷贝到新的对象中
    sum.combine(rhs);   // 把rhs的内容加到sum中
    return sum;
}

// combine的实现
Sales_data& Sales_data::combine(const Sales_data &rhs) {
    units_sold += rhs.units_sold;  // 加上rhs的数量
    revenue += rhs.revenue;  // 加上rhs的总收入
    return *this;  // 返回调用者
}

// read函数将数据从istream读入到Sales_data对象中
std::istream& read(std::istream &is, Sales_data &item) {
    double price = 0;
    is >> item.bookNo >> item.units_sold >> price;
    item.revenue = price * item.units_sold;
    return is;
}

// print函数将Sales_data对象的信息输出到ostream
std::ostream& print(std::ostream &os, const Sales_data &item) {
    os << "item.isbn(): " << item.isbn() << std::endl;
    os << "item.units_sold: " << item.units_sold << std::endl;
    os << "item.revenue: " << item.revenue << std::endl;
    os << "item.avg_price(): " << item.avg_price() << std::endl;

    return os;
}
#endif //CHAPTER6_CHAPTER8_H
```