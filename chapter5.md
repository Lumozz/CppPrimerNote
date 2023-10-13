# chapter5

## 5.1

什么是空语句？什么时候会用到空语句？

什么是空语句？什么时候会用到空语句？

解：

只含义一个单独的分号的语句是空语句。如：`;`。

如果在程序的某个地方，语法上需要一条语句但是逻辑上不需要，此时应该使用空语句。

```c++
while (cin >> s && s != sought)
	;
```

## 5.2

什么是块？什么时候会用到块？

解：

用花括号括起来的语句和声明的序列就是块。

```
{
	// ...
}
```



如果在程序的某个地方，语法上需要一条语句，而逻辑上需要多条语句，此时应该使用块

```c++
while (val <= 10) {
	sum += val;
	++val;
}
```

## 5.3

使用逗号运算符重写1.4.1节的`while`循环，使它不再需要块，观察改写之后的代码可读性提高了还是降低了。

```c++
while (val <= 10)
    sum += val, ++val;
```

代码的可读性反而降低了。

## 5.4

说明下列例子的含义，如果存在问题，试着修改它。

```c++
(a) while (string::iterator iter != s.end()) { /* . . . */ }

(b) while (bool status = find(word)) { /* . . . */ }
		if (!status) { /* . . . */ }
```

- (a) 这个循环试图用迭代器遍历`string`，但是变量的定义应该放在循环的外面，目前每次循环都会重新定义一个变量，明显是错误的。
- (b) 这个循环的`while`和`if`是两个独立的语句，`if`语句中无法访问`status`变量，正确的做法是应该将`if`语句包含在`while`里面。

## 5.5

写一段自己的程序，使用`if else`语句实现把数字转换为字母成绩的要求。

```c++
#include <iostream>
#include <vector>
#include <string>
using std::vector; using std::string; using std::cout; using std::endl; using std::cin;

int main()
{
    vector<string> scores = { "F", "D", "C", "B", "A", "A++" };
    for (int g; cin >> g;)
    {
        string letter;
        if (g < 60)
        {
            letter = scores[0];
        }
        else
        {
            letter = scores[(g - 50) / 10];
            if (g != 100)
                letter += g % 10 > 7 ? "+" : g % 10 < 3 ? "-" : "";
        }
        cout << letter << endl;
    }

    return 0;
}
```

## 5.6

改写上一题的程序，使用条件运算符代替`if else`语句。

```c++
#include <iostream>
#include <vector>
#include <string>
using std::vector; using std::string; using std::cout; using std::endl; using std::cin;

int main()
{
    vector<string> scores = { "F", "D", "C", "B", "A", "A++" };

    int grade = 0;
    while (cin >> grade)
    {
        string lettergrade = grade < 60 ? scores[0] : scores[(grade - 50) / 10];
        lettergrade += (grade == 100 || grade < 60) ? "" : (grade % 10 > 7) ? "+" : (grade % 10 < 3) ? "-" : "";
        cout << lettergrade << endl;
    }

    return 0;
}
```

## 5.7

改写下列代码段中的错误。

```c++
(a) if (ival1 != ival2) 
		ival1 = ival2
    else 
    	ival1 = ival2 = 0;
(b) if (ival < minval) 
		minval = ival;
    	occurs = 1;
(c) if (int ival = get_value())
    	cout << "ival = " << ival << endl;
    if (!ival)
    	cout << "ival = 0\n";
(d) if (ival = 0)
    	ival = get_value();
```

解：

- (a) `ival1 = ival2` 后面少了分号。
- (b) 应该用花括号括起来。
- (c) `if (!ival)` 应该改为 `else`。
- (d) `if (ival = 0)` 应该改为 `if (ival == 0)`。

## 5.8

什么是“悬垂else”？C++语言是如何处理else子句的？

解：

用来描述在嵌套的`if else`语句中，如果`if`比`else`多时如何处理的问题。C++使用的方法是`else`匹配最近没有配对的`if`。

## 5.9

编写一段程序，使用一系列`if`语句统计从`cin`读入的文本中有多少元音字母。

解：

```c++
#include <iostream>

using std::cout; using std::endl; using std::cin;

int main()
{
	unsigned aCnt = 0, eCnt = 0, iCnt = 0, oCnt = 0, uCnt = 0;
	char ch;
	while (cin >> ch)
	{
		if (ch == 'a') ++aCnt;
		else if (ch == 'e') ++eCnt;
		else if (ch == 'i') ++iCnt;
		else if (ch == 'o') ++oCnt;
		else if (ch == 'u') ++uCnt;
	}
	cout << "Number of vowel a: \t" << aCnt << '\n'
		<< "Number of vowel e: \t" << eCnt << '\n'
		<< "Number of vowel i: \t" << iCnt << '\n'
		<< "Number of vowel o: \t" << oCnt << '\n'
		<< "Number of vowel u: \t" << uCnt << endl;

	return 0;
}
```

## 5.9

编写一段程序，使用一系列`if`语句统计从`cin`读入的文本中有多少元音字母。

```c++
#include <iostream>

using std::cout; using std::endl; using std::cin;

int main()
{
	unsigned aCnt = 0, eCnt = 0, iCnt = 0, oCnt = 0, uCnt = 0;
	char ch;
	while (cin >> ch)
	{
		if (ch == 'a') ++aCnt;
		else if (ch == 'e') ++eCnt;
		else if (ch == 'i') ++iCnt;
		else if (ch == 'o') ++oCnt;
		else if (ch == 'u') ++uCnt;
	}
	cout << "Number of vowel a: \t" << aCnt << '\n'
		<< "Number of vowel e: \t" << eCnt << '\n'
		<< "Number of vowel i: \t" << iCnt << '\n'
		<< "Number of vowel o: \t" << oCnt << '\n'
		<< "Number of vowel u: \t" << uCnt << endl;

	return 0;
}
```

## 5.10

我们之前实现的统计元音字母的程序存在一个问题：如果元音字母以大写形式出现，不会被统计在内。编写一段程序，既统计元音字母的小写形式，也统计元音字母的大写形式，也就是说，新程序遇到'a'和'A'都应该递增`aCnt`的值，以此类推。

解：

```c++
#include <iostream>
using std::cin; using std::cout; using std::endl;

int main()
{
	unsigned aCnt = 0, eCnt = 0, iCnt = 0, oCnt = 0, uCnt = 0;
	char ch;
	while (cin >> ch)
		switch (ch)
	{
		case 'a':
		case 'A':
			++aCnt;
			break;
		case 'e':
		case 'E':
			++eCnt;
			break;
		case 'i':
		case 'I':
			++iCnt;
			break;
		case 'o':
		case 'O':
			++oCnt;
			break;
		case 'u':
		case 'U':
			++uCnt;
			break;
	}

	cout << "Number of vowel a(A): \t" << aCnt << '\n'
		<< "Number of vowel e(E): \t" << eCnt << '\n'
		<< "Number of vowel i(I): \t" << iCnt << '\n'
		<< "Number of vowel o(O): \t" << oCnt << '\n'
		<< "Number of vowel u(U): \t" << uCnt << endl;

	return 0;
}
```

## 5.11

修改统计元音字母的程序，使其也能统计空格、制表符、和换行符的数量。

解：

```c++
#include <iostream>

using std::cin; using std::cout; using std::endl;

int main()
{
	unsigned aCnt = 0, eCnt = 0, iCnt = 0, oCnt = 0, uCnt = 0, spaceCnt = 0, tabCnt = 0, newLineCnt = 0;
	char ch;
	while (cin >> std::noskipws >> ch)  //noskipws(no skip whitespce)
		switch (ch)
	{
		case 'a':
		case 'A':
			++aCnt;
			break;
		case 'e':
		case 'E':
			++eCnt;
			break;
		case 'i':
		case 'I':
			++iCnt;
			break;
		case 'o':
		case 'O':
			++oCnt;
			break;
		case 'u':
		case 'U':
			++uCnt;
			break;
		case ' ':
			++spaceCnt;
			break;
		case '\t':
			++tabCnt;
			break;
		case '\n':
			++newLineCnt;
			break;
	}

	cout << "Number of vowel a(A): \t" << aCnt << '\n'
		<< "Number of vowel e(E): \t" << eCnt << '\n'
		<< "Number of vowel i(I): \t" << iCnt << '\n'
		<< "Number of vowel o(O): \t" << oCnt << '\n'
		<< "Number of vowel u(U): \t" << uCnt << '\n'
		<< "Number of space: \t" << spaceCnt << '\n'
		<< "Number of tab char: \t" << tabCnt << '\n'
		<< "Number of new line: \t" << newLineCnt << endl;

	return 0;
}
```

## 5.12

修改统计元音字母的程序，使其能统计含以下两个字符的字符序列的数量：`ff`、`fl`和`fi`。

解：

```c++
#include <iostream>

using std::cin; using std::cout; using std::endl;

int main()
{
	unsigned aCnt = 0, eCnt = 0, iCnt = 0, oCnt = 0, uCnt = 0, spaceCnt = 0, tabCnt = 0, newLineCnt = 0, ffCnt = 0, flCnt = 0, fiCnt = 0;
	char ch, prech = '\0';
	while (cin >> std::noskipws >> ch)
	{
		switch (ch)
		{
		case 'a':
		case 'A':
			++aCnt;
			break;
		case 'e':
		case 'E':
			++eCnt;
			break;
		case 'i':
			if (prech == 'f') ++fiCnt;
		case 'I':
			++iCnt;
			break;
		case 'o':
		case 'O':
			++oCnt;
			break;
		case 'u':
		case 'U':
			++uCnt;
			break;
		case ' ':
			++spaceCnt;
			break;
		case '\t':
			++tabCnt;
			break;
		case '\n':
			++newLineCnt;
			break;
		case 'f':
			if (prech == 'f') ++ffCnt;
			break;
		case 'l':
			if (prech == 'f') ++flCnt;
			break;
		}
		prech = ch;
	}

	cout << "Number of vowel a(A): \t" << aCnt << '\n'
		<< "Number of vowel e(E): \t" << eCnt << '\n'
		<< "Number of vowel i(I): \t" << iCnt << '\n'
		<< "Number of vowel o(O): \t" << oCnt << '\n'
		<< "Number of vowel u(U): \t" << uCnt << '\n'
		<< "Number of space: \t" << spaceCnt << '\n'
		<< "Number of tab char: \t" << tabCnt << '\n'
		<< "Number of new line: \t" << newLineCnt << '\n'
		<< "Number of ff: \t" << ffCnt << '\n'
		<< "Number of fl: \t" << flCnt << '\n'
		<< "Number of fi: \t" << fiCnt << endl;

	return 0;
}
```

## 5.13

下面显示的每个程序都含有一个常见的编码错误，指出错误在哪里，然后修改它们。

```c++
(a) unsigned aCnt = 0, eCnt = 0, iouCnt = 0;
    char ch = next_text();
    switch (ch) {
        case 'a': aCnt++;
        case 'e': eCnt++;
        default: iouCnt++;
    }
// 没有break
(b) unsigned index = some_value();
    switch (index) {
        case 1:
            int ix = get_value();
            ivec[ ix ] = index;
            break;
        default:
            ix = ivec.size()-1;
            ivec[ ix ] = index;
    }
//在default分支当中，ix未定义。应该在外部定义ix。
(c) unsigned evenCnt = 0, oddCnt = 0;
    int digit = get_num() % 10;
    switch (digit) {
        case 1, 3, 5, 7, 9:
            oddcnt++;
            break;
        case 2, 4, 6, 8, 10:
            evencnt++;
            break;
    }
//(c) case后面应该用冒号而不是逗号。
(d) unsigned ival=512, jval=1024, kval=4096;
    unsigned bufsize;
    unsigned swt = get_bufCnt();
    switch(swt) {
        case ival:
            bufsize = ival * sizeof(int);
            break;
        case jval:
            bufsize = jval * sizeof(int);
            break;
        case kval:
            bufsize = kval * sizeof(int);
            break;
    }
// case标签必须是整型常量表达式。
```

## EMPTY!!!

## 5.23

编写一段程序，从标准输入读取两个整数，输出第一个数除以第二个数的结果。

解：

```c++
#include <iostream>
using std::cin;
using std::cout;
using std::endl;

int main() 
{
    int i, j; 
    cin >> i >> j;
    cout << i / j << endl;
 
    return 0;
}
```

## 5.24

修改你的程序，使得当第二个数是0时抛出异常。先不要设定`catch`子句，运行程序并真的为除数输入0，看看会发生什么？

解：

```c++
#include <iostream>
#include <stdexcept>

int main(void)
{
    int i, j;
    std::cin >> i >> j;
    if (j == 0)
        throw std::runtime_error("divisor is 0");
    std::cout << i / j << std::endl;

    return 0;
}
```

## 5.25

修改上一题的程序，使用`try`语句块去捕获异常。`catch`子句应该为用户输出一条提示信息，询问其是否输入新数并重新执行`try`语句块的内容。

解：

```c++
#include <iostream>
#include <stdexcept>
using std::cin; using std::cout; using std::endl; using std::runtime_error;

int main(void)
{
    for (int i, j; cout << "Input two integers:\n", cin >> i >> j; )
    {
        try 
        {
            if (j == 0) 
                throw runtime_error("divisor is 0");
            cout << i / j << endl;
        }
        catch (runtime_error err) 
        {
            cout << err.what() << "\nTry again? Enter y or n" << endl;
            char c;
            cin >> c;
            if (!cin || c == 'n')
                break;
        }
    }

    return 0;
}
```