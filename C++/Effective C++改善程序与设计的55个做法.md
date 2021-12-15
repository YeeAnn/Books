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
编译器可以为class创建default构造函数、copy构造函数、copy assignment操作符(operator =)、析构函数。  
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
基类的析构函数定义成virtual的原因：https://blog.csdn.net/weicao1990/article/details/81911341

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
# 11.在operator=中处理“自我赋值”
确保当对象自我赋值时operator=有良好的行为。其中的技术包括比较“来源对象”和“目标对象”的地址，精心周到的语句顺序，以及copy-and-swap
- 比较“来源对象”和“目标对象”
```C++
Widget& Widget::operator=(const Widget& rhs)
{
	if(this == &rhs)
		return *this;  //证同测试，防止自己给自己赋值之前，删除了自己的对象
	delete pb;
	pb = new Bitmap(*rhs.pb); //但仍然存在异常安全问题
	return *this;
}
```
- “精心周到的语句”
```C++
Widget& Widget::operator=(const Widget& rhs)
{
	Bitmap *pOrigin = pb; //首先保存原来的pb指针
	pb = new Bitmap(*rhs.pb);//令pb指向*pb的一个复件
	delete pOrigin;       //删除原先的pb
	return *this;
}
```
确定任何函数如果操作一个以上的对象，而其中多个对象是同一个对象时，其行为仍然正确

# 12. 复制对象时勿忘其每一个成分
copying函数应该应该确保复制“对象内的所有成员变量”及所有的“base class”成分，在派生类中调用父类的拷贝函数：`Customer::oprator=(rhs)`  
不要尝试以某个copying函数实现另一个copying函数，应该将共同机能放进第三个函数中，并由两个copying函数共同调用  

# 13.以对象管理资源
工厂函数和自定义构造函数： https://juejin.im/post/5d42a3c95188255d3a0f08bb  
1. 为防止资源泄露，请使用RAII对象，它们在**构造函数中获取资源并在析构函数中释放资源**；  
2. 两个常被使用的RAII class分别是`shared_ptr`和`auto_ptr`,前者通常是比较佳的选择，因为其copy行为比较直观。若选择auto_ptr,复制动作会使它（被复制物）指向NULL。  
Tips: auto_ptr和shared_ptr在其析构函数中调用的都是delete而不是delete[],这意味着动态分配得到的array身上使用auto_ptr或者shared_ptr是个馊主意，虽然它可以通过编译。对于这种动态数组，可以使用boost::scoped_array和boost::shared_array   class

# 14.在资源管理类中小心copying的行为
1. 复制RAII对象必须一并复制它所管理的资源，所以资源的copying行为决定了RAII对象的copying行为  
2. 普遍而常见RAII class copying行为是：抑制copying,施行引用计数法。不过其它的行为也都可能被实现。


# 15. 在资源管理类中提供对原始资源的访问
APIs往往要求访问原始资源，所以每一个RAII class应该提供一个“取得其所管理资源”的办法   
对原始资源的访问可能经由显示转换或者隐式转换。一般而言，显示转换比较安全，但是隐式转换对客户比较方便。   
**并没有get到这个点**


# 16.成对使用new和delete时要采用相同的形式
如果在new表达式中使用[]，必须在相应的delete中也使用[]; 如果在new表达式中未使用[],一定不要在相应的delete表达式中使用[]

# 17.以独立语句将newd对象置入智能指针
Java和C#总是以特定的顺序完成函数参数的核算，但是C++没有这个特性。   
所以不要在形参中new一个对象，否则某些参数调用的时候抛出异常导致内存泄漏。即：以独立语句将new对象存储于（置入）智能指针内，如果不这样做，一旦异常被抛出，有可能导致难以察觉的资源泄露。

# 18. 让接口容易被正确使用，不易被误用
1. 好的接口很容易被正确使用，不容易被误用，你应该在你的所有接口中努力达成这些性质；  
2. “促进正确使用”的办法包括接口的一致性，以及与内置类型的行为兼容；   
3. “阻止误用”的办法包括建立新类型、限制类型上的操作，束缚对象值，以及消除客户的资源管理责任；
4. tr1::shared_ptr支持定制型删除器，这可防范DLL问题，可被用来自动解除互斥锁等等。  

# 19. 设计class犹如设计type
1. 新type的对象应该如何被创建和销毁  
2. 对象的初始化和对象的赋值该有什么样的差别   
3. 新type的对象如果被passed by value，意味着什么？（涉及到拷贝构造函数应该如何去实现）   
4. 什么是新type的合法值？  
5. 你的新type需要配合某些继承图系么？   
6. 你的新type需要什么样的转换?  
7. 什么样的操作符和函数对此新type而言是合理的？   
8. 什么样的标准函数应该驳回？   
9. 谁该采用新type的成员？   
10. 什么事新type的未声明接口？   
11. 你的新type有多一般化？   


# 20.宁以pass-by-reference-to-const替换pass-by-value
1. 尽量以pass-by-reference-to-const替换pass-by-value,前者通常比较高效，并可避免切割问题；  
2. 以上规则并不适用于内置类型，以及STL的迭代器和函数对象，对它们而言，pass-by-value往往比较适当。 

# 21.必须返回对象时，别妄想返回其reference
绝不要返回pointer或reference指向一个local stack对象，或者返回reference指向一个heap-allocated对象，或返回pointer或reference指向一个local static对象而有可能需要多个这样的对象。条款4已经为“在单线程环境中合理返回reference指向一个Local static对象”提供了一份设计实例。

# 22.将成员变量声明为private
1. 请切记将成员变量声明为private,这可赋予客户访问数据的一致性，可细微划分访问控制，允诺约束条件获得保证，并提供class作者以充分的弹性实现   
2. protected并不比public更具备封装性  

# 23.宁以non-member, non-friend替换member函数
宁可拿non-member, non-friend函数替换member函数，这样可以增加封装性，包裹弹性和机能扩充性。

# 24.若所有参数均需类型转换，请为此采用non-member函数
如果你需要为某个函数的所有参数（包括被this指针所指的那个隐喻参数）进行类型转换，那么这个函数必须是个non-member,其实也就是`operator*`的重载为什么要放在类外的原因
```C++
//outside class
const Rational oprator*(const Rational& lhs, const Rational& rhs)
{
	return Rational(lhs.numerator()*rhs.numerator(), lhs.denominator()*rhs.denominator());
}

//in class
class Rational
{
public:
	const Rational operator*(const Rational& rhs) const;
};
```

# 25. 考虑写一个不抛出异常的swap函数










