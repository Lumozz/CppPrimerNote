# 定义抽象数据类型

涉及内容：this指针，成员函数，构造函数

- 对*实例*和*对象*两个词的解释：

  在面向对象编程（OOP）中，"对象"和"实例"两个术语经常被互换使用，它们在许多情况下都有**相同的意思**。然而，它们的含义略有差别。

  - **对象**：对象是类的一个具体化表示，它包含了由类定义的数据和行为。对象是类的一个具体实例，它的数据成员可以拥有实际的值。
  - **实例**：实例通常是指类的一个具体实例。创建一个类的实例就意味着实例化了一个类的对象。"实例"的概念强调了从类（模板）到特定对象（实例）的过程。

  在许多情况下，两者可以互换使用。例如，"创建一个对象"和"实例化一个类"的意思是相同的。但是，"实例"这个词更**强调过程和动态性**，而"对象"这个词更强调**结果和静态性**。

  例如，如果我们有一个名为 `Dog` 的类，并创建一个这个类的新对象 `myDog`，那么 `myDog` 是 `Dog` 类的一个实例，同时也是一个 `Dog` 类的对象。

  综上所属，这两个词可以互换使用，但**对象常用作名词**， **实例常用作动词**。此外，c++书籍中常用对象一词，而Python书籍中常用实例一词，可能只是习惯问题。

- 书中两个类

  - 对`Sales_data`类的描述

    书中描述了使用Sales_data类的一种方式：读取用户输入的书籍销售数据，如果这条销售数据和上一条都是同一本书的（ISBN号相同），则将两条数据融合，否则就把上一本书的数据打印出来。

  - 对`Screen`类的描述

    书中写了一个名为`Screen`的类作为例子。这个类用两个数代表屏幕的长和宽，用一个字符串表示二维屏幕中的所有字符，长度等于屏幕长宽的积。由于构造函数只能传入一个`char`字符，所以一开始屏幕中所有位置的字符都是一样的。


## 类的基本概念：

- 类的组成部分：
  - 数据成员
  - 成员函数
  - 访问修饰符
  - 构造函数和析构函数
  - 静态成员

- **数据抽象**和**封装**

  数据抽象，是指程序的**接口**和**实现**分离的技术

  封装，是实现数据抽象的一种技术

  数据抽象是一个概念，他描述了隐藏内部细节并只暴露出有限接口的做法。

  封装是实现数据抽象的一种技术，它通过隐藏对象的内部状态和细节，强制使用对象提供的方法来实现数据抽象。

- 类的声明和定义

  - 每个类定义了唯一的类型，即便两个类的成员完全一样，这两个类也是两个不同类型。

  - 声明对象时，`Sales_data item1`和`class Sales_data item1`是完全等价的。

  - 可以只声明不定义，`class Screen;`，成为**前向声明**。

  - **类成员**的数据类型**不能是该类自己**，但允许包含指向他自身类型的引用或指针。

    ```c++
    class Link-screen{
    	Screen window;
    	Link_screen *next;
    	Link_screen *prev;
    }
    ```

    

- **`const`对象和`const`数据成员**

  一个对象是否是常量，取决于声明它的方式，与类中是否包含`const`成员无关。

  如：

  ```c++
  class MyClass {
  public:
      int nonConstMember;
      const int constMember;
  
      MyClass(int nc, int c) : nonConstMember(nc), constMember(c) {}
  };
  
  // 创建非常量对象
  MyClass obj1(10, 20);
  obj1.nonConstMember = 30; // 正确，可以为 非常量对象 中的非常量数据成员赋值
  // obj1.constMember = 40; // 错误，不可移为 常量对象 中的常量数据成员赋值
  
  // 创建常量对象
  const MyClass obj2(10, 20);
  // obj2.nonConstMember = 30; // 错误，不可以为 常量对象 中的 非常量数据成员赋值
  // obj2.constMember = 40; // 错误，不可以为 常量对象 中的 常量数据成员赋值
  ```

  对于一个**非常量对象**，可以修改其中非常量的数据成员，但是不能修改常量数据成员；而对于**常量**对象，其中的常量和非常量数据成员均不可被修改。

  - 可变数据成员

    可以用`mutable`声明一个可变数据成员，一个`const`成员函数可以改变一个`const`对象的可变成员的值。

    ```c++
    #include <iostream>
    
    class MyClass {
    private:
        mutable int myValue; // 声明可变成员
    
    public:
        MyClass(int value) : myValue(value) {}
    
        void setValue(int value) const {
            myValue = value; // 在 const 成员函数中修改 mutable 成员的值
        }
    
        int getValue() const {
            return myValue;
        }
    };
    
    int main(){
        const MyClass c(1);
        std::cout << c.getValue() << std::endl;
        c.setValue(2); //即便是在const对象中，可变数据成员的值依然是可被修改的。
        std::cout << c.getValue() << std::endl;
    }
    ```

- 

## 成员函数与数据成员

- 类内数据成员初始值

  类内初始值必须使用=的初始化形式，或花括号括起来的直接初始化形式。

  类内初始值必须使用 `=` 或 `{}` 的形式，而不能使用括号 `()` 的形式进行初始化，这主要是为了避免编译器可能会将这种初始化语法误解为函数声明，从而导致错误或者不可预期的行为(Most vexing parse)。

  ```c++
  class Widget 
  {
  private: 
    typedef int x;
    int z(x); // 这是个函数还是初始化定义？
  };
  ```

- 成员函数声明与定义：

  - 成员函数的声明必须在类的内部，而它的定义既可以在类的内部也可以在类的外部。

    定义在类外时，返回类型，参数列表，函数名，是否`const`都需要与声明一致。必要时可以声明inline。

  - `inline`内联

    定义在类内时，类内部的成员函数是自动inline的。

    `inline` 关键字可以在函数的**声明或定义**处使用，但通常**建议在函数定义处使用**。因为`inline`是向编译器提出的一个建议，希望它将函数调用替换为函数体本身，以减少函数调用的开销。这需要编译器看到整个函数体才能做出决定。

  - 成员函数可以随意使用类中的其他成员而无须在意成员的出现次序。因为编译器首先编译成员声明，然后才是函数体。**但是类型名定义除外，类型名必须定义在使用之前**。

  - 成员函数可以被重载。

- 成员函数调用：

  调用成员函数时，实际上是替某个实例对象调用它。

  成员函数通过一个名为**this**的额外的饮食参数来访问调用它的哪个实例对象。当调用一个成员函数时，用请求该函数的**对象地址**初始化this。即，**this指针指向调用成员函数的那个对象**。

  在成员函数内部，可以**直接**使用该实例对象的成员，而无需使用成员访问运算符。这是因为任何对类成员的直接访问都被看作this的隐式引用，就像书写了this->一样。

  成员函数内部**可以显式**使用this，但是类内**禁止**自定义任何名为this的参数或变量。

- `const`成员函数: 

  常量对象，以及常量对象的引用或指针，都智能调用常量成员函数。

  this是**常量指针**，是一个顶层`const`，如`Sales_data *const`，默认情况下，它是指向非常量对象的常量指针。  

  - `const`成员函数的语法逻辑，与普通接受`const`形参的函数相同  
    - 如果对象是常量，则this指针应该是指向常量的常量指针。成员函数隐式接受this指针作为形参，此时，形参必须声明为指向常量的指针，因为非`const`形参无法接受`const`实参。这一点和普通的函数`const`形参语法逻辑是一样的。但是因为this指针的形参是隐式的，所以把`const`标记写在参数列表后面。
  
  
  - `const`成员函数存在的意义，设计逻辑：
  
    - 一个对象，可以被声明为`const`常量对象，也可以被声明为普通的非常量对象。常量对象中的数据成员是不可悲修改的。为了避免成员函数修改常量对象的数据成员，c++中规定，常量对象只能调用**不会修改数据成员**的成员函数，而这类函数统称为*`const`成员函数*。
  
  
    - 即使非 `const` 成员函数实际上并没有修改任何数据成员，编译器也不能知道这一点。非 `const` 成员函数有能力修改对象的状态，所以编译器不允许在常量对象上调用非 `const` 成员函数，以防止可能的修改。  


  ```c++
  class MyClass {
  public:
      int x = 1;
  
      void set_x(int val) {
          x = val; 
      }
  
      // 非常量成员函数
      int get_x(){
          return x;
      }
      // 常量成员函数
      int get_x_const() const {
          return x;
      }
  };
  
  int main(){
      MyClass m; // 声明非常量对象
      m.set_x(3);
      std::cout << m.get_x() << std::endl; //正确，非常量对象可以调用非常量成员函数
      std::cout << m.get_x_const() << std::endl; //正确， 非常量对象可以调用常量成员函数，因为const形参可以接受非const实参
      
      const MyClass mc; // 声明常量对象
      std::cout << mc.get_x() << std::endl; //错误，常量对象不可以调用非常量成员函数，因为非const形参不可以接受const实参
      std::cout << mc.get_x_const() << std::endl; //正确
  }
  ```

  - 一个`const`成员函数如果以引用的方式返回`*this`，那么他的返回类型将是常量引用。

  这一点可能导致链式编程失效，因为即便最开始生命的对象是非`const`的，在经过某个`const`成员函数后，会返回`const`的引用，因此后续的调用可能会出问题。

  为解决此问题，可以使用基于函数重载，声明一个非`const`版本的成员函数。

- 返回this对象的成员函数

  意义：

  返回一个*this可以很方便的实用链式编程风格

  ```c++
  #include <iostream>
  
  class Myclass{
  public:
      int i = 0;
  
      Myclass() = default;
      Myclass(int input): i(input) {};
  
      void combine_return_void(Myclass& other){
          i += other.i;
      }
      Myclass combine_return_this(Myclass& other){
          i += other.i;
          return *this;
      }
  };
  
  int main(){
      Myclass c1(1);
      Myclass c2(2);
      Myclass c3(3);
  
  	// c1.combine_return_this(c2)的返回值是c1,因此可以直接在后面使用.i，
      //如果此处调用的是combine_return_void，c1.combine_return_this(c2)计算完成后返回void,
      // 无法直接使用.i，比较麻烦。
      // 但无论返回什么值，c1内部的值都会变化。
      std::cout << c1.combine_return_this(c2).i << std::endl;
      std::cout << c1.combine_return_this(c2).combine_return_this(c3).i << std::endl;
  }
  ```

  如果该成员函数的返回值设置为引用，则当返回`*this`时，返回的是对象本身而非副本。

  ```c++
  Myclass& combine_return_this(Myclass& other){ // 这里返回Myclass的引用，表明返回对象本身
          i += other.i;
          return *this;
      }
  ```

- 非成员函数

  非成员函数和静态成员函数

## 构造函数

构造函数的任务是**初始化**类对象的**数据成员**，无论何时，只要类的对象被创建，就会执行构造函数。

构造函数没有返回类型

构造函数不能被声明为`const`，构造函数可以向`const`对象写入值

如果没有显式声明构造函数，则编译器自动添加**合成默认构造函数**

- 默认构造函数

  每个类只能有一个默认构造函数

  默认构造函数没有任何实参，或者所有参数都有默认值

  `Sale_data(std::string s = " "): bookNo(s){};`和`Sale_data(): bookNo(" "){};`都是默认构造函数。但是只能写一个，因为每个类只允许出现一个默认构造函数。

  初始化规则

  - 如果存在类内初始值，用该值初始化
  - 否则，默认初始化

  声明定义默认构造函数

  ```c++
  class MyClass {
  public:
      MyClass() = default;
  };
  
  int main(){
      MyClass c;
  }
  ```

  默认构造参数存在的必要性：

  - 当显式写了有实参的构造函数时，编译器不会自动生成默认构造函数。这意味着创建对象时必须传入初始化参数，例如

    ```c++
    class Myclass{
    public:
        int i = 0;
        Myclass(int input): i(input) {}; // 没有默认构造函数
    
    };
    
    int main(){
        Myclass c1(1); // 正确，传入了参数
        Myclass c2; // 错误，因没有传入参数，且没有默认构造函数，因此编译不通过。
    }
    ```
    ```c++
    class Myclass{
    public:
        int i = 0;
        Myclass(){};// 有默认构造函数
        Myclass(int input): i(input) {}; 
    
    };
    
    int main(){
        Myclass c1(1); // 正确，传入了参数
        Myclass c2; // 正确，调用默认构造函数
    }
    ```

  - 合成的默认构造函数可能执行错误的操作

    对于基本类型（如 `int`、`float`、`char`等在 C++ 中），合成的默认构造函数不会初始化它们。这意味着，如果一个类有这样的成员变量，它们将包含未定义的值，这可能会导致程序中的错误。

    ```c++
    class MyClass {
        int myValue; // 如果没有定义默认构造函数，myValue 将有一个未定义的值
    };
    ```

  - 有时候编译器不能为某些类合成默认构造函数

    如果类包含一个或多个不能默认构造的成员，那么编译器就无法为该类生成默认构造函数。例如，如果类的成员是引用或者是常量，或者类的成员是另一个没有默认构造函数的类的对象。

    ```c++
    class OtherClass {
    public:
        OtherClass(int value); // OtherClass 无默认构造函数
    };
    
    class MyClass {
        OtherClass myObject; // MyClass 不能有合成的默认构造函数，因为 myObject 不能默认构造
    };
    
    int main(){
        MyClass c; //错误
    }
    ```

  **注意**：

  `MyClass c()` 和 `MyClass c` 在 C++ 中有非常重要的区别。

  这要提到"最令人困惑的解析"（Most Vexing Parse）。这是 C++ 语言中的一种语法解析现象，它源于 C++ 的语法规则中的一条：如果一段代码可以被解析为声明，那么它就应该被解析为声明。

  这种现象最常见的形式就是试图用默认构造函数初始化一个对象，但编译器却将其解析为函数声明。例如：

  ```c++
  class MyClass {};
  
  // 意图是创建一个 MyClass 对象
  MyClass obj();
  
  // 但是编译器将其解析为一个函数声明。
  // 函数名为 obj，没有参数，返回一个 MyClass 对象。
  ```

  上述代码中的 `MyClass obj();` 是最令人困惑的解析的一个例子。开发者可能意图是创建一个 `MyClass` 类型的对象 `obj`，并使用默认构造函数进行初始化。但是，由于 C++ 的语法规则，编译器实际上将其解析为一个函数声明，函数名为 `obj`，没有参数，返回一个 `MyClass` 类型的对象。

  为了避免这种混淆，我们可以使用不同的语法来创建并初始化对象，例如：

  ```c++
  MyClass obj;      // 创建一个 MyClass 对象
  MyClass obj{};    // 使用花括号创建一个 MyClass 对象
  MyClass obj = MyClass(); // 使用赋值语法创建一个 MyClass 对象
  ```

  这些语法都会创建一个 `MyClass` 类型的对象 `obj`，并使用默认构造函数进行初始化，而不会引发最令人困惑的解析问题。

- 构造函数初始化数据成员的方式
  - 构造函数的函数体内赋值初始化

    本质是赋值，如果一个数据成员是`const`类型，则改种方法不可行。

  - 使用初始化列表初始化

    - 数据成员被初始化的顺序和在类中定义顺序一致，与在列表中出现的顺序无关。

- 委托构造函数

  委托构造函数是C++11引入的一种特性，它允许一个构造函数调用同一个类中的另一个构造函数，以减少代码冗余和提高代码复用性。

  例：

  ```c++
  class MyClass {
  public:
      MyClass(int value) : m_value(value) {} //普通的构造函数，被委托。
  
      // 委托构造函数
      MyClass() : MyClass(0) {} // 使用默认值0调用上面的构造函数
  
  private:
      int m_value;
  };
  ```

- 注意事项

  - 如果成员是`const`、引用，或者属于某种未提供默认构造函数的类类型，则**必须通过构造函数初值列表**为这些成员提供初始值。

  - 通常情况下，直接初始化数据成员比先初始化，再赋值要快。

  - 尽量避免用某些数据成员初始化其它数据成员，因为可能带来顺序相关问题。

## 拷贝、赋值、析构

见13章

## 访问控制、封装

- 访问说明符

  - `private` 成员只能被类的方法访问，不能被类的对象或派生类访问。
  - `protected` 成员可以被类的方法和派生类的方法访问，但不能被类的对象访问。
  - `public` 成员可以被任何人访问。

  一个访问说明符可以出现任意多次，作用范围从出现行，到下一个访问说明符为止。

- `class`和`struct`的区别

  `class` 默认`private`， `struct`默认`public`除此之外没有任何区别

- 封装

  **封装** 是一种将数据（变量）和操作数据的方法包装在一起的编程技巧。这意味着类的内部实现细节被隐藏在类的内部，只有通过类的公共接口（即公共方法）才能访问。这样做的好处是可以控制对类数据的访问和修改，可以防止类的用户直接访问类的内部状态并可能导致错误的状态。

  访问控制和封装一起工作，提供了一种强大的机制，可以隐藏类的内部实现，保护数据，并确保对象始终保持有效的状态。通过限制对类内部的直接访问（访问控制），并提供一个接口来操作数据（封装），可以确保数据的完整性，使代码更安全，更容易理解和维护。

- 友元

  可以声明类、类成员函数或全局函数为友元，让它们能够访问另一个类的私有或保护成员

  对于重载函数，声明友元时必须分别声明

  1. 声明类为友元
  
  ```c++
  class FriendClass; // 前向声明
  
  class MyClass {
  private:
      int myValue;
  
  public:
      MyClass(int value) : myValue(value) {}
  
      friend class FriendClass; // 声明 FriendClass 为友元
  };
  
  class FriendClass {
  public:
      void accessMyClass(const MyClass& obj) {
          std::cout << "Accessing MyClass private member: " << obj.myValue << std::endl;
      }
  };
  ```

  `FriendClass` 是前向声明，因为在 `MyClass` 的定义中，编译器需要知道 `FriendClass` 是一个类的名字。

  2. 声明类的成员函数为友元
  
  ```c++
  class OtherClass {
  public:
      void accessMyClass(const MyClass& obj);
  };
  
  class MyClass {
  private:
      int myValue;
  
  public:
      MyClass(int value) : myValue(value) {}
  
      // 声明 OtherClass 的成员函数 accessMyClass 为友元
      friend void OtherClass::accessMyClass(const MyClass& obj);
  };
  
  void OtherClass::accessMyClass(const MyClass& obj) {
      std::cout << "Accessing MyClass private member: " << obj.myValue << std::endl;
  }
  ```

  3. 声明函数为友元
  
  ```c++
  class MyClass {
  private:
      int myValue;
  
  public:
      MyClass(int value) : myValue(value) {}
  
      friend void accessMyClass(const MyClass& obj); // 声明函数为友元
  };
  
  void accessMyClass(const MyClass& obj) {
      std::cout << "Accessing MyClass private member: " << obj.myValue << std::endl;
  }
  ```

  在这个例子中，函数 `accessMyClass` 是 `MyClass` 的友元，所以 `accessMyClass` 可以访问 `MyClass` 的私有成员 `myValue`。
  



## 隐式类类型转换

- 用途：

  - 某些函数要求传入一个对象，但如果我们传入一个参数，编译器会隐式调用该类的构造函数，使用该参数初始化类，然后再将其传入函数。

	```c++
    class MyString {
    public:
        // 转换构造函数
        MyString(const char* str) {
            //...实现
        }
    };

    // 使用方式
    MyString str = "Hello";  // 实质上是 MyString str = MyString("Hello"); 这里隐式调用MyString构造函数，把"Hello"传入构造函数，生成临时对象。
  ```

- 限制：

  - 只能执行一步类型转换

- explicit

  `explicit`是C++中的一个关键字，主要用于阻止编译器进行不希望发生的隐式类型转换。`explicit`关键字可以用于修饰类的构造函数或转换操作符。
  
  - 设计目的
  
    当我们定义了一个带有一个参数的构造函数，或者定义了一个转换操作符，C++编译器将允许我们在需要的地方隐式地进行类型转换。然而，这种隐式转换可能会导致一些意想不到的结果，尤其是在复杂的表达式中。通过在构造函数或转换操作符前添加`explicit`关键字，我们可以禁止这种隐式转换。
  
  - 作用
  
    - 对于构造函数：当构造函数被声明为`explicit`时，它**不能**被用于**隐式转换**和**拷贝初始化**，只能用**直接初始化**。也可以**显式转换**。
  
  - 例
  
    ```c++
    class MyInt {
    public:
        explicit MyInt(int x) : data(x) {}  // 声明为explicit
    private:
        int data;
    };
    
    void Func(MyInt mi) {
        // 对mi执行某些操作
    }
    
    int main() {
        MyInt mi = 3;  // 错误，不允许隐式转换
        MyInt mi(3);  // 正确，直接初始化
        Func(3);  // 错误，不允许隐式转换
        Func(MyInt(3));  // 正确，显式转换
    }
    ```



## 聚合类

- 在C++中，聚合（Aggregation）是一种特殊的类，它满足以下特性：

  1. 所有成员都是公开的（public）。

  2. 没有定义用户自定义的构造函数。

  3. 没有类内初始化（C++14以前）。

  4. 没有基类或虚函数。

例

```c++
struct Aggregate {
    int a;
    double b;
    char c;
};
```

可以使用花括号初始化聚合类：

```
Aggregate aggr = {10, 20.0, 'c'};
```

这就是所谓的聚合初始化，也称为花括号初始化或列表初始化。

- **注意事项：**

  1. **成员的初始化顺序**：在聚合初始化中，值的顺序应与声明成员的顺序相匹配。例如，`Aggregate aggr = {10, 20.0, 'c'};` 中，`10` 将初始化 `a`，`20.0` 将初始化 `b`，`'c'` 将初始化 `c`。

  2. **越界初始化**：如果在初始化时给出的值多于成员变量，那么编译器将会报错。例如，`Aggregate aggr = {10, 20.0, 'c', 30};` 将导致编译错误，因为 `Aggregate` 只有三个成员。

  3. **未完全初始化**：如果给出的值少于成员变量，那么剩下的成员将被默认初始化。例如，`Aggregate aggr = {10, 20.0};` 在这种情况下，`c` 将被初始化为0。

  4. **聚合类中的聚合类**：如果聚合类的成员是另一个聚合类，可以使用嵌套花括号进行初始化。例如：

  ```c++
  struct NestedAggregate {
      Aggregate aggr;
      int d;
  };
  
  NestedAggregate nested = {{10, 20.0, 'c'}, 30};
  ```

## 字面值常量类

## 静态成员

在C++中，静态成员是类的一部分，但它们不是类实例的一部分。这意味着，不论类创建了多少个实例，静态成员只有一份拷贝。静态成员可以是静态成员变量，也可以是静态成员函数。

- **静态成员变量**：

  静态成员变量在所有对象中是共享的。不论你创建了多少个类的对象，静态成员变量只有一份拷贝。

  静态成员变量需要在类外进行初始化，只有一次初始化。

  ```c++
  class MyClass {
  public:
      static int myStaticVar;
  };
  
  // Initialize static member variable
  int MyClass::myStaticVar = 0;
  ```

- **静态成员函数**：

  静态成员函数也是类的一部分，但是它们可以在不创建类的对象的情况下被调用。静态成员函数只能访问静态成员变量，它们不能访问类的非静态成员变量。但是普通成员函数可以访问静态成员变量。

  ```c++
  class MyClass {
  public:
      static int myStaticVar;
      static void myStaticFunction() {
          ++myStaticVar;
      }
  };
  
  // Initialize static member variable
  int MyClass::myStaticVar = 0;
  
  int main() {
      // Call static member function
      MyClass::myStaticFunction();
      
      return 0;
  }
  ```

- **注意事项**

  		- 静态成员变量只被初始化一次，然后在整个程序执行过程中都存在。

  			-	静态成员函数只能访问静态成员变量或其他静态成员函数，它们不能访问类的非静态成员。

  - 无论创建多少个类的实例，静态成员都只有一个副本。

  - 静态成员变量在类外部初始化，但仍需要在类内部声明。
