# chapter 12

## 12.1

在此代码的结尾，`b1` 和 `b2` 各包含多少个元素？

```c++
StrBlob b1;
{
	StrBlob b2 = {"a", "an", "the"};
	b1 = b2;
	b2.push_back("about");
}
```

四个元素，b1 b2指向同一个底层对象

## 12.2

编写你自己的`StrBlob` 类，包含`const` 版本的 `front` 和 `back`。

```c++
#include <iostream>
#include <map>
#include <vector>
#include <string>
#include <memory>

class StrBlob{
public:
    typedef std::vector<std::string>::size_type size_type;
    StrBlob();
    StrBlob(std::initializer_list<std::string> li);

    size_type size() const {
        return data->size();
    }

    bool empty() const {
        return data->empty();
    }

    void push_back(const std::string &t){
        data->push_back(t);
    }

    void pop_back();
    const std::string& front() const;
    const std::string& back() const;

private:
    std::shared_ptr<std::vector<std::string>> data;
    void check(size_type i, const std::string &msg) const;

};

StrBlob::StrBlob() {
    data = std::make_shared<std::vector<std::string>>();
}
StrBlob::StrBlob(std::initializer_list<std::string> il) {
    data = std::make_shared<std::vector<std::string>>(il);
}

void StrBlob::check(size_type i, const std::string &msg) const{
    if (i >= data->size()){
        throw std::out_of_range(msg);
    }
}

const std::string& StrBlob::front() const {
    check(0, "front on empty StrBlob");
    return data->front();
}

const std::string& StrBlob::back() const {
    check(0, "back on empty StrBlob");
    return data->back();
}

void StrBlob::pop_back(){
    check(0, "pop_back on empty StrBlob");
    data->pop_back();
}


int main() {
    StrBlob b1{"1","3", "5"};
    StrBlob b2=b1;

    std::cout << "b1: " << b1.back() << std::endl;
    std::cout << "b2: " << b2.back() << std::endl;

    {
        StrBlob b3{"2","4", "6"};
        b2 = b3;
    }

    std::cout << "b2 from b3: " << b2.back() << std::endl;
    return 0;
}
```

## 12.3

不需要，添加了const就无法添加或删除元素

## 12.4

在我们的 `check` 函数中，没有检查 `i` 是否大于0。为什么可以忽略这个检查？

因为 `size_type` 是一个无符号整型，当传递给 `check` 的参数小于 0 的时候，参数值会转换成一个正整数。

## 12.5

我们未编写接受一个 `initializer_list explicit` 参数的构造函数。讨论这个设计策略的优点和缺点。



## 12.6

编写函数，返回一个动态分配的 `int` 的`vector`。将此`vector` 传递给另一个函数，这个函数读取标准输入，将读入的值保存在 `vector` 元素中。再将`vector`传递给另一个函数，打印读入的值。记得在恰当的时刻`delete vector`

```c++
#include <vector>
#include <iostream>

std::vector<int>* GetVector(){
    return new std::vector<int>;
}

void GetInput(std::vector<int>* v_ptr){
    int i = 0;
    while(std::cin >> i){
        v_ptr -> push_back(i);
    }
}

void Print(std::vector<int>* v_ptr){
    for(auto ele: *v_ptr){
        std::cout << ele << ", ";
    }
    delete v_ptr;
}

int main(){
    std::vector<int>* v_ptr = GetVector();
    GetInput(v_ptr);
    Print(v_ptr);

    return 0;
};
```

## 12.7

重做上一题，这次使用 `shared_ptr` 而不是内置指针。

```c++
#include <vector>
#include <iostream>
#include <memory>

std::shared_ptr<std::vector<int>> GetVector(){
    std::shared_ptr<std::vector<int>> v_ptr = std::make_shared<std::vector<int>>();
    return v_ptr;
}

void GetInput(std::shared_ptr<std::vector<int>> v_ptr){
    int i = 0;
    while(std::cin >> i){
        v_ptr -> push_back(i);
    }
}

void Print(std::shared_ptr<std::vector<int>> v_ptr){
    for(auto ele: *v_ptr){
        std::cout << ele << ", ";
    }
}

int main(){
    std::shared_ptr<std::vector<int>> v_ptr = GetVector();
    GetInput(v_ptr);
    Print(v_ptr);

    return 0;
};
```

## 12.8

下面的函数是否有错误？如果有，解释错误原因

```c++
bool b() {
	int* p = new int;
	// ...
	return p;
}
```

有错误。`p`会被强制�转换成`bool`，继而没有释放指针 `p` 指向的对象。

## 12.9

解释下面代码执行的结果。

```c++
int *q = new int(42), *r = new int(100);
r = q;
auto q2 = make_shared<int>(42), r2 = make_shared<int>(100);
r2 = q2;
```

`r` 和 `q` 指向 42，而之前 `r` 指向的 100 的内存空间并没有被释放，因此会发生内存泄漏。`r2` 和 `q2` 都是智能指针，当对象空间不被引用的时候会自动释放。

## 12.10

下面的代码调用了第413页中定义的`process` 函数，解释此调用是否正确。如果不正确，应如何修改

```c++
shared_ptr<int> p(new int(42));
process(shared_ptr<int>(p));
```

正确。`shared_ptr<int>(p)` 会创建一个临时的智能指针，这个智能指针与 `p` 引用同一个对象，此时引用计数为 2。当表达式结束时，临时的智能指针被销毁，此时引用计数为 1。

## 12.11

如果我们像下面这样调用 `process`，会发生什么？

```c++
process(shared_ptr<int>(p.get()));
```

这样会创建一个新的智能指针，它的引用计数为 1，这个智能指针所指向的空间与 `p` 相同。在表达式结束后，这个临时智能指针会被销毁，引用计数为 0，所指向的内存空间也会被释放。而导致 `p` 所指向的空间被释放，使得 p` 成为一个空悬指针。

## 12.12

`p` 和 `sp` 的定义如下，对于接下来的对 `process` 的每个调用，如果合法，解释它做了什么，如果不合法，解释错误原因：

```c++
auto p = new int();
auto sp = make_shared<int>();
(a) process(sp);
(b) process(new int());
(c) process(p);
(d) process(shared_ptr<int>(p));
```

- (a) 合法。将`sp` 拷贝给 `process`函数的形参，在函数里面引用计数为 2，函数结束后引用计数为 1。
- (b) 不合法。不能从内置指针隐式转换为智能指针。
- (c) 不合法。不能从内置指针隐式转换为智能指针。
- (d) 合法。但是智能指针和内置指针一起使用可能会出现问题，在表达式结束后智能指针会被销毁，它所指向的对象也被释放。而此时内置指针 `p` 依旧指向该内存空间。之后对内置指针 `p` 的操作可能会引发错误。

## 12.13

如果执行下面的代码，会发生什么？

```c++
auto sp = make_shared<int>();
auto p = sp.get();
delete p;
```

智能指针 `sp` 所指向空间已经被释放，再对 `sp` 进行操作会出现错误。

## 12.14

编写你自己版本的用 `shared_ptr` 管理 `connection` 的函数。

```c++
struct connection{
    std::string start;
    std::string end;
};

void openConn(connection* p) {
    std::cout << "Opening connection from " << p->start << " to " << p->end << std::endl;
}

void closeConn(connection* p) {
    std::cout << "Closing connection from " << p->start << " to " << p->end << std::endl;
    delete p;
}

std::shared_ptr<connection> createConnection(const std::string& start, const std::string& end) {
    connection* conn = new connection{start, end};
    openConn(conn);
    return std::shared_ptr<connection>(conn, closeConn);
}
```

## 12.15

重写上一题的程序，用 `lambda` 代替`end_connection` 函数。

```c++
struct connection {
    std::string start;
    std::string end;
};

void openConn(connection* p) {
    std::cout << "Opening connection from " << p->start << " to " << p->end << std::endl;
}

std::shared_ptr<connection> createConnection(const std::string& start, const std::string& end) {
    connection* conn = new connection{start, end};
    openConn(conn);
    
    // 使用 lambda 表达式作为自定义删除器
    return std::shared_ptr<connection>(conn, [](connection* p) {
        std::cout << "Closing connection from " << p->start << " to " << p->end << std::endl;
        delete p;
    });
}
```

## 12.16

如果你试图拷贝或赋值 `unique_ptr`，编译器并不总是能给出易于理解的错误信息。编写包含这种错误的程序，观察编译器如何诊断这种错误。

```c++
#include <memory>

int main() {
    std::unique_ptr<int> p1(new int(5));
    std::unique_ptr<int> p2 = p1; // 错误：试图复制 unique_ptr

    return 0;
}
```

/usr/bin/c++   -g -std=gnu++11 -MD -MT CMakeFiles/exec.dir/main.cpp.o -MF CMakeFiles/exec.dir/main.cpp.o.d -o CMakeFiles/exec.dir/main.cpp.o -c /home/lumos/learnshs/c++/chapter6/main.cpp
/home/lumos/learnshs/c++/chapter6/main.cpp: In function ‘int main()’:
/home/lumos/learnshs/c++/chapter6/main.cpp:5:31: error: use of deleted function ‘std::unique_ptr<_Tp, _Dp>::unique_ptr(const std::unique_ptr<_Tp, _Dp>&) [with _Tp = int; _Dp = std::default_delete<int>]’
    5 |     std::unique_ptr<int> p2 = p1; // 错误：试图复制 unique_ptr
      |                               ^~
In file included from /usr/include/c++/9/memory:80,
                 from /home/lumos/learnshs/c++/chapter6/main.cpp:1:
/usr/include/c++/9/bits/unique_ptr.h:414:7: note: declared here
  414 |       unique_ptr(const unique_ptr&) = delete;
      |       ^~~~~~~~~~
ninja: build stopped: subcommand failed.



## 12.17

下面的 `unique_ptr` 声明中，哪些是合法的，哪些可能导致后续的程序错误？解释每个错误的问题在哪里

```c++
int ix = 1024, *pi = &ix, *pi2 = new int(2048);
typedef unique_ptr<int> IntP;
(a) IntP p0(ix);
(b) IntP p1(pi);
(c) IntP p2(pi2);
(d) IntP p3(&ix);
(e) IntP p4(new int(2048));
(f) IntP p5(p2.get());
```

- (a) 不合法。在定义一个 `unique_ptr` 时，需要将其绑定到一个`new` 返回的指针上。
- (b) 不合法。理由同上。
- (c) 合法。但是也可能会使得 `pi2` 成为空悬指针。
- (d) 不合法。当 `p3` 被销毁时，它试图释放一个栈空间的对象。
- (e) 合法。
- (f) 不合法。`p5` 和 `p2` 指向同一个对象，当 `p5` 和 `p2` 被销毁时，会使得同一个指针被释放两次。

## 12.18

`shared_ptr` 为什么没有 `release` 成员？

`release` 成员的作用是放弃控制权并返回指针，因为在某一时刻只能有一个 `unique_ptr` 指向某个对象，`unique_ptr` 不能被赋值，所以要使用 `release` 成员将一个 `unique_ptr` 的指针的所有权传递给另一个 `unique_ptr`。而 `shared_ptr` 允许有多个 `shared_ptr` 指向同一个对象，因此不需要 `release` 成员

## 12.19

定义你自己版本的 `StrBlobPtr`，更新 `StrBlob` 类，加入恰当的 `friend` 声明以及 `begin` 和 `end` 成员。





