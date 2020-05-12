# 1. C++ 为一个语言联邦
- C语言
- Object-Oriented C++: C with classes: 包含构造函数，析构函数，封装，继承，多态，虚函数等等
- Template C++，泛型编程
- STL: 

# 2. 尽量以const, enum, inline替换#define
原因： #define可能没有被编译器看见，然后将变量替换进代码中，导致追踪十分复杂 **原因不是很清楚,为什么可能会看不到**
- 对于单纯的变量，最好以const对象或者enums替换#defines
- 对于形似函数的宏，最好改用inline函数替换#defines
```C++
#define CALL_WITH_MAX (a, b) f((a) > (b) ? (a) : (b))

//使用inline改写成：
template<typename T>
inline void CallWithMax(const T& a, const T& b)
{
  f(a > b ? a : b);
}
```

# 3. 尽可能使用const 
如果const在`*`左边，表示常量本身，如果在`*`右边，表示指针自身是常量。而在第一种情况下，有人习惯将`*`放在类型前面，有人习惯放在后面，都是可以的。  
- const成员函数：
  - 使得class接口比较容易被理解，可以明确知道哪个函数可以更改对象内容，哪个不可以
  - 操作const对象成为可能  
在同一个类中，如果两个函数只是常量性不同，其实是属于两个重载的函数的。  
当const和non-const成员函数有着实质等价的实现时，令non-const版本调用const版本可避免代码重复,**不可以反过来使用**。
```C++
class TextBook
{
    public:
    	...
        const char& operator[](std::size_t pos) const
        {
            //边界检查
            //志记数据访问
            //检验数据完整性
            return text[pos];
        }
    
    	char& operator[](std::size_t pos)
        {
            return const_cast<char&>(static_cast<const TextBook&>(*this)[pos]);
        }
};
```

# 4. 确定对象在使用前已经被初始化
- 为内置对象进行手工初始化，因为C++不保证初始化他们
- 构造函数最好使用成员初始化列表，而不是在构造函数本体中使用赋值操作。初值列列出来的成员变量，其排列次序应该和他们在class中定义的次序一致
可使用成员初始化列表实现变量的初始化
- 为避免“跨编译单元之初始化次序”的问题，请以Local static对象替换non-local static对象   **其中涉及的单例模式和和线程安全的问题不是很清晰**

# 5. 了解C++默默编写并调用了哪些函数
编译器可以为class创建default构造函数、copy构造函数、copy assignment操作符、析构函数。  
至于copy构造函数和copy assignment操作符，编译器创建的版本只是单纯的将来源对象的每一个non-static成员变量拷贝到目标对象。 **static的成员变量是如何处理的**  
如果打算在内含reference 成员或者const成员的class内支持赋值操作，需要手动实现这个功能。   C++不允许reference改变成指向不同的对象

# 6. 若不想使用编译器自动生成的函数，就应该明确拒绝
为驳回编译器自动提供的机能。可将相应的成员函数声明为private并且不予实现。使用像uncopyable这样的base class也是一种做法。

# 7. 为多态基类声明virtual析构函数
polymorphic（带多态性质的）base class 应该声明一个virtual析构函数，如果class带有任何的virtual函数，它就应该拥有一个virtual析构函数。  
classes的设计目的如果不是为了作为base class使用，或者不是为了具备多态性（polymorphically）,就不应该声明virtual析构函数。（*因为会增加对象的体积大小*）  
虚函数表：属于类。不同的编译环境存放的位置不同  
虚指针：属于每个对象，指向虚函数表  
参考链接： https://blog.csdn.net/qq_28584889/article/details/88756022

# 8. 别让异常逃离析构函数
1. 析构函数绝对不要吐出异常。如果一个被析构函数调用的函数函数可能抛出异常，析构函数应该捕捉任何异常，然后吞下（不传播）它们或者结束程序。  
2. 如果客户需要对某个操作函数运行期间抛出的异常做出反应，那么class应该提供一个普通函数（而非在析构函数中）执行该操作。

# 9. 绝不在构造和析构过程中调用virtual函数
在构造和析构函数中不要调用virtual函数，因为这类调用从不下降至derived class（比起当前执行构造函数和析构函数那层），这样会造成意想不到的结果。

# 10. 令operator=返回一个 reference to `*this`
令赋值操作符返回一个reference to `*this`,以保证赋值运算符的传递性
```C++
class Wight
{
public:
    Wight& operator=(const Wight& rh)
    {
        ......
        return *this;
    }
	Wight& operator=(int rhs) 
    {
        ....
        return *this;
    }
    
    
};
```




