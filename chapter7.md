# chapter7

## 7.1

```c++
#include<iostream>
#include<string>

class Sales_data{
public:
    std::string bookNo;
    unsigned units_sold = 0;
    double revenue = 0.0;
};

int main(){
    Sales_data total;
    if(std::cin >> total.bookNo >> total.units_sold >> total.revenue){
        Sales_data trans;
        while(std::cin >> trans.bookNo >> trans.units_sold >> trans.revenue){
            if (total.bookNo == trans.bookNo) {
                total.units_sold += trans.units_sold;
                total.revenue += trans.revenue;
            } else{
                std::cout << total.bookNo << " " << total.units_sold << " " << total.revenue << std::endl;
                total = trans;
            }
        }
        std::cout << total.bookNo << " " << total.units_sold << " " << total.revenue << std::endl;
    }
    else{
        std::cerr << "No data?" << std::endl;
        return -1;
    }
    return 0;
}
```

## 7.2

曾在2.6.2节的练习中编写了一个`Sales_data`类，请向这个类添加`combine`函数和`isbn`成员。

```c++
class Sales_data{
public:
    std::string bookNo;
    unsigned units_sold = 0;
    double revenue = 0.0;

    std::string isbn() const{
        return bookNo;
    }
    Sales_data &combine(const Sales_data&);
};

Sales_data& Sales_data::combine(const Sales_data &rhs) {
    units_sold += rhs.units_sold;
    revenue += rhs.revenue;
    return *this;
}
```

## 7.3

修改7.1.1节的交易处理程序，令其使用这些成员。

```c++
#include<iostream>
#include<string>

class Sales_data{
public:
    std::string bookNo;
    unsigned units_sold = 0;
    double revenue = 0.0;

    std::string isbn() const{
        return bookNo;
    }
    Sales_data &combine(const Sales_data&);
};

Sales_data& Sales_data::combine(const Sales_data &rhs) {
    units_sold += rhs.units_sold;
    revenue += rhs.revenue;
    return *this;
}

int main(){
    Sales_data total;
    if(std::cin >> total.bookNo >> total.units_sold >> total.revenue){
        Sales_data trans;
        while(std::cin >> trans.bookNo >> trans.units_sold >> trans.revenue){
            if (total.isbn() == trans.isbn()) {
                total.combine(trans);
            } else{
                std::cout << total.bookNo << " " << total.units_sold << " " << total.revenue << std::endl;
                total = trans;
            }
        }
        std::cout << total.bookNo << " " << total.units_sold << " " << total.revenue << std::endl;
    }
    else{
        std::cerr << "No data?" << std::endl;
        return -1;
    }
    return 0;
}
```

## 7.4

编写一个名为`Person`的类，使其表示人员的姓名和地址。使用`string`对象存放这些元素，接下来的练习将不断充实这个类的其他特征。

```c++
#include <iostream>
#include <string>

class Person{
public:
    std::string name;
    std::string address;
};
```

## 7.5

在你的`Person`类中提供一些操作使其能够返回姓名和地址。 这些函数是否应该是`const`的呢？解释原因。

```c++
#include <string>

class Person{
public:
    std::string name;
    std::string address;

    std::string get_name() const {return name;};
    std::string get_address() const {return address;};
};
```

**Note**

这是[成员函数](https://www.zhihu.com/search?q=成员函数&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra={"sourceType"%3A"answer"%2C"sourceId"%3A38388459})定义时会遇到的问题。

比如我定义了一个类FooClass:

```cpp
class FooClass{
public:
    void Foo(){ /*...*/}
private:
    /*...*/
};
```

然后，我在程序中创建了一个FooClass的[常量对象](https://www.zhihu.com/search?q=常量对象&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra={"sourceType"%3A"answer"%2C"sourceId"%3A38388459})A，并试图调用这个成员函数Foo()。

```cpp
const FooClass A; 
A.Foo();
```

但是，对A调用成员函数Foo()将会出错！

A是一个const的对象，但是Foo()只能用于非const的对象。定义成员函数Foo()时，显然不能把调用它的对象写到形参列表里面去声明为一个const，比如Foo(const FooClass* this)。

怎么让Foo()能用于const对象呢？就是给Foo()加上一个[const声明](https://www.zhihu.com/search?q=const声明&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra={"sourceType"%3A"answer"%2C"sourceId"%3A38388459})，如下：

```cpp
class FooClass{
public:
    void Foo() const{ /*...*/}
private:
    /*...*/
};
```

这个时候，对前面创建的const的A就可以调用这个成员函数了。

实际上，成员函数Foo()有一个隐式的形参this，它是自身对象的一个指针，但是不能显式地使用在Foo()的[形参列表](https://www.zhihu.com/search?q=形参列表&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra={"sourceType"%3A"answer"%2C"sourceId"%3A38388459})里。加上const就说明，this指向的对象是[const对象](https://www.zhihu.com/search?q=const对象&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra={"sourceType"%3A"answer"%2C"sourceId"%3A38388459})。

当然，加上了const声明的成员函数，不能对调用它的对象内的成员进行修改（声明为mutable的成员例外）。