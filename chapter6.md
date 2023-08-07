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

**Note**

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

## 6.6

形参：在函数形参列表里面的为形参；

局部变量：在函数体中定义的变量为局部变量，函数执行结束后销毁；

局部静态变量：程序运行到创建该类对象时初始化，直到程序终止才被销毁，即使是定义在函数体中。

```c++
#include <iostream>

void count_add(int input){ //input为形参
	
    int local_var = input; //local_var位局部变量
    static int runtimes; //runtimes为静态局部变量

    std::cout << "local variable " << local_var << std::endl;
    std::cout << "static variable " << runtimes << std::endl;

    runtimes++;
}

int main()
{
    for(int i=0; i<3; i++) count_add(i);
    return 0;
}
```

```
local variable 0
static variable 0
local variable 1
static variable 1
local variable 2
static variable 2
```



## 6.7

```c++
#include <iostream>

int count_function(){

    static int runtimes(0);

   return runtimes++; //++在右侧，先返回，后运算，++在左侧，先运算，后返回。
}

int main()
{
    for(int i=0; i<3; i++) {
        std::cout << count_function() << std::endl;
    }
    return 0;
}
```

**Note** //++在右侧，先返回，后运算，++在左侧，先运算，后返回。

## 6.8

Chapter6.h中：

```c++
#ifndef CHAPTER6_CHAPTER6_H // 头文件保护符防止重复include
#define CHAPTER6_CHAPTER6_H

int fact(int);

#endif //CHAPTER6_CHAPTER6_H
```

main.cpp中：

```c++
#include <iostream>
#include "Chapter6.h"

int main() {
    int value = 5;
    std::cout << fact(value) << std::endl;
    return 0;
}

int fact(int value) {

    int res = 1;

    while (value > 0) {
        res = res * value;
        value--;
    }

    return res;
}
```

main.cpp中，fact函数定义在main函数后面，如果在main之前声明fact，将会报错。此时在Chapter6.h中声明fact函数，然后#include "Chapter6.h"就相当于在main函数之前生命了fact函数。

## 6.9

命令行操作g++，跳过

## 6.10

```c++
#include <iostream>

void swap(int *a, int *b){
    int mid = *a;
    *a = *b;
    *b = mid;
}

int main() {
    int a, b;
    if(std::cin >> a >> b) {
        swap(&a, &b);
        std::cout << a << b << std::endl;
    }
    return 0;
}
```

## 6.11

```c++
#include <iostream>

void reset(int &i){
    i = 0;
}

int main() {
    int i;
    if(std::cin >> i) {
        std::cout << i << std::endl;
        reset(i);
        std::cout << i << std::endl;
    }
    return 0;
}
```

## 6.12

```c++
#include <iostream>

void swap(int &a, int &b){
    int mid = a;
    a = b;
    b = mid;
}

int main() {
    int a, b;
    if(std::cin >> a >> b) {
        swap(a, b);
        std::cout << a << b << std::endl;
    }
    return 0;
}
```

c++中，最好使用传引用而不是传指针，因为指针需要在函数体中校验空指针。

## 6.13

```c++
void f(T) //传值

void f(&T) //传引用
```

## 6.14

**Note** ：

- 左值：C/C++语言中可以放在赋值符号左边的变量，即具有对应的可以由用户访问的[存储单元](https://baike.baidu.com/item/存储单元/8727749?fromModule=lemma_inlink)，并且能够由用户去改变其值的量。[左值](https://baike.baidu.com/item/左值/2327412?fromModule=lemma_inlink)表示存储在[计算机内存](https://baike.baidu.com/item/计算机内存/9021807?fromModule=lemma_inlink)的对象，而不是常量或计算的结果。或者说左值是代表一个内存地址值，并且通过这个内存地址，就可以对内存进行读并且写（主要是能写）操作；这也就是为什么左值可以被赋值的原因了。
- [右值](https://baike.baidu.com/item/右值/6187364?fromModule=lemma_inlink)：当一个符号或者常量放在[操作符](https://baike.baidu.com/item/操作符/8978896?fromModule=lemma_inlink)右边的时候，计算机就读取他们的“右值”，也就是其代表的真实值。
- 简单来说就是，左值相当于地址值，右值相当于数据值。右值指的是引用了一个存储在某个内存地址里的数据。 

```c++
#include <iostream>

void swap(const int &a, const int &b){
    int mid = a;
    a = b; //错误，常量整形不能修改
    b = mid;
}

int main() {
    int a, b;
    const int c=1;
    const int d=2;
    if(std::cin >> a >> b) {
        swap(1, 2);
        std::cout << a << b << std::endl;
    }
    return 0;
}
```

## 6.15

1. `s` 为`const string&` 类型，是为了避免拷贝，减少内存和时间消耗，且常量引用避免了意外改动；

2. `occurs`为`string::size_type&`类型，是为了像函数外传递结果；

3. `c`为普通传值，我们不能把const对象，字面值或需要类型转换的对象传递给普通的引用形参，但常量引用可以；

4. 如果`s`是普通引用，也可能会意外改变原来字符串的内容；

5. `occurs`如果是常量引用，那么意味着不能改变它的值；

## 6.16



```c++
bool is_empty(string& s) { return s.empty(); }
```

该函数的局限性在于不能把const对象，字面值或需要类型转换的对象传递给普通的引用形参。因此改为：

```c++
bool is_empty(const string& s) { return s.empty(); }
```

## 6.17

第一个函数使用常量引用，第二个函数使用普通引用。

## 6.18

```c++
bool compare(const matrix&, const matrix&);

vector<int> ::iterator change_val(int, vector<int>::iterator);
```

## 6.19

```c++
double calc(double); // 非法，形参与实参数量不符
int count(const string &, char); // 合法
int sum(vector<int>::iterator, vector<int>::iterator, int); // 合法
vector<int> vec(10); // 合法

(a) calc(23.4, 55.1);
(b) count("abcda",'a');
(c) calc(66);
(d) sum(vec.begin(), vec.end(), 3.8);
```

## 6.20

如果不需要对实参进行修改，则应尽量使用常量引用。因为普通引用的局限性在于不能把const对象，字面值或需要类型转换的对象传递给普通的引用形参

## 6.21

```c++
#include<iostream>

int compare(const int i, const int *const j){
    // 空指针保护
    return i>*j ? i : *j;
}

int main() {
    int i = 5;
    int j = 3;
    int res = compare(i, &j);
    std::cout << res <<std::endl;
}
```

## 6.22

```c++
#include<iostream>

void swap1(int *&pa,int*&pb){
    int *temp=pa;
    pa=pb;
    pb=temp;
}

void swap2(int **pa,int **pb){
    int *temp=*pa;
    *pa=*pb;
    *pb=temp;
}

int main()
{
    int i = 42, j = 99;
    auto lft = &i;
    auto rht = &j;
    std::cout << *lft << " " << *rht << std::endl;

    swap1(lft, rht);
    std::cout << *lft << " " << *rht << std::endl;

    swap2(&lft, &rht);
    std::cout << *lft << " " << *rht << std::endl;

    return 0;
}
```



**Note** [指针的引用](https://zhuanlan.zhihu.com/p/139543762)

类似下面这种语法，叫做指针的引用：	

```cpp
void func(int *&x)
{
	++x;
}
```

```cpp
int *&x
```

按照C++程序员的习惯，指针“*”号是和类型放在一起的。

所以只需要这样：

```cpp
int* &x
```

一目了然！



## 6.23

参考本节介绍的几个`print`函数，根据理解编写你自己的版本。 依次调用每个函数使其输入下面定义的`i`和`j`:

```c++
int i = 0, j[2] = { 0, 1 };
```

声明

```c++
void print(const int *);
void print(const char *);
void print(const int *, const int *);
void print(const int [], size_t);
void print(int (&arr)[2]);
```

```c++
#include<iostream>

void print(const int *);
void print(const char *);
void print(const int *, const int *);
void print(const int [], size_t);
void print(int (&arr)[2]);

void print(const int *i){
    if(i){ //空指针保护
        std::cout << *i << std::endl;
    }
}

void print(const char *i){
    for(; *i; ++i){
        std::cout << *i << std::endl;
    }
}

void print(const int *begin, const int *end){
    while(begin != end){
        std::cout << *begin++ << std::endl;
    }
}

void print(const int i[], size_t size){
    for(size_t s = 0; s<size; ++s){
        std::cout << i[s] << std::endl;
    }
}

void print(int (&arr)[2]){
    for(auto ele: arr){
        std::cout << ele << std::endl;
    }
}

int main()
{
    int i = 0, j[2] = { 0, 1 };
    char ch[5] = "abcd";

    print(&i);
    print(ch);
    print(std::begin(j), std::end(j));
    print(j, std::end(j)-std::begin(j));
    print(j);

}
```

## 6.24

描述下面这个函数的行为。如果代码中存在问题，请指出并改正。

```c++
void print(const int ia[10])
{
	for (size_t i = 0; i != 10; ++i)
		cout << ia[i] << endl;
}
```

该函数打印一个数组中的数字。可以正常运行，但是不够优雅。

```c++
void print(const int (&ia)[10])
{
	for (size_t i = 0; i != 10; ++i)
		cout << ia[i] << endl;
}
```

## 6.25

编写一个`main`函数，令其接受两个实参。把实参的内容连接成一个`string`对象并输出出来。

省略

## 6.26

编写一个程序，使其接受本节所示的选项；输出传递给`main`函数的实参内容。

省略

## 6.27

编写一个函数，它的参数是`initializer_list`类型的对象，函数的功能是计算列表中所有元素的和。



```cpp
#include<iostream>
#include<initializer_list>

int sum(std::initializer_list<int> list){
    int sum = 0;
    for(auto i : list){
        sum += i;
    }
    return sum;
}

int main(){
    std::initializer_list<int> elements = {1, 2, 3, 4, 5, 6, 7, 8, 9, 10};
    std::cout << sum(elements) << std::endl;

    return 0;
}
```

**Note** 函数入参也要定义为`initializer_list`类型。

## 6.28

在`error_msg`函数的第二个版本中包含`ErrCode`类型的参数，其中循环内的`elem`是什么类型？

`elem`是`const string &`类型。

因为实参是 `{"functionX", expected, actual}都是const string类型。`

std::initializer_list内的类型必须相同，且都为常量。都是引用类型吗 ？

## 6.29

在范围`for`循环中使用`initializer_list`对象时，应该将循环控制变量声明成引用类型吗？为什么？

解：

应该使用常量引用类型。`initializer_list`对象中的元素都是常量，我们无法修改`initializer_list`对象中的元素的值。

## 6.30

编译第200页的str_subrange函数，看看你的编译器是如何处理函数中的错误的。

略

## 6.31

什么情况下返回的引用无效？什么情况下返回常量的引用无效？

当被引用的对象是局部变量时，返回引用无效；当我们希望返回的对象被修改时，返回常量的引用无效。

## 6.32

下面的函数合法吗？如果合法，说明其功能；如果不合法，修改其中的错误并解释原因。

```c++
int &get(int *array, int index) { return array[index]; }
int main()
{
    int ia[10];
    for (int i = 0; i != 10; ++i)
        get(ia, i) = i;
}
```

合法，该函数使数组`ia`的第`i`位为`i`.

## 6.33

编写一个递归函数，输出`vector`对象的内容。

```c++
void print_vector(std::vector<int> &v){
    if(v.size()==0){
        return;
    }
    std::cout << v[0];
    v.erase(v.begin());
    print_vector(v);
}

int main(){
    std::vector<int> v = {1, 2, 3, 4, 5, 6, 7, 8, 9, 10};
    print_vector(v);
    
    return 0;
}
```



## 6.34

如果`factorial`函数的停止条件如下所示，将发生什么？

```
if (val != 0)
```

如果`val`为正数，从结果上来说没有区别（多乘了个1）; 如果`val`为负数，那么递归永远不会结束。

## 6.35

在调用`factorial`函数时，为什么我们传入的值是`val-1`而非`val--`？

如果传入的值是`val--`，则-1操作是在调用函数之后进行的，传入的`val`还是原来的值，递归将永远不会结束。

是否可以传入`--val`？

不可以，因为`--val`相当于在调用factorial函数之前，执行了`val=val-1`，此时该层调用栈的`val`会非预期的减少1，导致计算结果出错。

```c++
#include<iostream>

int factorial(int val){
    if(val > 1){
        return val*factorial(val-1);
    }
    return 1;
}

int main(){
    int res;
    res = factorial(5);
    std::cout << res ;

    return 0;
}
```

## 6.36 

编写一个函数声明，使其返回数组的引用并且该数组包含10个`string`对象。 不用使用尾置返回类型、`decltype`或者类型别名。

```c++ 
std::string (&function(int)) [10] 
```

## 6.37

为上一题的函数再写三个声明，一个使用类型别名，另一个使用尾置返回类型，最后一个使用`decltype`关键字。 你觉得哪种形式最好？为什么？

```c++ 
typedef std::string arrT[10];
arrT& function(int);

auto function(int) -> int (&)[10];

std::string s[10]
decltype(s) &function(int);
    
//个人还是觉得6.36的方法好，比较明了，且就一行代码，不花里胡哨。
```

## 6.38

修改`arrPtr`函数，使其返回数组的引用。

```c++
decltype(odd)& arrPtr(int i)
{
    return (i % 2) ? odd : even;
}
```

## 6.39

说明在下面的每组声明中第二条语句是何含义。 如果有非法的声明，请指出来。

```c++
(a) int calc(int, int);
	int calc(const int, const int);
	//错误，顶层const不影响传入函数的对象，即顶层const可以被非const初始化，所以这两条声明是一样的。
(b) int get();
	double get();
	//非法，重载只看参数列表，与返回值无关。
(c) int *reset(int *);
	double *reset(double *);
	//合法
```

## 6.40

下面的哪个声明是错误的？为什么？

```c++
(a) int ff(int a, int b = 0, int c = 0);
//正确

(b) char *init(int ht = 24, int wd, char bckgrnd);	
//错误，第一个参数有默认实参，右侧两个参数也必须有默认实参。
```

## 6.41

下面的哪个调用是非法的？为什么？哪个调用虽然合法但显然与程序员的初衷不符？为什么？

```c++
char *init(int ht, int wd = 80, char bckgrnd = ' ');

(a) init();
//非法，未提供第一个形参的实参；
(b) init(24,10);
//合法
(c) init(14,'*');
//合法，但第二个字符串实参被传到了整形 wd 形参中，与程序员的初衷不符。
```

## 6.42

给`make_plural`函数的第二个形参赋予默认实参's', 利用新版本的函数输出单词success和failure的单数和复数形式。

```c++
#include <iostream>
#include <string>

std::string make_plural(size_t ctr, const std::string& word, const std::string& ending = "s")
{
    return (ctr > 1) ? word + ending : word;
}

int main()
{
    std::cout << "single: " << make_plural(1, "success", "es") << " "
         << make_plural(1, "failure") << std::endl;
    std::cout << "plural : " << make_plural(2, "success", "es") << " "
         << make_plural(2, "failure") << std::endl;

    return 0;
}
```

## 6.43

你会把下面的哪个声明和定义放在头文件中？哪个放在源文件中？为什么？

```c++
(a) inline bool eq(const BigInt&, const BigInt&) {...}
(b) void putValues(int *arr, int size);
```

全部放在头文件中，a是内联函数，b是声明。

## 6.44

将6.2.2节的`isShorter`函数改写成内联函数。

```c++
#include <iostream>
#include <string>

inline bool isShorter(const std::string &s1, const std::string &s2){
    return s1.size() < s2.size();
}

int main()
{
    std::cout << isShorter("123","1234") << std::endl;

    return 0;
}
```

## 6.45

回顾在前面的练习中你编写的那些函数，它们应该是内联函数吗？ 如果是，将它们改写成内联函数；如果不是，说明原因。

引用p124页的说法，内联机制用于优化规模较小，流程直接，频繁调用的函数，以空间换时间。

## 6.46

能把`isShorter`函数定义成`constexpr`函数吗？ 如果能，将它改写成`constxpre`函数；如果不能，说明原因。

不能。`constexpr`函数的返回值类型及所有形参都得是字面值类型。

```c++
//这样写是合法的，只不过失去了意义。
constexpr bool isShorter(const std::string &s1, const std::string &s2){
//    return s1.size() < s2.size();
    return true;
}

int main()
{
    std::cout << isShorter("123","1234") << std::endl;

    return 0;
}
```

## 6.47

改写6.3.2节练习中使用递归输出`vector`内容的程序，使其有条件地输出与执行过程有关的信息。 例如，每次调用时输出`vector`对象的大小。 分别在打开和关闭调试器的情况下编译并执行这个程序。

```c++
#define NDEBUG 1 //有这个宏定义时，将会输出Vector size

#include <vector>
#include <iostream>

void print_vector(std::vector<int> &v){
#ifndef NDEBUG
    std::cout << "vector size: " << v.size() << std::endl;
#endif
    if(v.size()==0){
        return;
    }
    std::cout << v[0];
    v.erase(v.begin());
    print_vector(v);
}

int main(){
    std::vector<int> v = {1, 2, 3, 4, 5, 6, 7, 8, 9, 10};
    print_vector(v);

    return 0;
}
```

## 6.48

说明下面这个循环的含义，它对assert的使用合理吗？

```c++
string s;
while (cin >> s && s != sought) { } //空函数体
```
assert(cin);不合理。从这个程序的意图来看，应该用
```c++
assert(s == sought);
```



## 6.49

什么是候选函数？什么事可行函数？

- 候选函数：1）与被调用的函数同名；2）其声明在调用点可见
- 可行函数：1）在候选函数中；2）形参与实参数量相同（默认参数除外）；3）每个实参的类型与对应的形参类型相同，或者可以转换成形参的类型

## 6.50

已知有第217页对函数`f`的声明，对于下面的每一个调用列出可行函数。 其中哪个函数是最佳匹配？ 如果调用不合法，是因为没有可匹配的函数还是因为调用具有二义性？

```c++
void f();
void f(int);
void f(int, int);
void f(double, double = 3.14);
```



```c++
(a) f(2.56, 42) //非法，二义性
(b) f(42)		//合法，调用2
(c) f(42, 0)	//合法，调用3
(d) f(2.56, 3.14)//合法，调用4
```

## 6.51

编写函数`f`的4版本，令其各输出一条可以区分的消息。 验证上一个练习的答案，如果你的回答错了，反复研究本节内容直到你弄清自己错在何处。

```c++
#include <vector>
#include <iostream>

void f();
void f(int);
void f(int, int);
void f(double, double = 3.14);

void f(){
    std::cout << "this is f()" << std::endl;
}

void f(int){
    std::cout << "this is f(int)" << std::endl;
}

void f(int, int){
    std::cout << "this is f(int, int)" << std::endl;
}

void f(double, double){
    std::cout << "this is f(double, double=3.14)" << std::endl;
}

int main(){
    f();
    f(12);
    f(12, 13);
    f(1.2, 1.3);

    f(1.1);

    return 0;
}
```

```c++
//输出：
this is f()
this is f(int)
this is f(int, int)
this is f(double, double=3.14)
this is f(double, double=3.14)
```

## 6.52

已知有如下声明：

```c++
void manip(int ,int);
double dobj;
```

请指出下列调用中每个类型转换的等级。

```c++
(a) manip('a', 'z'); 	//第三级，类型提升实现的匹配。
(b) manip(55.4, dobj);	//第四级，算数类型转换实现的匹配。
```

## 6.53

说明下列每组声明中的第二条语句会产生什么影响，并指出哪些不合法（如果有的话）。

```c++
(a) int calc(int&, int&); 
	int calc(const int&, const int&); //合法，底层const引用与非引用不冲突。
(b) int calc(char*, char*);
	int calc(const char*, const char*); //合法，底层const指针与非指针不冲突。
(c) int calc(char*, char*);
	int calc(char* const, char* const); //非法，重载不区分顶层const，两个声明其实是一个。
```

## 6.54

编写函数的声明，令其接受两个`int`形参并返回类型也是`int`；然后声明一个`vector`对象，令其元素是指向该函数的指针。 

```c++
#include <vector>

int func(int, int){
    return 0;
};

int main(){

    std::vector<int (*) (int, int)> v = {func};

    return 0;
}
```

## 6.55

编写4个函数，分别对两个`int`值执行加、减、乘、除运算；在上一题创建的`vector`对象中保存指向这些函数的指针。

```c++
#include <vector>
#include <iostream>

int add(int i, int j){
    return i+j;
};

int sub(int i, int j){
    return i-j;
};

int mul(int i, int j){
    return i*j;
};

int divd(int i, int j){
    return i/j;
};


int main(){

    std::vector<int (*) (int, int)> v = {add, sub, mul, divd};

    for (int (*func) (int, int) : v){
        std::cout << func(10, 5) << std::endl;
    }
	//输出15，5，50，2
    return 0;
}
```

## 6.56

//输出15，5，50，2
