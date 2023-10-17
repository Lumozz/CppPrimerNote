# chapter 10

## 10.1

头文件`algorithm`中定义了一个名为`count`的函数，它类似`find`， 接受一对迭代器和一个值作为参数。`count`返回给定值在序列中出现的次数。编写程序，读取`int`序列存入`vector`中，打印有多少个元素的值等于给定值。

```c++
#include <iostream>
#include <algorithm>
#include <vector>

int main()
{
    std::vector<int> v = { 1,2,3,4,4,4,5,6 };
    int number = std::count(v.begin(), v.end(), 4);
    std::cout << number << std::endl;

    return 0;
}
```

## 10.2

重做上一题，但读取 `string` 序列存入 `list` 中.

```c++
#include <iostream>
#include <algorithm>
#include <list>
#include <string>

int main()
{
    std::list<std::string> s = { "1", "2", "3", "3" };
    int number = std::count(s.begin(), s.end(), "3");
    std::cout << number << std::endl;

    return 0;
}
```

## 10.3

用 `accumulate`求一个 `vector<int>` 中元素之和。

```c++
#include <iostream>
#include <algorithm>
#include <numeric>
#include <list>
#include <string>
#include <vector>

int main()
{
    std::vector<int> v = {1,2,3,4,5};
    std::list<std::string> s = { "1", "2", "3", "3" };

    int sum_v = std::accumulate(v.begin(), v.end(), 0);
    std::string sum_s = std::accumulate(s.begin(), s.end(), std::string(""));

    std::cout << sum_v << std::endl;
    std::cout << sum_s << std::endl;

    return 0;
}
```

## 10.4

假定 `v` 是一个`vector<double>`，那么调用 `accumulate(v.cbegin(),v.cend(),0)` 有何错误（如果存在的话）？

accumulate第三个参数的类型决定了返回值的类型，此处第三个值是int，则返回值是int，可能会丢失精度。

## 10.5

在本节对名册（`roster`）调用`equal`的例子中，如果两个名册中保存的都是C风格字符串而不是`string`，会发生什么？

C风格字符串是用指向字符的指针表示的，因此会比较两个指针的值（地址），而不会比较这两个字符串的内容。

## 10.6

编写程序，使用 `fill_n` 将一个序列中的 `int` 值都设置为0。

```c++
#include <iostream>
#include <algorithm>
#include <numeric>
#include <list>
#include <string>
#include <vector>

int main()
{
    std::vector<int> v = {1,2,3,4,5};


    std::fill_n(v.begin(), v.size(), 0);

    for(auto ele: v){
        std::cout << ele << std::endl;
    }

    return 0;
}
```

## 10.7

下面程序是否有错误？如果有，请改正：

```c++
(a) vector<int> vec; list<int> lst; int i;
	while (cin >> i)
		lst.push_back(i);
	copy(lst.cbegin(), lst.cend(), vec.begin());
(b) vector<int> vec;
	vec.reserve(10);
	fill_n(vec.begin(), 10, 0);
```

- (a) 应该加一条语句 `vec.resize(lst.size())` 。`copy`时必须保证目标目的序列至少要包含与输入序列一样多的元素。
- (b) reserve分配了足够的空间，但是不会创建新元素。算法可能改变容器中保存的元素的值，也可能在容器内移动元素，永远不会直接添加和删除元素(c++ priemr中文版第五版 P338)，所以此处应该改为resize(10)。

## 10.8

本节提到过，标准库算法不会改变它们所操作的容器的大小。为什么使用 `back_inserter`不会使这一断言失效？

`back_inserter` 是插入迭代器，在 `iterator.h` 头文件中，不是标准库的算法。

## 10.9

实现你自己的`elimDups`。分别在读取输入后、调用`unique`后以及调用`erase`后打印`vector`的内容。

```c++
#include <iostream>
#include <algorithm>
#include <vector>

int elimDups(std::vector<int>& v){
    std::sort(v.begin(), v.end());
    std::vector<int>::iterator end_unique = std::unique(v.begin(), v.end());
    v.erase(end_unique, v.end());
}

int main()
{
    std::vector<int> v = {1,2,2,3,4,5,5};
    elimDups(v);
    for(auto e: v){
        std::cout << e <<std::endl;
    }
    return 0;
}
```

## 10.10

你认为算法不改变容器大小的原因是什么？

算法操作的是迭代器

## 10.11

编写程序，使用 `stable_sort` 和 `isShorter` 将传递给你的 `elimDups` 版本的 `vector` 排序。打印 `vector`的内容，验证你的程序的正确性。

```c++
#include <iostream>
#include <algorithm>
#include <string>
#include <vector>

bool isShorter(const std::string& s1, const std::string& s2){
    return s1.size() < s2.size();
}

int elimDups(std::vector<std::string>& v){
    std::stable_sort(v.begin(), v.end(), isShorter);
    auto end_unique = std::unique(v.begin(), v.end());
    v.erase(end_unique, v.end());
}

int main()
{
    std::vector<std::string> v = {"a", "bcd", "ef", "ef"};
    elimDups(v);
    for(auto e: v){
        std::cout << e <<std::endl;
    }
    return 0;
}
```

## 10.12

编写名为 `compareIsbn` 的函数，比较两个 `Sales_data` 对象的`isbn()` 成员。使用这个函数排序一个保存 `Sales_data` 对象的 `vector`。



## 10.13

标准库定义了名为 `partition` 的算法，它接受一个谓词，对容器内容进行划分，使得谓词为`true` 的值会排在容器的前半部分，而使得谓词为 `false` 的值会排在后半部分。算法返回一个迭代器，指向最后一个使谓词为 `true` 的元素之后的位置。编写函数，接受一个 `string`，返回一个 `bool` 值，指出 `string` 是否有5个或更多字符。使用此函数划分 `words`。打印出长度大于等于5的元素

```c++
#include <iostream>
#include <algorithm>
#include <numeric>
#include <list>
#include <string>
#include <vector>

bool largerThen5(const std::string& s1){
    return s1.size()>=5;
}

int main()
{
    std::vector<std::string> s = {"a", "bcd", "efasdf", "effaadasdf"};
    auto s_end = std::partition(s.begin(), s.end(), largerThen5);

    std::cout << "larger then 5: " << std::endl;

    for(auto it = s.begin(); it != s_end; ++it){
        std::cout << *it <<std::endl;
    }
    return 0;
```

## 10.14

编写一个`lambda`，接受两个`int`，返回它们的和。

```c++
#include <iostream>
#include <algorithm>

int main()
{
    auto f = [] (int a, int b) {return a+b;};
    std::cout << f(3, 4) << std::endl;
    return 0;
}
```

## 10.15

编写一个 `lambda` ，捕获它所在函数的 `int`，并接受一个 `int`参数。`lambda` 应该返回捕获的 `int` 和 `int` 参数的和。

```c++
#include <iostream>
#include <algorithm>

int main()
{
    int i = 9;
    auto f = [i] (int a) {return a+i;};
    std::cout << f(1) << std::endl;
    return 0;
}
```

## 10.16

使用 `lambda` 编写你自己版本的 `biggies`。

```c++
#include <iostream>
#include <algorithm>
#include <vector>
#include <string>

void elimdups(std::vector<std::string> &vs)
{
    std::sort(vs.begin(), vs.end());
    auto new_end = std::unique(vs.begin(), vs.end());
    vs.erase(new_end, vs.end());
}

void biggies(std::vector<std::string>& word, std::vector<std::string>::size_type sz){
    elimdups(word);
    stable_sort(word.begin(), word.end(), [] (const std::string& s1, const std::string& s2) { return s1.size() < s2.size();});
    auto wc = find_if(word.begin(), word.end(), [sz](const std::string &a){return a.size() >= sz;});
    auto count = word.end() - wc;

    for_each(wc, word.end(), [](const std::string &s){std::cout << s << " ";});

}

int main()
{
    std::vector<std::string> vs = {"1234", "12345", "123", "12345678", "123456789"};
    biggies(vs, 5);
    return 0;
}
```

## 10.17

重写10.3.1节练习10.12的程序，在对`sort`的调用中使用 `lambda` 来代替函数 `compareIsbn`。



## 10.18

> 重写 `biggies`，用 `partition` 代替 `find_if`。我们在10.3.1节练习10.13中介绍了 `partition` 算法。

```c++
#include <iostream>
#include <algorithm>
#include <vector>
#include <string>

void elimdups(std::vector<std::string> &vs)
{
    std::sort(vs.begin(), vs.end());
    auto new_end = std::unique(vs.begin(), vs.end());
    vs.erase(new_end, vs.end());
}

void biggies(std::vector<std::string>& word, std::vector<std::string>::size_type sz){
    elimdups(word);
    stable_sort(word.begin(), word.end(), [] (const std::string& s1, const std::string& s2) { return s1.size() < s2.size();});
    auto s_end = std::partition(word.begin(), word.end(), [sz](const std::string& s){return s.size() >= sz;});

    for_each(word.begin(), s_end, [](const std::string &s){std::cout << s << " ";});

}

int main()
{
    std::vector<std::string> vs = {"1234", "12345", "123", "12345678", "123456789"};
    biggies(vs, 5);
    return 0;
}
```

## 10.19

用 `stable_partition` 重写前一题的程序，与 `stable_sort` 类似，在划分后的序列中维持原有元素的顺序。

```c++
#include <iostream>
#include <algorithm>
#include <vector>
#include <string>

void elimdups(std::vector<std::string> &vs)
{
    std::sort(vs.begin(), vs.end());
    auto new_end = std::unique(vs.begin(), vs.end());
    vs.erase(new_end, vs.end());
}

void biggies(std::vector<std::string>& word, std::vector<std::string>::size_type sz){
    elimdups(word);
    stable_sort(word.begin(), word.end(), [] (const std::string& s1, const std::string& s2) { return s1.size() < s2.size();});
    auto s_end = std::stable_partition(word.begin(), word.end(), [sz](const std::string& s){return s.size() >= sz;});

    for_each(word.begin(), s_end, [](const std::string &s){std::cout << s << " ";});

}

int main()
{
    std::vector<std::string> vs = {"1234", "12345", "123", "12345678", "123456789"};
    biggies(vs, 5);
    return 0;
}
```

## 10.20

标准库定义了一个名为 `count_if` 的算法。类似 `find_if`，此函数接受一对迭代器，表示一个输入范围，还接受一个谓词，会对输入范围中每个元素执行。`count_if`返回一个计数值，表示谓词有多少次为真。使用`count_if`重写我们程序中统计有多少单词长度超过6的部分。

```c++
int main()
{
    std::vector<std::string> vs = {"1234", "12345", "123", "12345678", "123456789"};
    int number = std::count_if(vs.begin(), vs.end(), [](const std::string s) -> bool {if (s.size() > 6){return true;} else{return false;}});
    std::cout << number << std::endl;
    return 0;
}
```

## 10.21

编写一个 `lambda`，捕获一个局部 `int` 变量，并递减变量值，直至它变为0。一旦变量变为0，再调用`lambda`应该不再递减变量。`lambda`应该返回一个`bool`值，指出捕获的变量是否为0。

```c++
int main()
{
    int i = 10;
    auto f = [&i]() -> bool {return(i==0 ? true: !(i--));};
    while (!f()) std::cout << i << std::endl;
    return 0;
}
```

## 10.22

重写统计长度小于等于6的单词数量的程序，使用函数代替`lambda`。

```c++
#include <iostream>
#include <algorithm>
#include <functional>
#include <vector>
#include <string>

bool isLarger(const std::string& s, std::string::size_type sz){
    return s.size() <= sz;
}

int main(){
    std::vector<std::string> s={"123","1234","123456","123456","1234567","12345678"};
    std::string::size_type sz = 6;

    auto f = std::bind(isLarger, std::placeholders::_1, sz);
    long number = std::count_if(s.begin(), s.end(),f);
    std::cout << number << std::endl;
}
```

## 10.23

`bind` 接受几个参数？

假设被绑定的函数接受 `n` 个参数，那么`bind` 接受`n + 1`个参数。

## 10.24

给定一个`string`，使用 `bind` 和 `check_size` 在一个 `int` 的`vector` 中查找第一个大于`string`长度的值。

```c++
#include <iostream>
#include <algorithm>
#include <functional>
#include <vector>
#include <string>

bool isLarger(const int& i, int sz){
    return i == sz;
}

int main(){
    std::vector<int> v = {1,2,3,4,5,6,7,8,9};
    std::string s = "123";

    auto f = std::bind(isLarger, std::placeholders::_1, s.size());
    auto it = std::find_if(v.begin(), v.end(),f);
    std::cout << *it << std::endl;
}
```

## 10.25

在10.3.2节的练习中，编写了一个使用`partition` 的`biggies`版本。使用 `check_size` 和 `bind` 重写此函数。



```c++
#include <iostream>
#include <algorithm>
#include <functional>
#include <vector>
#include <string>

void elimdups(std::vector<std::string> &vs)
{
    std::sort(vs.begin(), vs.end());
    auto new_end = std::unique(vs.begin(), vs.end());
    vs.erase(new_end, vs.end());
}

bool check_size(const std::string &s, std::string::size_type sz)
{
    return s.size() >= sz;
}


void biggies(std::vector<std::string>& word, std::vector<std::string>::size_type sz){
    elimdups(word);
    stable_sort(word.begin(), word.end(), [] (const std::string& s1, const std::string& s2) { return s1.size() < s2.size();});
    auto s_end = std::partition(word.begin(), word.end(), bind(check_size, std::placeholders::_1, sz));

    for_each(word.begin(), s_end, [](const std::string &s){std::cout << s << " ";});

}

int main()
{
    std::vector<std::string> vs = {"1234", "12345", "123", "12345678", "123456789"};
    biggies(vs, 5);
    return 0;
}
```

## 10.26

解释三种插入迭代器的不同之处。

- `back_inserter` 使用 `push_back` 。
- `front_inserter` 使用 `push_front` 。
- `inserter` 使用 `insert`，此函数接受第二个参数，这个参数必须是一个指向给定容器的迭代器。元素将被插入到给定迭代器所表示的元素之前。

## 10.27

除了 `unique` 之外，标准库还定义了名为 `unique_copy` 的函数，它接受第三个迭代器，表示拷贝不重复元素的目的位置。编写一个程序，使用 `unique_copy`将一个`vector`中不重复的元素拷贝到一个初始化为空的`list`中。

```c++
#include <iostream>
#include <algorithm>
#include <vector>
#include <list>
#include <iterator>

int main()
{
    std::vector<int> vec{ 1, 1, 3, 3, 5, 5, 7, 7, 9 };
    std::list<int> lst;
    
    std::unique_copy(vec.begin(), vec.end(), back_inserter(lst));
    for (auto i : lst)
        std::cout << i << " ";
    std::cout << std::endl;
}
```

## 10.28

一个`vector` 中保存 1 到 9，将其拷贝到三个其他容器中。分别使用`inserter`、`back_inserter` 和 `front_inserter` 将元素添加到三个容器中。对每种 `inserter`，估计输出序列是怎样的，运行程序验证你的估计是否正确

```c++
#include <iostream>
#include <algorithm>
#include <vector>
#include <list>
#include <iterator>

int main()
{
    std::vector<int> vec{ 1, 1, 3, 3, 5, 5, 7, 7, 9 };
    std::vector<int> v1;
    std::list<int> v2;
    std::vector<int> v3;

    auto bi = std::back_inserter(v1);
    auto fi = std::front_inserter(v2);
    auto ii = std::inserter(v3, v3.begin());

    for (auto i : vec){
        *bi = i;
        *fi = i;
        *ii = i;
    }

    for(auto i: v1){
        std::cout << i;
    }
    std::cout << std::endl;

    for(auto i: v2){
        std::cout << i;
    }
    std::cout << std::endl;

    for(auto i: v3){
        std::cout << i;
    }
    std::cout << std::endl;
}
```

## 10.29

编写程序，使用流迭代器读取一个文本文件，存入一个`vector`中的`string`里。

```c++
#include <iostream>
#include <fstream>
#include <vector>
#include <string>
#include <iterator>

using std::string;

int main()
{
    std::ifstream ifs("./file");
    std::cout << ifs.rdstate() << std::endl;
    std::istream_iterator<string> in(ifs), eof;
    std::vector<string> vec;
    std::copy(in, eof, back_inserter(vec));

    // output
    std::copy(vec.cbegin(), vec.cend(), std::ostream_iterator<string>(std::cout, "\n"));
}
```

## 10.30

使用流迭代器、`sort` 和 `copy` 从标准输入读取一个整数序列，将其排序，并将结果写到标准输出

```c++
#include <iostream>
#include <vector>
#include <algorithm>
#include <iterator>

using namespace std;

int main()
{
	vector<int> v;
	istream_iterator<int> int_it(cin), int_eof;

	copy(int_it, int_eof, back_inserter(v));
	sort(v.begin(), v.end());

	copy(v.begin(), v.end(), ostream_iterator<int>(cout," "));
	cout << endl;
	return 0;
}
```

## 10.31

> 修改前一题的程序，使其只打印不重复的元素。你的程序应该使用 `unique_copy`。

```c++
#include <iostream>
#include <vector>
#include <algorithm>
#include <iterator>

using namespace std;

int main()
{
	vector<int> v;
	istream_iterator<int> int_it(cin), int_eof;
	
	copy(int_it, int_eof, back_inserter(v));
	sort(v.begin(), v.end());
	unique(v.begin(), v.end());
	copy(v.begin(), v.end(), ostream_iterator<int>(cout, " "));
	cout << endl;
	return 0;
}
```

## 10.32

重写1.6节中的书店程序，使用一个`vector`保存交易记录，使用不同算法完成处理。使用 `sort` 和10.3.1节中的 `compareIsbn` 函数来排序交易记录，然后使用 `find` 和 `accumulate` 求和。



## 10.33

编写程序，接受三个参数：一个输入文件和两个输出文件的文件名。输入文件保存的应该是整数。使用 `istream_iterator` 读取输入文件。使用 `ostream_iterator` 将奇数写入第一个输入文件，每个值后面都跟一个空格。将偶数写入第二个输出文件，每个值都独占一行。

```c++
#include <fstream>
#include <iterator>
#include <algorithm>

int main(int argc, char **argv)
{
    if (argc != 4) return -1;

    std::ifstream ifs(argv[1]);
    std::ofstream ofs_odd(argv[2]), ofs_even(argv[3]);

    std::istream_iterator<int> in(ifs), in_eof;
    std::ostream_iterator<int> out_odd(ofs_odd, " "), out_even(ofs_even, "\n");

    std::for_each(in, in_eof, [&out_odd, &out_even](const int i)
    {
        *(i & 0x1 ? out_odd : out_even)++ = i;
    });

    return 0;
}
```
