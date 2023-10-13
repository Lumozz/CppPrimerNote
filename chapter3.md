# chapter3

## 3.1

用恰当的using 声明重做 1.4.1节和2.6.2节的练习。

解：

1.4.1

```c++
#include <iostream>

using std::cin;
using std::cout;
using std::endl;

int main()
{
	int sum = 0;
	for (int val = 1; val <= 10; ++val) sum += val;

	cout << "Sum of 1 to 10 inclusive is " << sum << endl;
	
	return 0;
}
```

## 3.2

编写一段程序从标准输入中一次读入一行，然后修改该程序使其一次读入一个词。

```c++
#include<string>
#include<iostream>

int main(){
    std::string line;
    while(std::getline(std::cin, line)){
        std::cout << line << std::endl;
    }

}
```

```c++
#include<string>
#include<iostream>

int main(){
    std::string word;
    while(std::cin >> word){
        std::cout << word << std::endl;
    }

}
```

## 3.3

请说明string类的输入运算符和getline函数分别是如何处理空白字符的。

解：

- 类似`is >> s`的读取：string对象会忽略开头的空白并从第一个真正的字符开始，直到遇见下一**空白**为止。
- 类似`getline(is, s)`的读取：string对象会从输入流中读取字符，直到遇见**换行符**为止。

## 3.4

编写一段程序读取两个字符串，比较其是否相等并输出结果。如果不相等，输出比较大的那个字符串。改写上述程序，比较输入的两个字符串是否等长，如果不等长，输出长度较大的那个字符串。

```c++
#include<string>
#include<iostream>

int main(){
    std::string s1;
    std::string s2;

    while(std::cin >> s1 >> s2) {

        if (s1 == s2) {
            std::cout << "Equal" << std::endl;
        } else {
            s1 = s1 > s2 ? s1 : s2;
            std::cout << s1 << std::endl;
        }
    }
}
```

## 3.5

编写一段程序从标准输入中读入多个字符串并将他们连接起来，输出连接成的大字符串。然后修改上述程序，用空格把输入的多个字符串分割开来。

```c++
#include<string>
#include<iostream>

int main(){
    std::string s1;
    std::string st;

    while(std::cin >> s1) {
        st += s1;
        std::cout << st << std::endl;
    }
}
```

```c++
#include<string>
#include<iostream>

int main(){
    std::string s1;
    std::string st;

    while(std::cin >> s1) {
        st += ' ' + s1;
        std::cout << st << std::endl;
    }
}
```

## 3.6

编写一段程序，使用范围for语句将字符串内所有字符用X代替。

```c++
#include<string>
#include<iostream>

int main(){
    std::string s1;
    std::cin >> s1;

    for (char &s : s1){
        s = 'X';
    }
    std::cout << s1 << std::endl;
}
```

## 3.7

就上一题完成的程序而言，如果将循环控制的变量设置为char将发生什么？先估计一下结果，然后实际编程进行验证。

只用使用char& 才会改变原来字符串中的值，使用char不会改变原字符串的值；

## 3.8

分别用while循环和传统for循环重写第一题的程序，你觉得哪种形式更好呢？为什么？

```c++
#include<string>
#include<iostream>

int main(){
    std::string s1;
    std::cin >> s1;

    std::string::size_type n = s1.size();
    decltype(n) i = 0;

    while(i < n){
        s1[i] = 'X';
        ++i;
    }
    std::cout << s1 << std::endl;
}
```

```c++
#include<string>
#include<iostream>

int main(){
    std::string s1;
    std::cin >> s1;

    std::string::size_type n = s1.size();
    decltype(n) i = 0;

    for( ; i < n; ++i){
        s1[i] = 'X';
    }
    std::cout << s1 << std::endl;
}
```

显然，范围for更加简洁明了

## 3.9

下面的程序有何作用？它合法吗？如果不合法？为什么？

```c++
string s;
cout << s[0] << endl;
```

不合法，不能使用下标访问空string

## 3.10

编写一段程序，读入一个包含标点符号的字符串，将标点符号去除后输出字符串剩余的部分。

```c++
#include<string>
#include<iostream>

int main(){
    std::string s1;
    std::string s2;
    std::cin >> s1;


    for (char s : s1){
        if(!std::ispunct(s)) s2 += s;
    }
    std::cout << s2 << std::endl;
}
```

## 3.11

下面的范围for语句合法吗？如果合法，c的类型是什么？

```c++
const string s = "Keep out!";
for(auto &c : s){ /* ... */ }
```

如果不改变c的值，就合法，不过这种写法不好。

## 3.12

下列vector对象的定义有不正确的吗？如果有，请指出来。对于正确的，描述其执行结果；对于不正确的，说明其错误的原因。

```c++
vector<vector<int>> ivec;         // 在C++11当中合法
vector<string> svec = ivec;       // 不合法，类型不一样
vector<string> svec(10, "null");  // 合法
```

## 3.13

下列的vector对象各包含多少个元素？这些元素的值分别是多少？

```c++
vector<int> v1;         // size:0,  no values.
vector<int> v2(10);     // size:10, value:0
vector<int> v3(10, 42); // size:10, value:42
vector<int> v4{ 10 };     // size:1,  value:10
vector<int> v5{ 10, 42 }; // size:2,  value:10, 42
vector<string> v6{ 10 };  // size:10, value:""
vector<string> v7{ 10, "hi" };  // size:10, value:"hi"
```

## 3.14

```c++
#include <iostream>
#include <string>
#include <cctype>
#include <vector>

using std::cin;
using std::cout;
using std::endl;
using std::vector;

int main()
{
	vector<int> v;
	int i;
	while (cin >> i)
	{
		v.push_back(i);
	}
	return 0;
}
```

## 3.15

改写上题程序，不过这次读入的是字符串。

解：

```c++
#include <iostream>
#include <string>
#include <cctype>
#include <vector>

using std::cin;
using std::cout;
using std::endl;
using std::vector;
using std::string;

int main()
{
	vector<string> v;
	string i;
	while (cin >> i)
	{
		v.push_back(i);
	}
	return 0;
}
```

## 3.16

编写一段程序，把练习3.13中vector对象的容量和具体内容输出出来

```c++
#include <iostream>
#include <string>
#include <cctype>
#include <vector>

using std::cin;
using std::cout;
using std::endl;
using std::vector;
using std::string;

int main()
{
    vector<int> v1;         // size:0,  no values.
    vector<int> v2(10);     // size:10, value:0
    vector<int> v3(10, 42); // size:10, value:42
    vector<int> v4{ 10 };     // size:1,  value:10
    vector<int> v5{ 10, 42 }; // size:2,  value:10, 42
    vector<string> v6{ 10 };  // size:10, value:""
    vector<string> v7{ 10, "hi" };  // size:10, value:"hi"

    cout << "v1 size :" << v1.size() << endl;
    cout << "v2 size :" << v2.size() << endl;
    cout << "v3 size :" << v3.size() << endl;
    cout << "v4 size :" << v4.size() << endl;
    cout << "v5 size :" << v5.size() << endl;
    cout << "v6 size :" << v6.size() << endl;
    cout << "v7 size :" << v7.size() << endl;

    cout << "v1 content: ";
    for (auto i : v1)
    {
        cout << i << " , ";
    }
    cout << endl;

    cout << "v2 content: ";
    for (auto i : v2)
    {
        cout << i << " , ";
    }
    cout << endl;

    cout << "v3 content: ";
    for (auto i : v3)
    {
        cout << i << " , ";
    }
    cout << endl;

    cout << "v4 content: ";
    for (auto i : v4)
    {
        cout << i << " , ";
    }
    cout << endl;

    cout << "v5 content: ";
    for (auto i : v5)
    {
        cout << i << " , ";
    }
    cout << endl;

    cout << "v6 content: ";
    for (auto i : v6)
    {
        cout << i << " , ";
    }
    cout << endl;

    cout << "v7 content: ";
    for (auto i : v7)
    {
        cout << i << " , ";
    }
    cout << endl;
    return 0;
}
```

## 3.17

从cin读入一组词并把它们存入一个vector对象，然后设法把所有词都改为大写形式。输出改变后的结果，每个词占一行。

```c++
#include <iostream>
#include <string>
#include <cctype>
#include <vector>

using std::cin;
using std::cout;
using std::endl;
using std::vector;
using std::string;

int main()
{
	vector<string> v;
	string s;

	while (cin >> s)
	{
		v.push_back(s);
	}

	for (auto &str : v)
	{
		for (auto &c : str)
		{
			c = toupper(c);
		}
	}

	for (auto i : v)
	{
		cout << i << endl;
	}
	return 0;
}
```

## 3.18

下面的程序合法吗？如果不合法，你准备如何修改？

```c++
vector<int> ivec;
ivec[0] = 42;
```

不合法，不能访问空vector

改为：

```c++
ivec.push_back(42);
```

## 3.19

如果想定义一个含有10个元素的vector对象，所有元素的值都是42，请例举三种不同的实现方法，哪种方式更好呢？

如下三种：

```c++
vector<int> ivec1(10, 42);
vector<int> ivec2{ 42, 42, 42, 42, 42, 42, 42, 42, 42, 42 };
vector<int> ivec3;
for (int i = 0; i < 10; ++i)
	ivec3.push_back(42);
```

## 3.20

读入一组整数并把他们存入一个vector对象，将每对相邻整数的和输出出来。改写你的程序，这次要求先输出第一个和最后一个元素的和，接着输出第二个和倒数第二个元素的和，以此类推。

解：

```c++
#include <iostream>
#include <string>
#include <cctype>
#include <vector>

using std::cin;
using std::cout;
using std::endl;
using std::vector;
using std::string;

int main()
{
	vector<int> ivec;
	int i;
	while (cin >> i)
	{
		ivec.push_back(i);
	}

	for (int i = 0; i < ivec.size() - 1; ++i)
	{
		cout << ivec[i] + ivec[i + 1] << endl;
	}
	
	//---------------------------------
	cout << "---------------------------------" << endl;
	
	int m = 0;
	int n = ivec.size() - 1;
	while (m < n)
	{
		cout << ivec[m] + ivec[n] << endl;
		++m;
		--n;
	}
	return 0;
}
```

## 3.21

```c++
#include <vector>
#include <iterator>
#include <string>
#include <iostream>

using std::vector;
using std::string;
using std::cout;
using std::endl;

void check_and_print(const vector<int>& vec)
{
	cout << "size: " << vec.size() << "  content: [";
	for (auto it = vec.begin(); it != vec.end(); ++it)
		cout << *it << (it != vec.end() - 1 ? "," : "");
	cout << "]\n" << endl;
}

void check_and_print(const vector<string>& vec)
{

	cout << "size: " << vec.size() << "  content: [";
	for (auto it = vec.begin(); it != vec.end(); ++it)
		cout << *it << (it != vec.end() - 1 ? "," : "");
	cout << "]\n" << endl;
}

int main()
{
	vector<int> v1;
	vector<int> v2(10);
	vector<int> v3(10, 42);
	vector<int> v4{ 10 };
	vector<int> v5{ 10, 42 };
	vector<string> v6{ 10 };
	vector<string> v7{ 10, "hi" };

	check_and_print(v1);
	check_and_print(v2);
	check_and_print(v3);
	check_and_print(v4);
	check_and_print(v5);
	check_and_print(v6);
	check_and_print(v7);

	return 0;
}
```

## 3.22

修改之前那个输出text第一段的程序，首先把text的第一段全部改成大写形式，然后输出它。

解： 略

## 3.23

```c++
#include <iostream>
#include <vector>

int main(){
    std::vector<int> v(10);
    int ele = 0;
    for(auto i = v.begin(); i != v.end(); ++i){
        *i = ele;
        ++ele;
    }
    for(int e : v){
        std::cout << e << ' ';
    }
    std::cout << std::endl;
    for(auto i = v.begin(); i != v.end(); ++i){
        *i *= 2;
    }
    for(int e : v){
        std::cout << e << ' ';
    }
}
```

## 3.24

请使用迭代器重做3.3.3节的最后一个练习。

解：

```c++
#include <iostream>
#include <string>
#include <cctype>
#include <vector>

using std::cin;
using std::cout;
using std::endl;
using std::vector;
using std::string;

int main()
{
	vector<int> ivec;
	int i;
	while (cin >> i)
	{
		ivec.push_back(i);
	}

	for (auto it = ivec.begin(); it != ivec.end() - 1; ++it)
	{
		cout << *it + *(it + 1) << endl;
	}

	//---------------------------------
	cout << "---------------------------------" << endl;

	auto it1 = ivec.begin();
	auto it2 = ivec.end() - 1;
	while (it1 < it2)
	{
		cout << *it1 + *it2 << endl;
		++it1;
		--it2;
	}
	return 0;
}
```

## 3.25

3.3.3节划分分数段的程序是使用下标运算符实现的，请利用迭代器改写该程序实现完全相同的功能。

解：

```c++
#include <vector>
#include <iostream>

using std::vector; using std::cout; using std::cin; using std::endl;

int main()
{
	vector<unsigned> scores(11, 0);
	unsigned grade;
	while (cin >> grade)
	{
		if (grade <= 100)
			++*(scores.begin() + grade / 10);
	}

	for (auto s : scores)
		cout << s << " ";
	cout << endl;

	return 0;
}
```

## 3.26

在100页的二分搜索程序中，为什么用的是 `mid = beg + (end - beg) / 2`, 而非 `mid = (beg + end) / 2 ;` ?

解：

**因为两个迭代器相互之间支持的运算只有 `-` ，而没有 `+` 。 但是迭代器和迭代器差值（整数值）之间支持 `+`。**

## 3.27

假设`txt_size`是一个无参函数，它的返回值是`int`。请回答下列哪个定义是非法的，为什么？

```c++
unsigned buf_size = 1024;
(a) int ia[buf_size]; //illegal, buf_size must be a const object
(b) int ia[4 * 7 - 14]; // legal
(c) int ia[txt_size()]; // txt_size() 的值必须要到运行时才能得到。
(d) char st[11] = "fundamental"; // 非法。数组的大小应该是12。
```

## 3.28

```c++
string sa[10];
int ia[10];
int main() {
	string sa2[10];
	int ia2[10];
}
```

数组的元素会被默认初始化。 `sa`的元素值全部为空字符串，`ia` 的元素值全部为0。 `sa2`的元素值全部为空字符串，`ia2`的元素值全部未定义。

## 3.29

相比于vector 来说，数组有哪些缺点，请例举一些。

解：

- 数组的大小是确定的。
- 不能随意增加元素。
- 不允许拷贝和赋值。

## 3.30

指出下面代码中的索引错误。

```c++
constexpr size_t array_size = 10;
int ia[array_size];
for (size_t ix = 1; ix <= array_size; ++ix)
	ia[ix] = ix;
```

当`ix`增长到 10 的时候，`ia[ix]`的下标越界。

## 3.31

编写一段程序，定义一个含有10个int的数组，令每个元素的值就是其下标值。

```c++
#include <iostream>
using std::cout; using std::endl;

int main()
{
    int arr[10];
    for (auto i = 0; i < 10; ++i) arr[i] = i;
    for (auto i : arr) cout << i << " ";
    cout << endl;

    return 0;
}
```

## 3.32

将上一题刚刚创建的数组拷贝给另一数组。利用vector重写程序，实现类似的功能。

```c++
#include <iostream>
#include <vector>
using std::cout; using std::endl; using std::vector;

int main()
{
    // array
    int arr[10];
    for (int i = 0; i < 10; ++i) arr[i] = i;
    int arr2[10];
    for (int i = 0; i < 10; ++i) arr2[i] = arr[i];

    // vector
    vector<int> v(10);
    for (int i = 0; i != 10; ++i) v[i] = arr[i];
    vector<int> v2(v);
    for (auto i : v2) cout << i << " ";
    cout << endl;

    return 0;
}
```

## 3.33

对于104页的程序来说，如果不初始化scores将会发生什么？

解：

数组中所有元素的值将会未定义。

