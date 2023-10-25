# chapter 11

## 11.1

描述`map`和`vector`的不同。

GPT4:

`std::vector` 和 `std::map` 是 C++ 标准库中的两种不同类型的容器，它们在用途和性能特性方面都有所不同。

1. **元素存储和访问**
   - `std::vector` 是一个动态数组，它在内存中连续存储元素。这意味着你可以通过索引快速访问元素（O(1) 时间复杂度）。
   - `std::map` 是一个有序的关键字-值对集合，它通常使用红黑树实现。元素不是连续存储的，而是根据关键字的排序存储的。因此，你不能通过索引访问元素，而必须通过关键字来访问元素（O(log n) 时间复杂度）。
2. **插入和删除**
   - `std::vector` 的元素插入和删除操作在某些情况下可能需要重新分配和复制整个数组，尤其是在数组的中间插入或删除元素时（O(n) 时间复杂度）。
   - `std::map` 的元素插入和删除操作的时间复杂度通常为 O(log n)，因为它们不需要移动除了插入或删除点附近的元素之外的其他元素。
3. **用途**
   - `std::vector` 适合存储需要连续内存空间或者需要通过索引访问的元素。
   - `std::map` 适合存储需要根据特定关键字快速查找的元素，或者需要保持元素的有序性。

## 11.2

分别给出最适合使用`list`、`vector`、`deque`、`map`以及`set`的例子。

GPT4:

`std::list` 是一个双向链表，适用于需要频繁在中间位置进行插入和删除操作的场景。

`std::vector` 是一个动态数组，适用于需要频繁访问元素或在尾部插入和删除元素的场景。

`std::deque` 是一个双端队列，适用于需要在头部和尾部都能快速插入和删除元素的场景。

`std::map` 是一个有序键值对集合，适用于需要根据特定的键快速查找值的场景。

`std::set` 是一个有序集合，适用于需要快速检查元素是否存在或需要排除重复元素的场景。



## 11.3

编写你自己的单词计数程序。

```c++
#include <iostream>
#include <map>
#include <string>
#include <algorithm>

int main(int argc, char **argv)
{
    std::string s;
    std::map<std::string, int> m;
    while(std::cin >> s){
        ++m[s];
    }

    std::for_each(m.begin(), m.end(), [](const std::pair<std::string, int>& p) {std::cout << "Key: " << p.first << ", Value: " << p.second << std::endl;});
    return 0;
}
```

## 11.4

扩展你的程序，忽略大小写和标点。例如，"example."、"example,"和"Example"应该递增相同的计数器

```c++
#include <iostream>
#include <map>
#include <string>
#include <algorithm>
#include <cctype>

int main(int argc, char **argv)
{
    std::string s;
    std::map<std::string, int> m;

    while(std::cin >> s){

        // 转换为小写
        std::transform(s.begin(), s.end(), s.begin(), ::tolower);

        // 移除标点
        s.erase(std::remove_if(s.begin(), s.end(), ::ispunct), s.end());

        ++m[s];
    }

    std::for_each(m.begin(), m.end(), [](const std::pair<std::string, int>& p) {std::cout << "Key: " << p.first << ", Value: " << p.second << std::endl;});
    return 0;
}
```

## 11.5

解释`map`和`set`的区别。你如何选择使用哪个？

`map` 是键值对，而 `set` 只有键没有值。当我需要存储键值对的时候使用 `map`，而只需要键的时候使用 `set`。

## 11.6

解释`set`和`list`的区别。你如何选择使用哪个？

`set` 是有序不重复集合，底层实现是红黑树，而 `list` 是无序可重复集合，底层实现是链表。

## 11.7

定义一个`map`，关键字是家庭的姓，值是一个`vector`，保存家中孩子（们）的名。编写代码，实现添加新的家庭以及向已有家庭中添加新的孩子。

```c++
#include <iostream>
#include <map>
#include <vector>
#include <string>

int main() {
    std::map<std::string, std::vector<std::string>> families;

    // 添加新的家庭
    families["Smith"] = {"John", "James"};
    families["Johnson"] = {"Lisa"};

    // 向已有家庭中添加新的孩子
    families["Smith"].push_back("Anna");
    families["Johnson"].push_back("Tom");

    // 打印所有的家庭及其孩子
    for (const auto &family : families) {
        std::cout << "Family " << family.first << " has children: ";
        for (const auto &child : family.second) {
            std::cout << child << " ";
        }
        std::cout << std::endl;
    }

    return 0;
}
```

## 11.8

编写一个程序，在一个`vector`而不是一个`set`中保存不重复的单词。使用`set`的优点是什么？

```c++
#include <iostream>
#include <vector>
#include <algorithm>
#include <iterator>

int main() {
    std::vector<std::string> words;
    std::string word;

    while (std::cin >> word) {
        if (std::find(words.begin(), words.end(), word) == words.end()) {
            // 单词不在 vector 中，添加它
            words.push_back(word);
        }
    }

    // 打印所有单词
    for (const auto &w : words) {
        std::cout << w << ' ';
    }

    return 0;
}
```

## 11.9

定义一个`map`，将单词与一个行号的`list`关联，`list`中保存的是单词所出现的行号。

```c++
#include <iostream>
#include <map>
#include <list>
#include <string>

int main() {
    std::map<std::string, std::list<int> > word_list = {{"a", {1,2,3}},
                                                        {"b", {5,6,7}},
                                                        {"f", {1,3,5}}};;

    for(const auto &ele: word_list){
        std::cout << "word: " << ele.first << " page: ";
        for(const auto &e: ele.second){
            std::cout << e << " ";
        }
        std::cout <<  std::endl;
    }

    return 0;
}
```

## 11.10

可以定义一个`vector<int>::iterator` 到 `int` 的`map`吗？`list<int>::iterator` 到 `int` 的`map`呢？对于两种情况，如果不能，解释为什么。

可以定义 `vector<int>::iterator` 到 `int` 的`map`，但是不能定义 `list<int>::iterator` 到 `int` 的`map`。因为`map`的键必须实现 `<` 操作，`list` 的迭代器不支持比较运算。

## 11.11

不使用`decltype` 重新定义 `bookstore`

GPT4

```
bool compareIsbn(const Sales_data& a, const Sales_data& b);
```

你可以这样定义 `bookstore`：

cpp

Copy

```
bool (*func_ptr)(const Sales_data&, const Sales_data&) = compareIsbn;
std::multiset<Sales_data, bool(*)(const Sales_data&, const Sales_data&)> bookstore(func_ptr);
```

在这个例子中，`bool(*)(const Sales_data&, const Sales_data&)` 是一个函数指针类型，它表示一个指向接受两个 `Sales_data` 参数并返回 `bool` 的函数的指针。然后我们创建了一个此类型的函数指针 `func_ptr`，并将 `compareIsbn` 函数赋值给它。最后，我们创建了一个 `multiset`，并将 `func_ptr` 作为比较函数传递给它。

这种方法和使用 `decltype` 的结果是一样的，但是直接定义函数指针类型可能会更加复杂，尤其是在处理返回类型或参数类型复杂的函数时。总的来说，`decltype` 提供了一种更简洁、更易于理解的方式来获取函数的类型。



##  11.12

编写程序，读入`string`和`int`的序列，将每个`string`和`int`存入一个`pair` 中，`pair`保存在一个`vector`中

```c++
#include <iostream>
#include <vector>
#include <string>
#include <utility>

int main() {
    std::vector<std::pair<std::string, int> > p;
    std::string s;
    int i;
    while(std::cin >> s >> i){
        p.push_back(std::pair<std::string, int> {s, i});
    }

    for (const auto &e : p)
        std::cout << e.first << ":" << e.second << std::endl;

}
```

## 11.13

在上一题的程序中，至少有三种创建`pair`的方法。编写此程序的三个版本，分别采用不同的方法创建`pair`。解释你认为哪种形式最易于编写和理解，为什么？



```c++
vec.push_back(std::make_pair(str, i));
vec.push_back({ str, i });
vec.push_back(std::pair<string, int>(str, i)); 
```



## 11.14

扩展你在11.2.1节练习中编写的孩子姓达到名的`map`，添加一个`pair`的`vector`，保存孩子的名和生日。



## 11.15

