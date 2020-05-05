# 1. C++ 为一个语言联邦
- C语言
- Object-Oriented C++: C with classes: 包含构造函数，析构函数，封装，继承，多态，虚函数等等
- Template C++，泛型编程
- STL: 

# 2. 尽量以const, enum, inline替换#define
原因： #define可能没有被编译器看见，然后将变量替换进代码中，导致追踪十分复杂 *原因不是很清楚,为什么可能会看不到*
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
- 为避免“跨编译单元之初始化次序”的问题，请以Local static对象替换non-local static对象   *其中涉及的单例模式和和线程安全的问题不是很清晰*






