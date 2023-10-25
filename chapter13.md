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