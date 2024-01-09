# 拷贝、赋值、销毁

## 基本概念

一个类，通过定义**五种**特殊的成员函数来控制对此类型**拷贝、移动、赋值和销毁**时做什么。

- 拷贝构造函数
- 拷贝赋值运算符
- 移动构造函数
- 移动赋值运算符
- 析构函数

### 直接初始化与拷贝初始化

1. 直接初始化：

	- 使用圆括号`()`或花括号`{}`直接初始化一个对象。

	- 直接初始化时，编译器会直接调用构造函数（默认构造函数、带参数的构造函数、拷贝构造函数），实际上是要求编译器使用普通的函数匹配，根据我们提供的参数，选择最合适的构造函数。

	- ```c++
		MyClass obj1(10);  // 使用单个参数的构造函数
		MyClass obj2{20};  // 统一初始化（C++11）
		```

2. 拷贝初始化

	- 使用等号`=`进行初始化

	- 首先创建一个临时对象，然后把这个临时对象拷贝到正在创建的对象中

	- 可能会进行类型转换

	- 然后编译器会调用构造函数（默认构造函数、带参数的构造函数、拷贝构造函数等）初始化对象

	- ```c++
		MyClass obj3 = 30;  // 拷贝初始化
		```

	常见的函数传参，不论是传入还是传出参数，只要对象类型不是引用，那么就会执行拷贝初始化。因此，拷贝构造函数的参数类型**必须是引用类型**，否则就是把钥匙锁在了箱子里。

## 拷贝构造函数
