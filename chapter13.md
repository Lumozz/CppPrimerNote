# chapter 13

## 13.1

拷贝构造函数是什么？什么时候使用它？

拷贝构造函数是一种特殊的拷贝函数，用于初始化一个新对象作为原有对象的副本。它接受一个同类型对象的常量引用作为参数。

拷贝构造函数主要在以下情况下使用：

1. **当一个对象作为值传给函数时**。在这种情况下，拷贝构造函数用于创建函数的形参，这是一个源对象的副本。
2. **当函数返回一个对象时**。在这种情况下，拷贝构造函数用于从函数返回一个临时对象。然而，现代编译器通常会使用一种称为返回值优化（RVO）的技术来消除这种不必要的拷贝。
3. **当用一个对象初始化另一个对象时**。如 `MyClass obj2 = obj1;`，在这种情况下，`obj2`是`obj1`的一个副本，`obj2`的初始化会调用拷贝构造函数。

## 13.2

解释为什么下面的声明是非法的：

```c++
Sales_data::Sales_data(Sales_data rhs);
```

形参应该是引用类型

为什么拷贝构造函数必须是引用类型？

避免无限递归

## 13.3

当我们拷贝一个`StrBlob`时，会发生什么？拷贝一个`StrBlobPtr`呢？

**拷贝一个 `StrBlob` 对象**

`StrBlob` 类的数据成员是一个 `std::shared_ptr`，指向一个 `std::vector<std::string>` 容器。当执行拷贝操作时，会使用 `std::shared_ptr` 的拷贝构造函数，它会拷贝指针并增加引用计数，而不是拷贝底层的 `std::vector<std::string>` 容器。

因此，拷贝一个 `StrBlob` 对象会得到一个新的 `StrBlob` 对象，它和原对象共享同一个底层 `std::vector<std::string>` 容器。任何对其中一个 `StrBlob` 对象的修改（例如添加或删除元素）都会影响另一个对象，因为它们共享同一个底层数据。

**拷贝一个 `StrBlobPtr` 对象**

与 `StrBlob` 类似，`StrBlobPtr` 的拷贝操作也会执行 `std::weak_ptr` 和 `std::size_t` 的拷贝构造函数。这样得到的新 `StrBlobPtr` 对象和原对象共享同一个底层 `std::vector<std::string>` 容器，并且新对象的当前位置与原对象的当前位置相同。

但是需要注意的是，`StrBlobPtr` 对象并不拥有底层的 `std::vector<std::string>` 容器。当最后一个拥有底层容器的 `StrBlob` 对象被销毁时，底层容器也会被销毁，这时所有的 `StrBlobPtr` 对象都将变得无效。

## 13.4

假定 `Point` 是一个类类型，它有一个`public`的拷贝构造函数，指出下面程序片段中哪些地方使用了拷贝构造函数：

```c++
Point global;
Point foo_bar(Point arg) // 1
{
	Point local = arg, *heap = new Point(global); // 2: Point local = arg,  3: Point *heap = new Point(global) 
	*heap = local; 
	Point pa[4] = { local, *heap }; // 4, 5
	return *heap;  // 6
}
```

六个地方都使用了拷贝构造

## 13.5

给定下面的类框架，编写一个拷贝构造函数，拷贝所有成员。你的构造函数应该动态分配一个新的`string`，并将对象拷贝到`ps`所指向的位置，而不是拷贝ps本身：

```c++
class HasPtr {
public:
	HasPtr(const std::string& s = std::string()):
		ps(new std::string(s)), i(0) { }
private:
	std::string *ps;
	int i;
}
```



```c++
#include<string>
#include<iostream>

class HasPtr {
public:
    HasPtr(const std::string& s = std::string()):
            ps(new std::string(s)), i(0) { };
    HasPtr(const HasPtr& has_ptr):
            ps(new std::string(*has_ptr.ps)){};

    std::string GetStr(){
        return ps == nullptr ? "" : *ps;
    }

    void del(){
        delete ps;
        ps = nullptr;
    }
private:
    std::string *ps;
    int i;
};

int main(){
    HasPtr h1("12345");
    HasPtr h2(h1);

    std::cout << "h1: " << h1.GetStr() << std::endl;
    std::cout << "h2: " << h2.GetStr() << std::endl;

    h1.del();

    if(!h1.GetStr().empty()){
        std::cout << "h1: " << h1.GetStr() << std::endl;
    } else {
        std::cout << "h1: " << "h1 ps empty" << std::endl;
    }

    if(!h2.GetStr().empty()){
        std::cout <<"h2: " << h2.GetStr() << std::endl;
    } else {
        std::cout <<"h2: "  << "h2 ps empty" << std::endl;
    }
}
```

```sh
# output
h1: 12345
h2: 12345
h1: h1 ps empty
h2: 12345
```



## 13.6

拷贝赋值运算符是什么？什么时候使用它？合成拷贝赋值运算符完成什么工作？什么时候会生成合成拷贝赋值运算符？

拷贝赋值运算符：用于定义，一个对象如何通过赋值`=`操作符，从一个同类型对象那里获得数据。当把一个对象赋值给一个已经存在的同类型对象时，会调用拷贝赋值运算符。合成拷贝运算符会将右侧运算对象的每个非`static`成员赋予左侧运算对象的对应成员。如果一个类未定义自己的拷贝赋值运算符，编译器会生成一个合成拷贝赋值运算符。

## 13.7

当我们将一个`StrBlob`赋值给另一个`StrBlob`，会发生什么？赋值`StrBlobPtr`呢？

- `StrBlob`
	- 赋值操作通常会调用拷贝赋值运算符，如果没有用户自定义的拷贝赋值运算符，将会调用编译器合成的拷贝赋值运算符。
	- 合成的拷贝赋值运算符会逐个复制非静态成员，对于`std::shared_ptr`成员，它会增加左侧对象(赋值后的对象)所持有的`std::vector`的引用计数。
	- 由于`StrBlob`内部的`std::shared_ptr`是共享所有权的智能指针，赋值后两个`StrBlob`对象将共享对同一`std::vector`的所有权。如果一个`StrBlob`对象对`std::vector`做出了修改，这些修改对另一个`StrBlob`对象也是可见的。
	- 当最后一个拥有共享所有权的`StrBlob`对象被销毁时，`std::vector`将会被自动删除。
- `StrBlobPtr`
	- 同样，赋值操作会调用拷贝赋值运算符，如果没有用户自定义的，则调用编译器合成的。
	- 拷贝赋值运算符会复制`StrBlobPtr`的所有非静态成员，包括`std::weak_ptr`和当前索引`curr`。
	- `std::weak_ptr`的赋值不会改变`std::vector`的引用计数。它只是简单地复制了一个指针，指向同一个`vector`，不影响`vector`的生命周期。
	- 赋值后，两个`StrBlobPtr`对象指向同一个`StrBlob`内部的`vector`



## 13.8

```c++
#include<string>
#include<iostream>

class HasPtr {
public:
    HasPtr(const std::string& s = std::string()):
            ps(new std::string(s)), i(0) { };
    HasPtr(const HasPtr& has_ptr):
            ps(new std::string(*has_ptr.ps)){};

    HasPtr& operator=(const HasPtr& has_ptr){
        ps = new std::string(*has_ptr.ps);
        *ps += "6";
        return *this;
    };

    std::string GetStr(){
        return ps == nullptr ? "" : *ps;
    }

    void del(){
        delete ps;
        ps = nullptr;
    }
private:
    std::string *ps;
    int i;
};

int main(){
    HasPtr h1("12345");
    HasPtr h2;
    h2 = h1;

    std::cout << "h1: " << h1.GetStr() << std::endl;
    std::cout << "h2: " << h2.GetStr() << std::endl;

    h1.del();

    if(!h1.GetStr().empty()){
        std::cout << "h1: " << h1.GetStr() << std::endl;
    } else {
        std::cout << "h1: " << "h1 ps empty" << std::endl;
    }

    if(!h2.GetStr().empty()){
        std::cout <<"h2: " << h2.GetStr() << std::endl;
    } else {
        std::cout <<"h2: "  << "h2 ps empty" << std::endl;
    }
}
```

```sh
h1: 12345
h2: 123456
h1: h1 ps empty
h2: 123456
```

## 13.9

