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

## 7.6



```c++
#include <iostream>
#include <string>

class Sales_data{
public:
    std::string const& isbn() const {
        return bookNo;
    }
    Sales_data& combine(const Sales_data&);
    std::string bookNo;
    unsigned units_sold = 0;
    double revenue = 0.0;
};

Sales_data& Sales_data::combine(const Sales_data& rhs) {
    units_sold += units_sold;
    revenue += rhs.revenue;
    return *this;
}

std::istream& read(std::istream &is, Sales_data &item){
    double price = 0;
    is >> item.bookNo >> item.units_sold >> price;
    item.revenue = price* item.units_sold;
    return is;
}

std::ostream &print(std::ostream &os, const Sales_data &item){
    os << item.isbn() << " " << item.units_sold << " " << item.revenue;
    return os;
}

Sales_data add(const Sales_data &lhs, const Sales_data &rhs){
    Sales_data sum = lhs;
    sum.combine(rhs);
    return sum;
}

int main(){
    ;
}
```

## 7.7

使用这些新函数重写7.1.2节练习中的程序。

```c++
#include <iostream>
#include <string>

class Sales_data{
public:
    std::string const& isbn() const {
        return bookNo;
    }
    Sales_data& combine(const Sales_data&);
    std::string bookNo;
    unsigned units_sold = 0;
    double revenue = 0.0;
};

Sales_data& Sales_data::combine(const Sales_data& rhs) {
    units_sold += units_sold;
    revenue += rhs.revenue;
    return *this;
}

std::istream& read(std::istream &is, Sales_data &item){
    double price = 0;
    is >> item.bookNo >> item.units_sold >> price;
    item.revenue = price* item.units_sold;
    return is;
}

std::ostream &print(std::ostream &os, const Sales_data &item){
    os << item.isbn() << " " << item.units_sold << " " << item.revenue;
    return os;
}

Sales_data add(const Sales_data &lhs, const Sales_data &rhs){
    Sales_data sum = lhs;
    sum.combine(rhs);
    return sum;
}

int main(){
    Sales_data total;
    if(read(std::cin, total)){
        Sales_data trans;
        while(read(std::cin, trans)){
            if(total.isbn() == trans.isbn()) total.combine(trans);
            else{
                print(std::cout, total) << std::endl;
                total = trans;
            }
        }
        print(std::cout, total) << std::endl;
    }
    else
    {
        std::cerr << "No data?" << std::endl;
        return -1;
    }
    return 0;
}
```

## 7.8

为什么`read`函数将其`Sales_data`参数定义成普通的引用，而`print`函数将其参数定义成常量引用？

因为read函数中要改变Sales_data参数对应对象的值，而print不用。

## 7.9

对于7.1.2节练习中代码，添加读取和打印`Person`对象的操作。

```c++
#include <iostream>
#include <string>

class Person {
public:
    std::string name;
    std::string address;

};

void read(std::istream& is, Person& p){
    is >> p.name >> p.address; //这里没用返回值，如果向上一题那样使用istream& 作为返回值，可以判断读取是否成功
}
void print(std::ostream& os, const Person& p) {
    os << p.name << " " << p.address;
}

int main(){
    Person p;
    read(std::cin, p);
    print(std::cout, p);
    return 0;
}
```

## 7.10

在下面这条`if`语句中，条件部分的作用是什么？

```c++
if (read(read(cin, data1), data2)) //等价read(std::cin, data1);read(std::cin, data2);
```

if可以判断读取是否成功

## 7.11

```c++
#include <string>
#include <iostream>

struct Sales_data {
    Sales_data() = default;
    Sales_data(const std::string &s):bookNo(s) { }
    Sales_data(const std::string &s, unsigned n, double p):bookNo(s), units_sold(n), revenue(n*p){ }
    Sales_data(std::istream &is);

    std::string isbn() const { return bookNo; };
    Sales_data& combine(const Sales_data&);

    std::string bookNo;
    unsigned units_sold = 0;
    double revenue = 0.0;
};

// nonmember functions
std::istream &read(std::istream &is, Sales_data &item)
{
    double price = 0;
    is >> item.bookNo >> item.units_sold >> price;
    item.revenue = price * item.units_sold;
    return is;
}

std::ostream &print(std::ostream &os, const Sales_data &item)
{
    os << item.isbn() << " " << item.units_sold << " " << item.revenue;
    return os;
}

Sales_data add(const Sales_data &lhs, const Sales_data &rhs)
{
    Sales_data sum = lhs;
    sum.combine(rhs);
    return sum;
}

// member functions.
Sales_data::Sales_data(std::istream &is)
{
    read(is, *this);
}

Sales_data& Sales_data::combine(const Sales_data& rhs)
{
    units_sold += rhs.units_sold;
    revenue += rhs.revenue;
    return *this;
}

int main()
{
    Sales_data item1;
    print(std::cout, item1) << std::endl;

    Sales_data item2("0-201-78345-X");
    print(std::cout, item2) << std::endl;

    Sales_data item3("0-201-78345-X", 3, 20.00);
    print(std::cout, item3) << std::endl;

    Sales_data item4(std::cin);
    print(std::cout, item4) << std::endl;

    return 0;
}
```

## 7.12

```c++
#include <string>
#include <iostream>

struct Sales_data {
    Sales_data() = default;
    Sales_data(const std::string &s):bookNo(s) { }
    Sales_data(const std::string &s, unsigned n, double p):bookNo(s), units_sold(n), revenue(n*p){ }
    Sales_data(std::istream &is);

    std::string isbn() const { return bookNo; };
    Sales_data& combine(const Sales_data&);

    std::string bookNo;
    unsigned units_sold = 0;
    double revenue = 0.0;
};

// nonmember functions
std::istream &read(std::istream &is, Sales_data &item)
{
    double price = 0;
    is >> item.bookNo >> item.units_sold >> price;
    item.revenue = price * item.units_sold;
    return is;
}

std::ostream &print(std::ostream &os, const Sales_data &item)
{
    os << item.isbn() << " " << item.units_sold << " " << item.revenue;
    return os;
}

Sales_data add(const Sales_data &lhs, const Sales_data &rhs)
{
    Sales_data sum = lhs;
    sum.combine(rhs);
    return sum;
}

// member functions.
Sales_data::Sales_data(std::istream &is)
{
    read(is, *this);
}

Sales_data& Sales_data::combine(const Sales_data& rhs)
{
    units_sold += rhs.units_sold;
    revenue += rhs.revenue;
    return *this;
}

int main()
{
    Sales_data item1;
    print(std::cout, item1) << std::endl;

    Sales_data item2("0-201-78345-X");
    print(std::cout, item2) << std::endl;

    Sales_data item3("0-201-78345-X", 3, 20.00);
    print(std::cout, item3) << std::endl;

    Sales_data item4(std::cin);
    print(std::cout, item4) << std::endl;

    return 0;
}
```

## 7.13

```c++
int main()
{
    Sales_data total(std::cin);
    if (!total.isbn().empty())
    {
        std::istream &is = std::cin;
        while (is) {
            Sales_data trans(is);
            if (!is) break;
            if (total.isbn() == trans.isbn())
                total.combine(trans);
            else {
                print(std::cout, total) << std::endl;
                total = trans;
            }
        }
        print(std::cout, total) << std::endl;
    }
    else
    {
        std::cerr << "No data?!" << std::endl;
        return -1;
    }

    return 0;
}
```

## 7.14

编写一个构造函数，令其用我们提供的类内初始值显式地初始化成员。

```c++
Sales_data() : units_sold(0) , revenue(0) { }
```

## 7.15

```c++
class Person {
public:
    std::string name;
    std::string address;
    Person() = default;
    Person(const std::string& sname, const std::string& saddress):name(name), address(address){}
    Person(std::istream& is){read(is, *this);}
    std::istream &read(std::istream&, Person&);
};
```

## 7.16

在类的定义中对于访问说明符出现的位置和次数有限定吗？ 如果有，是什么？什么样的成员应该定义在`public`说明符之后？ 什么样的成员应该定义在`private`说明符之后？

解：

**

在类的定义中对于访问说明符出现的位置和次数**没有限定**。

每个访问说明符指定了接下来的成员的访问级别，其有效范围直到出现下一个访问说明符或者达到类的结尾处为止。

如果某个成员能够在整个程序内都被访问，那么它应该定义为`public`; 如果某个成员只能在类内部访问，那么它应该定义为`private`。

**

## 7.17

使用`class`和`struct`时有区别吗？如果有，是什么？

解：

`class`和`struct`的唯一区别是默认的访问级别不同。

## 7.18

封装是何含义？它有什么用处？

封装将类内部分成员设置为类外不可见，而提供部分接口给外面，这样的行为叫封装

封装的作用：

- 确保用户的代码不会无意见破坏被封装对象的状态
- 被封装的对象可以随时改变，而不需要调整用户端代码

## 7.19

在你的`Person`类中，你将把哪些成员声明成`public`的？ 哪些声明成`private`的？ 解释你这样做的原因。

构造函数、`getName()`、`getAddress()`函数将设为`public`。 `name`和 `address` 将设为`private`。 函数是暴露给外部的接口，因此要设为`public`； 而数据则应该隐藏让外部不可见。

## 7.20

友元在什么时候有用？请分别举出使用友元的利弊。

解：

当其他类或者函数想要访问当前类的私有变量时，这个时候应该用友元。

利：

与当前类有关的接口函数能直接访问类的私有变量。

弊：

牺牲了封装性与可维护性。

## 7.21

```c++
#include <string>
#include <iostream>

class Sales_data {
    friend std::istream &read(std::istream &is, Sales_data &item);
    friend std::ostream &print(std::ostream &os, const Sales_data &item);
    friend Sales_data add(const Sales_data &lhs, const Sales_data &rhs);

public:
    Sales_data() = default;
    Sales_data(const std::string &s):bookNo(s) { }
    Sales_data(const std::string &s, unsigned n, double p):bookNo(s), units_sold(n), revenue(n*p){ }
    Sales_data(std::istream &is) { read(is, *this); }

    std::string isbn() const { return bookNo; };
    Sales_data& combine(const Sales_data&);

private:
    std::string bookNo;
    unsigned units_sold = 0;
    double revenue = 0.0;
};

// member functions.
Sales_data& Sales_data::combine(const Sales_data& rhs)
{
    units_sold += rhs.units_sold;
    revenue += rhs.revenue;
    return *this;
}

// friend functions
std::istream &read(std::istream &is, Sales_data &item)
{
    double price = 0;
    is >> item.bookNo >> item.units_sold >> price;
    item.revenue = price * item.units_sold;
    return is;
}

std::ostream &print(std::ostream &os, const Sales_data &item)
{
    os << item.isbn() << " " << item.units_sold << " " << item.revenue;
    return os;
}

Sales_data add(const Sales_data &lhs, const Sales_data &rhs)
{
    Sales_data sum = lhs;
    sum.combine(rhs);
    return sum;
}

int main()
{
    Sales_data item1;
    print(std::cout, item1) << std::endl;

    Sales_data item2("0-201-78345-X");
    print(std::cout, item2) << std::endl;

    Sales_data item3("0-201-78345-X", 3, 20.00);
    print(std::cout, item3) << std::endl;

    Sales_data item4(std::cin);
    print(std::cout, item4) << std::endl;

    return 0;
}
```

## 7.22

```c++
#include <string>
#include <iostream>

class Person {
    friend std::istream &read(std::istream &is, Person &person);
    friend std::ostream &print(std::ostream &os, const Person &person);

public:
    Person() = default;
    Person(const std::string sname, const std::string saddr):name(sname), address(saddr){ }
    Person(std::istream &is){ read(is, *this); }

    std::string getName() const { return name; }
    std::string getAddress() const { return address; }
private:
    std::string name;
    std::string address;
};

std::istream &read(std::istream &is, Person &person)
{
    is >> person.name >> person.address;
    return is;
}

std::ostream &print(std::ostream &os, const Person &person)
{
    os << person.name << " " << person.address;
    return os;
}
```

## 7.23

编写你自己的`Screen`类型。

```c++
#include <string>

class Screen {
    public:
        using pos = std::string::size_type;

        Screen() = default;
        Screen(pos ht, pos wd, char c):height(ht), width(wd), contents(ht*wd, c){ }

        char get() const { return contents[cursor]; }
        char get(pos r, pos c) const { return contents[r*width+c]; }

    private:
        pos cursor = 0;
        pos height = 0, width = 0;
        std::string contents;
};
```

## 7.24

给你的`Screen`类添加三个构造函数：一个默认构造函数；另一个构造函数接受宽和高的值，然后将`contents`初始化成给定数量的空白；第三个构造函数接受宽和高的值以及一个字符，该字符作为初始化后屏幕的内容。

```c++
#include <string>

class Screen {
public:
    typedef std::string::size_type pos;

    Screen() = default;
    Screen(pos ht, pos wd, pos ):height(ht), width(wd), contents(ht*wd, ' '){ }
    Screen(pos ht, pos wd, char c):height(ht), width(wd), contents(ht*wd, c){ }

    char get() const { return contents[cursor]; }
    char get(pos r, pos c) const { return contents[r*width+c]; }

private:
    pos cursor = 0;
    pos height = 0, width = 0;
    std::string contents;
};
```

## 7.25

`Screen`能安全地依赖于拷贝和赋值操作的默认版本吗？ 如果能，为什么？如果不能？为什么？

可以

`Screen`的成员只有内置类型和`string`，因此能安全地依赖于拷贝和赋值操作的默认版本。

## 7.26

将`Sales_data::avg_price`定义成内联函数。

在定义处加上inline:

```c++
inline double Sales_data::avg_price() const
{
    return units_sold ? revenue/units_sold : 0;
}
```

## 7.27

给你自己的`Screen`类添加`move`、`set` 和`display`函数，通过执行下面的代码检验你的类是否正确。

```c++
Screen myScreen(5, 5, 'X');
myScreen.move(4, 0).set('#').display(cout);
cout << "\n";
myScreen.display(cout);
cout << "\n";
```
