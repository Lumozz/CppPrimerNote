 # chapter 9

## 9.1


>对于下面的程序任务，`vector`、`deque`和`list`哪种容器最为适合？解释你的选择的理由。如果没有哪一种容器优于其他容器，也请解释理由。

- (a) 读取固定数量的单词，将它们按字典序插入到容器中。我们将在下一章中看到，关联容器更适合这个问题。

  list 插入操作比较快

- (b) 读取未知数量的单词，总是将单词插入到末尾。删除操作在头部进行。

  deque 头尾删除、插入操作较快

- (c) 从一个文件读取未知数量的整数。将这些数排序，然后将它们打印到标准输出。

  vector 只需要在末尾插入

##  9.2

定义一个`list`对象，其元素类型是`int`的`deque`。

```c++
std::list<std::deque<int>> l;
```

## 9.3

构成迭代器范围的迭代器有何限制？



两个迭代器 `begin` 和 `end`需满足以下条件：

- 它们指向同一个容器中的元素，或者是容器最后一个元素之后的位置。
- 我们可以通过反复递增`begin`来到达`end`。换句话说，`end` 不在`begin`之前。

## 9.4

编写函数，接受一对指向`vector<int>`的迭代器和一个`int`值。在两个迭代器指定的范围中查找给定的值，返回一个布尔值来指出是否找到。



```c++
#include<vector>
#include<iostream>

int main(){
    int target(8);
    std::vector<int> v{1,2,3,4,5,6,7};
    std::vector<int>::iterator it = v.begin();
    while(it != v.end()){
        if(*it == target){
            std::cout << "Found" << std::endl;
            return 0;
        }
        ++it;
    }
    std::cout << "Not found" << std::endl;
    return 0;
}
```

## 9.5

重写上一题的函数，返回一个迭代器指向找到的元素。注意，程序必须处理未找到给定值的情况。

```c++
#include<vector>
#include<iostream>

std::vector<int>::iterator find(int target){
    std::vector<int> v{1,2,3,4,5,6,7};
    std::vector<int>::iterator it = v.begin();
    while(it != v.end()){
        if(*it == target){
            std::cout << "Found" << std::endl;
            return it;
        }
        ++it;
    }
    std::cout << "Not found" << std::endl;
    return v.end();
}

int main(){
    int target(2);

    std::vector<int>::iterator r = find(target);

}
```

## 9.6

下面的程序有何错误？你应该如何修改它？

```c++
list<int> lst1;
list<int>::iterator iter1 = lst1.begin(),
					iter2 = lst1.end();
while (iter1 < iter2) /* ... */
```

```c++
while (iter1 != iter2)
```

## 9.7

为了索引`int`的`vector`中的元素，应该使用什么类型？

GPT4：

为了索引 `std::vector<int>` 中的元素，你应该使用 `std::vector<int>::size_type` 类型。这是一个无符号整数类型，通常与 `size_t` 类型相同，足以表示向量中的任何元素的索引。

这是一个例子：

```cpp
std::vector<int> vec = {1, 2, 3, 4, 5};
for (std::vector<int>::size_type i = 0; i < vec.size(); ++i) {
    std::cout << vec[i] << std::endl;
}
```

在这个例子中，`i` 的类型是 `std::vector<int>::size_type`，它被用作 `vec` 的索引。

使用 `std::vector<int>::size_type` 而不是 `int` 或 `unsigned int` 的好处是它确保了你的代码在所有平台上都能正确工作，无论向量的大小如何。一些平台可能允许非常大的向量，这些向量的大小可能超过 `int` 或 `unsigned int` 的最大值。如果你使用 `std::vector<int>::size_type`，你的代码就能在这些平台上正确工作。

## 9.8

为了读取`string`的`list`中的元素，应该使用什么类型？如果写入`list`，又应该使用什么类型？

GPT4：

可以使用 `std::list<std::string>::iterator` 来遍历和修改 `std::list<std::string>` 中的元素。以下是一个例子：

```cpp
std::list<std::string> lst = {"Hello", "World", "!"};

for (std::list<std::string>::iterator it = lst.begin(); it != lst.end(); ++it) {
    *it = "Modified";  // 修改元素
    std::cout << *it << std::endl;
}
```

在这个例子中，我们使用 `std::list<std::string>::iterator` 来遍历列表 `lst` 并修改其元素。

然而，如果你只是想读取 `std::list<std::string>` 中的元素，而不修改它们，那么最好使用 `std::list<std::string>::const_iterator`，以明确表示你不打算修改这些元素。这可以帮助防止意外修改元素，而且可以使你的代码的意图更清晰。

另外，如果你想向 `std::list<std::string>` 中添加新元素，你应该使用 `std::list::push_back` 或 `std::list::insert` 等方法，而不是直接使用迭代器。这是因为迭代器通常用于访问和修改已存在的元素，而不是添加新元素。



## 9.9

`begin`和`cbegin`两个函数有什么不同？

begin是普通迭代器，cbegin是常量迭代器。

## 9.10

```c++
下面4个对象分别是什么类型？
vector<int> v1;
const vector<int> v2;
auto it1 = v1.begin(), it2 = v2.begin();
auto it3 = v1.cbegin(), it4 = v2.cbegin();
```

it1: vector<int>::iterator

it2: vector<int>::const_iterator

it3: vector<int>::const_iterator

it4: vector<int>::const_iterator



GPT4：

常量容器只有vector<int>::const_iterator而没有vector<int>::iterator吗？

是的，对于一个常量容器，例如`const std::vector<int> v`，你只能获取到`std::vector<int>::const_iterator`，不能获取到`std::vector<int>::iterator`。原因是`std::vector<int>::iterator`允许修改元素，这与容器的常量性质矛盾。

在C++标准库中，`begin()`和`end()`函数对于常量和非常量对象有两个重载版本。对于非常量对象，这两个函数返回`std::vector<int>::iterator`，而对于常量对象，它们返回`std::vector<int>::const_iterator`。

在你的例子中，`v2`是一个常量对象，因此`v2.begin()`和`v2.end()`返回的是`std::vector<int>::const_iterator`。

这就是为什么对于常量容器，我们只能获取到常量迭代器。这确保了我们不会意外地修改常量容器的元素。

## 9.11

对6种创建和初始化`vector`对象的方法，每一种都给出一个实例。解释每个`vector`包含什么值。

```cpp
#include <iostream>
#include <vector>
#include <iterator>
#include <algorithm>

int main(){
    std::cout << "running" << std::endl;
    // C c
    std::vector<int> v1;
    std::copy(v1.begin(), v1.end(), std::ostream_iterator<int>(std::cout, " "));
    // C c1(c2)
    std::vector<int> c1={1,2,3,4};
    std::vector<int> c2(c1);
    std::copy(c2.begin(), c2.end(), std::ostream_iterator<int>(std::cout, " "));
    // C c(a,b,c)
    std::vector<int> c3{1,2,3,4};
    std::copy(c3.begin(), c3.end(), std::ostream_iterator<int>(std::cout, " "));
    std::vector<int> c4={1,2,3,4};
    std::copy(c4.begin(), c4.end(), std::ostream_iterator<int>(std::cout, " "));
    //c (b,e)
    std::vector<int> c5(c3.begin(), c3.end());
    std::copy(c5.begin(), c5.end(), std::ostream_iterator<int>(std::cout, " "));
    
}

```

## 9.12

对于接受一个容器创建其拷贝的构造函数，和接受两个迭代器创建拷贝的构造函数，解释它们的不同。

如果一个构造函数接受一个容器，拷贝这个容器的内容作为新容器的内容，则要求两个容器的**容器类型**和**元素类型**都相同；

如果一个构造函数接受另一个容器的迭代器，以此拷贝另一个容器的内容，则不要求**容器类型**相同。且不要求**容器内容**的类型完全相同，只要求两种类型可以进行类型转换即可。

## 9.13

如何从一个`list<int>`初始化一个`vector<double>`？从一个`vector<int>`又该如何创建？编写代码验证你的答案。

从不同类型的容器初始化新容器，不能直接使用拷贝构造函数，应该考虑使用迭代器拷贝。

```c++
#include <iostream>
#include <vector>
#include <list>
#include <iterator>
#include <algorithm>

int main(){
    std::list<int> l={1,2,3};
    std::vector<double> v(l.begin(), l.end());

    std::copy(v.begin(), v.end(), std::ostream_iterator<double>(std::cout, " "));

    std::vector<int> v_origin = {5,6,7};
    std::vector<double> v2(v_origin.begin(), v_origin.end());

    std::copy(v2.begin(), v2.end(), std::ostream_iterator<double>(std::cout, " "));

}
```

## 9.14

编写程序，将一个`list`中的`char *`指针元素赋值给一个`vector`中的`string`。

```c++
#include <iostream>
#include <vector>
#include <string>
#include <list>
#include <iterator>
#include <algorithm>

int main(){
    std::list<const char* > l={"abc", "123"};
    std::vector<std::string> v;
    v.assign(l.begin(), l.end());
    std::copy(v.begin(), v.end(), std::ostream_iterator<std::string>(std::cout, " "));

}
```

## 9.15

编写程序，判定两个`vector<int>`是否相等。

```c++
#include <iostream>
#include <vector>
#include <algorithm>

int main(){
    std::vector<int> v1 = {1,2,3,5};
    std::vector<int> v2 = {1,2,3,4,5};

    std::cout << (v1 < v2 ? "true" : "false") << std::endl;
}
```

## 9.16

重写上一题的程序，比较一个list中的元素和一个vector中的元素。

不同的容器类不能直接比较，因此可用基于迭代器的拷贝构造，构造新的容器，让二者可以比较。

```c++
#include <iostream>
#include <vector>
#include <list>
#include <algorithm>

int main(){
    std::list<int> l = {1,2,3,4};
    std::vector<int> v = {1,2,3,4,5};

    std::cout << (std::vector<int>(l.begin(), l.end()) < v ? "true" : "false") << std::endl;
}
```

## 9.17

假定`c1`和`c2`是两个容器，下面的比较操作有何限制？

```c++
if (c1 < c2)
```

两个容器的类型必须相同，其元素类型必须相同

其元素支持关系运算符



## 9.18

编写程序，从标准输入读取`string`序列，存入一个`deque`中。编写一个循环，用迭代器打印`deque`中的元素。

```c++
#include <iostream>
#include <deque>

int main(){
    std::string s;
    std::deque<std::string> dq;
    while(std::cin >> s){
        dq.push_back(s);
    }
    for(std::deque<std::string>::iterator si = dq.begin(); si != dq.end(); ++si){
        std::cout << *si << std::endl;
    }
}
```

## 9.19

重写上一题的程序，用`list`替代`deque`。列出程序要做出哪些改变。

```c++
#include <iostream>
#include <list>

int main(){
    std::string s;
    std::list<std::string> dq;
    while(std::cin >> s){
        dq.push_back(s);
    }
    for(std::list<std::string>::iterator si = dq.begin(); si != dq.end(); ++si){
        std::cout << *si << std::endl;
    }
}
```

## 9.20

编写程序，从一个`list<int>`拷贝元素到两个`deque`中。值为偶数的所有元素都拷贝到一个`deque`中，而奇数值元素都拷贝到另一个`deque`中。

```c++
#include <iostream>
#include <list>
#include <deque>
#include <sstream>

int main(){
    std::string s;
    std::list<std::string> ls;

    std::deque<std::string> dq1;
    std::deque<std::string> dq2;
    std::deque<std::string> dq3;

    int number;

    while(std::cin >> s){
        ls.push_back(s);
    }
    for(std::list<std::string>::iterator si = ls.begin(); si != ls.end(); ++si){
        std::stringstream ss(*si);
        ss >> number;
        if(number % 2 == 0){
            dq2.push_back(*si);
        } else {
            dq1.push_back(*si);
        }
    }
}
```

## 9.21

如果我们将第308页中使用`insert`返回值将元素添加到`list`中的循环程序改写为将元素插入到`vector`中，分析循环将如何工作。

每次都把新元素插入到vector的首位

## 9.22

假定`iv`是一个`int`的`vector`，下面的程序存在什么错误？你将如何修改？

```c++
vector<int>::iterator iter = iv.begin(),
				mid = iv.begin() + iv.size() / 2;
while (iter != mid)
	if (*iter == some_val)
		iv.insert(iter, 2 * some_val);
```

可改为：

```c++
while (iter != mid)
{
	if (*iter == some_val)
	{
		iter = v.insert(iter, 2 * some_val);
		++iter;
    }
	++iter;
}
```

## 9.23

在本节第一个程序中，若`c.size()` 为1，则`val`、`val2`、`val3`和`val4`的值会是什么？

都是1

## 9.24

编写程序，分别使用`at`、下标运算符、`front` 和 `begin` 提取一个`vector`中的第一个元素。在一个空`vector`上测试你的程序。

```c++
#include <iostream>
#include <vector>

int main(){
    std::vector<int> v1 = {1,2,3};
    std::vector<int> v2;

    std::cout << "v1 at " << v1.at(0) << std::endl;
    std::cout << "v2 at " << v2.at(0) << std::endl;

    std::cout << "v1 front() " << v1.front() << std::endl;
    std::cout << "v2 front() " << v2.front() << std::endl;
}
```

## 9.25

> 对于第312页中删除一个范围内的元素的程序，如果 `elem1` 与 `elem2` 相等会发生什么？如果 `elem2` 是尾后迭代器，或者 `elem1` 和 `elem2` 皆为尾后迭代器，又会发生什么？

- 如果 `elem1` 和 `elem2` 相等，那么不会发生任何操作。
- `如果elem2` 是尾后迭代器，那么删除从 `elem1` 到最后的元素。
- 如果两者皆为尾后迭代器，也什么都不会发生。

## 9.26

使用下面代码定义的`ia`，将`ia`拷贝到一个`vector`和一个`list`中。是用单迭代器版本的`erase`从`list`中删除奇数元素，从`vector`中删除偶数元素。

```c++
int ia[] = { 0, 1, 1, 2, 3, 5, 8, 13, 21, 55, 89 };
```

```c++
#include <iostream>
#include <vector>
#include <list>

int main(){
    int ia[] = { 0, 1, 1, 2, 3, 5, 8, 13, 21, 55, 89 };

    std::vector<int> v(ia, std::end(ia));
    std::list<int> ls(v.begin(), v.end());

    for (auto it = v.begin(); it != v.end(); ){
        if (*it % 2 == 0){
            it = v.erase(it);
        } else {
            ++it;
        }
    }

    for (auto it = ls.begin(); it != ls.end(); ){
        if (*it % 2 != 0){
            it = ls.erase(it);
        } else {
            ++it;
        }
    }

    for (auto v_ele : v){
        std::cout << v_ele << std::endl;
    }
    for (auto ls_ele : ls){
        std::cout << ls_ele << std::endl;
    }

}
```

** 对于 `vector` 来说，当你调用 `erase()` 后，当前迭代器和之后所有的迭代器都会失效，所以你不能在 `erase()` 后再使用原迭代器 **

## 9.27

编写程序，查找并删除`forward_list<int>`中的奇数元素。

```c++
#include <iostream>
#include <forward_list>

int main(){
    std::forward_list<int> fl={1,2,3,4,5,6,7};
    std::forward_list<int>::iterator prev = fl.before_begin();
    std::forward_list<int>::iterator curr = fl.begin();

    while( curr != fl.end()){
        if (*curr % 2 != 0){
            curr = fl.erase_after(prev);
        } else{
            prev = curr;
            ++curr;
        }
    }

    for(auto fl_ele: fl){
        std::cout << fl_ele << std::endl;
    }

}
```

## 9.28

编写函数，接受一个`forward_list<string>`和两个`string`共三个参数。函数应在链表中查找第一个`string`，并将第二个`string`插入到紧接着第一个`string`之后的位置。若第一个`string`未在链表中，则将第二个`string`插入到链表末尾。

```c++
#include <iostream>
#include <forward_list>

void insert(std::forward_list<std::string>& fl, std::string s1, std::string s2){
    std::forward_list<std::string>::iterator curr = fl.begin();
    std::forward_list<std::string>::iterator prev = fl.before_begin();
    while( curr != fl.end()){
        if (*curr == s1){
            fl.insert_after(curr, s2);
            return;
        }
        ++curr;
        ++prev;
    }
    fl.insert_after(prev, s2);
}

int main(){
    std::forward_list<std::string> fl={"this", "is", "a", "test"};

    insert(fl, "a", "easy");

    for(auto fl_ele: fl){
        std::cout << fl_ele << std::endl;
    }
}
```

## 9.29

假定`vec`包含25个元素，那么`vec.resize(100)`会做什么？如果接下来调用`vec.resize(10)`会做什么？

1.会在vec后面填充75个初始值元素

2.会删除末尾90个元素

## 9.30

接收单个参数的resize版本对元素类型有什么限制

元素类型必须提供一个默认构造函数。

## 9.31

第316页中删除偶数值元素并复制奇数值元素的程序不能用于`list`或`forward_list`。为什么？修改程序，使之也能用于这些类型。

```c++
#include <iostream>
#include <forward_list>
#include <list>
#include <vector>

int main(){
    std::list<int> vi = {0,1,2,3,4,5,6,7,8,9};
    auto iter = vi.begin();

    while(iter != vi.end()){
        if (*iter % 2){
            iter = vi.insert(iter, *iter);
            ++iter; ++iter;	// iter += 2; 是错误的
        } else {
            iter = vi.erase(iter);
        }
    }

    for(auto vi_ele: vi){
        std::cout << vi_ele << std::endl;
    }
}
```

`std::list::iterator` 类型不支持 `+=` 操作。`std::list` 是一个双链表，它的迭代器只支持单步的递增和递减（也就是说，只支持 `++` 和 `--` 操作）。因此，`iter += 2;` 这行代码会导致编译错误。

这是因为链表的节点可能会被分散在内存的不同位置，所以我们不能像处理数组那样，通过直接计算偏移来访问元素。每次移动迭代器，我们都需要跟踪链表的下一个节点，这个操作的时间复杂度是 O(1)。但如果我们想要跳过多个节点，就需要重复这个操作多次，时间复杂度变为 O(n)。因此，`std::list` 的迭代器不支持 `+=` 操作。

## 9.32

在第316页的程序中，向下面语句这样调用`insert`是否合法？如果不合法，为什么？

```c++
iter = vi.insert(iter, *iter++);
```

不合法。因为参数的求值顺序是未指定的。这取决于编译器

## 9.33

在本节最后一个例子中，如果不将`insert`的结果赋予`begin`，将会发生什么？编写程序，去掉此赋值语句，验证你的答案。

`begin`将会失效，且逻辑不正确。

## 9.34

假定`vi`是一个保存`int`的容器，其中有偶数值也有奇数值，分析下面循环的行为，然后编写程序验证你的分析是否正确。

```c++
iter = vi.begin();
while (iter != vi.end())
	if (*iter % 2)
		iter = vi.insert(iter, *iter);
	++iter;
```

遇到奇数时，会在该奇数前添加一个该数的副本，迭代器此时会指向新添加的元素，然后迭代器会向后一位，指向原来的奇数。因此，一旦遇到奇数，循环就不会终止。

## 9.35

解释一个`vector`的`capacity`和`size`有何区别。

容器的size是指他已经保存的元素的数量，capacity则是在不分配新的内存空间的前提下，它最多可以保存多少元素。

## 9.36

一个容器的`capacity`可能小于它的`size`吗？

不可能

## 9.37

为什么`list`或`array`没有`capacity`成员函数？

链表结构天生不需要内存连续，也就无所谓capacity。array是固定长度数组，不能改变大小，不需要预留空间。

## 9.38

编写程序，探究在你的标准实现中，`vector`是如何增长的。

```c++
#include <iostream>
#include <vector>

using namespace std;

int main()
{
    vector<int> v;

    for (int i = 0; i < 70; i++)
    {
        cout << "capacity: " << v.capacity() << "  size: " << v.size() << endl;
        v.push_back(i);
    }
    return 0;
}
```

```
capacity: 0  size: 0
capacity: 1  size: 1
capacity: 2  size: 2
capacity: 4  size: 3
capacity: 4  size: 4
capacity: 8  size: 5
capacity: 8  size: 6
capacity: 8  size: 7
capacity: 8  size: 8
capacity: 16  size: 9
capacity: 16  size: 10
capacity: 16  size: 11
capacity: 16  size: 12
capacity: 16  size: 13
capacity: 16  size: 14
capacity: 16  size: 15
capacity: 16  size: 16
capacity: 32  size: 17
capacity: 32  size: 18
capacity: 32  size: 19
capacity: 32  size: 20
capacity: 32  size: 21
capacity: 32  size: 22
capacity: 32  size: 23
capacity: 32  size: 24
capacity: 32  size: 25
capacity: 32  size: 26
capacity: 32  size: 27
capacity: 32  size: 28
capacity: 32  size: 29
capacity: 32  size: 30
capacity: 32  size: 31
capacity: 32  size: 32
capacity: 64  size: 33
capacity: 64  size: 34
capacity: 64  size: 35
capacity: 64  size: 36
capacity: 64  size: 37
capacity: 64  size: 38
capacity: 64  size: 39
capacity: 64  size: 40
capacity: 64  size: 41
capacity: 64  size: 42
capacity: 64  size: 43
capacity: 64  size: 44
capacity: 64  size: 45
capacity: 64  size: 46
capacity: 64  size: 47
capacity: 64  size: 48
capacity: 64  size: 49
capacity: 64  size: 50
capacity: 64  size: 51
capacity: 64  size: 52
capacity: 64  size: 53
capacity: 64  size: 54
capacity: 64  size: 55
capacity: 64  size: 56
capacity: 64  size: 57
capacity: 64  size: 58
capacity: 64  size: 59
capacity: 64  size: 60
capacity: 64  size: 61
capacity: 64  size: 62
capacity: 64  size: 63
capacity: 64  size: 64
capacity: 128  size: 65
capacity: 128  size: 66
capacity: 128  size: 67
capacity: 128  size: 68
capacity: 128  size: 69

```

## 9.39

解释下面程序片段做了什么：

```c++
vector<string> svec;
svec.reserve(1024);
string word;
while (cin >> word)
	svec.push_back(word);
svec.resize(svec.size() + svec.size() / 2);
```

定义一个`vector`，为它分配1024个元素的空间。然后通过一个循环从标准输入中读取字符串并添加到`vector`当中。循环结束后，改变`vector`的容器大小（元素数量）为原来的1.5倍，使用元素的默认初始化值填充。如果容器的大小超过1024，`vector`也会重新分配空间以容纳新增的元素。

## 9.40

如果上一题的程序读入了256个词，在`resize`之后容器的`capacity`可能是多少？如果读入了512个、1000个、或1048个呢？

- 如果读入了256个词，`capacity` 仍然是 1024
- 如果读入了512个词，`capacity` 仍然是 1024
- 如果读入了1000或1048个词，`capacity` 取决于具体实现。

## 9.41

编写程序，从一个`vector<char>`初始化一个`string`。

```c++
vector<char> v{ 'h', 'e', 'l', 'l', 'o' };
string str(v.cbegin(), v.cend());
```

## 9.42

假定你希望每次读取一个字符存入一个`string`中，而且知道最少需要读取100个字符，应该如何提高程序的性能？

```c++
#include <iostream>
#include <string>

int main()
{
    std::string s;
    s.reserve(100); //先声明100个长度的空间
    while(std::cin >> s){
        continue;
    }
    return 0;
}
```

## 9.43

编写一个函数，接受三个`string`参数是`s`、`oldVal` 和`newVal`。使用迭代器及`insert`和`erase`函数将`s`中所有`oldVal`替换为`newVal`。测试你的程序，用它替换通用的简写形式，如，将"tho"替换为"though",将"thru"替换为"through"。

```c++
#include <iterator>
#include <iostream>
#include <string>
#include <cstddef>

void replace_with(std::string &s, std::string const& oldVal, std::string const& newVal)
{
    for (auto cur = s.begin(); cur <= s.end() - oldVal.size(); )
        if (oldVal == std::string{ cur, cur + oldVal.size() })
            cur = s.erase(cur, cur + oldVal.size()),
            cur = s.insert(cur, newVal.begin(), newVal.end()),
            cur += newVal.size();
        else
            ++cur;
}

int main()
{
    std::string s{ "To drive straight thru is a foolish, tho courageous act." };
    replace_with(s, "tho", "though");
    replace_with(s, "thru", "through");
    std::cout << s << std::endl;

    return 0;
}
```

## 9.44

重写上一题的函数，这次使用一个下标和`replace`。

```c++
#include <iostream>
#include <string>

using std::cout; 
using std::endl;
using std::string;

auto replace_with(string &s, string const& oldVal, string const& newVal)
{
    for (size_t pos = 0; pos <= s.size() - oldVal.size();)
        if (s[pos] == oldVal[0] && s.substr(pos, oldVal.size()) == oldVal)
            s.replace(pos, oldVal.size(), newVal),
            pos += newVal.size();
        else
            ++pos;
}

int main()
{
    string str{ "To drive straight thru is a foolish, tho courageous act." };
    replace_with(str, "tho", "though");
    replace_with(str, "thru", "through");
    cout << str << endl;
    return 0;
}
```

## 9.45

编写一个函数，接受一个表示名字的`string`参数和两个分别表示前缀（如"Mr."或"Mrs."）和后缀（如"Jr."或"III"）的字符串。使用迭代器及`insert`和`append`函数将前缀和后缀添加到给定的名字中，将生成的新`string`返回。

```c++
#include <iostream>
#include <string>

void add_pre_and_suffix(std::string& name, std::string const& pre, std::string const& su)
{
    name.insert(name.begin(), pre.cbegin(), pre.cend());
    name.append(su);
}

int main()
{
    std::string name("Wang");
    add_pre_and_suffix(name, "Mr.", ", Jr.");
    std::cout << name << std::endl;
    return 0;
}
```

## 9.46

```c++
#include <iostream>
#include <string>

void add_pre_and_suffix(std::string& name, std::string const& pre, std::string const& su)
{
    name.insert(0, pre);
    name.insert(name.size(), su);
}

int main()
{
    std::string name("Wang");
    add_pre_and_suffix(name, "Mr.", ", Jr.");
    std::cout << name << std::endl;
    return 0;
}
```

## 9.47

编写程序，首先查找`string`"ab2c3d7R4E6"中每个数字字符，然后查找其中每个字母字符。编写两个版本的程序，第一个要使用`find_first_of`，第二个要使用`find_first_not_of`。

```c++
#include <iostream>
#include <string>

using namespace std;

int main()
{
	string numbers("0123456789");
	string s("ab2c3d7R4E6");

	cout << "numeric characters: ";
	for (int pos = 0; (pos = s.find_first_of(numbers, pos)) != string::npos; ++pos)
	{
		cout << s[pos] << " ";
	}

	cout << "\nalphabetic characters: ";
	for (int pos = 0; (pos = s.find_first_not_of(numbers, pos)) != string::npos; ++pos)
	{
		cout << s[pos] << " ";
	}

	return 0;
}
```

## 9.48

假定`name`和`numbers`的定义如325页所示，`numbers.find(name)`返回什么？

返回string::npos;

## 9.49

如果一个字母延伸到中线之上，如d或f，则称其有上出头部分（`ascender`）。如果一个字母延伸到中线之下，如p或g，则称其有下出头部分（`descender`）。编写程序，读入一个单词文件，输出最长的既不包含上出头部分，也不包含下出头部分的单词。

```c++
#include <string>
#include <fstream>
#include <iostream>

using std::string; using std::cout; using std::endl; using std::ifstream;

int main()
{
    ifstream ifs("../data/letter.txt");
    if (!ifs) return -1;

    string longest;
    auto update_with = [&longest](string const& curr)
    {
        if (string::npos == curr.find_first_not_of("aceimnorsuvwxz"))
            longest = longest.size() < curr.size() ? curr : longest;
    };
    for (string curr; ifs >> curr; update_with(curr));
    cout << longest << endl;

    return 0;
}
```

## 9.50

```c++
#include <iostream>
#include <string>
#include <vector>

int sum_for_int(std::vector<std::string> const& v)
{
    int sum = 0;
    for (auto const& s : v)
        sum += std::stoi(s);
    return sum;
}

float sum_for_float(std::vector<std::string> const& v)
{
    float sum = 0.0;
    for (auto const& s : v)
        sum += std::stof(s);
    return sum;
}

int main()
{
    std::vector<std::string> v = { "1", "2", "3", "4.5" };
    std::cout << sum_for_int(v) << std::endl;
    std::cout << sum_for_float(v) << std::endl;

    return 0;
}
```

## 9.51

设计一个类，它有三个`unsigned`成员，分别表示年、月和日。为其编写构造函数，接受一个表示日期的`string`参数。你的构造函数应该能处理不同的数据格式，如January 1,1900、1/1/1990、Jan 1 1900 等。

