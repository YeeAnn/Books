
# chap2 面向过程的编程风格
1. pass by reference
  - 定义
  ```C++
  int ival = 1024;
  int *pi = &ival; //point
  int &rval = ival;//reference
  ```
  - 将参数声明为引用的好处：
    - 得以直接对所传入的对象进行修改
    - 降低复制大型对象的额外负担
  - 和指针的区别：
    - 指针需要做安全性检查
 
 2. 动态内存管理：
  - 从堆上分配内存；
  - 使用news申请内存，使用delete进行内存释放
    `eg: int *pi = new int[24];`
        `delete [] pi`

3. 提供默认参数值：
  可以为函数形参提供默认参数值
  
4. inline函数和函数重载

5. 函数模板：
```C++
template <typename elemType>
void Dis_message(const string& msg, const vector<elemType> &vec)
{
  cout << msg;
  for(int ix = 0; ix < vec.size(); ++ix)
  {
    elemType t = vec[ix];
    cout << t << ' ';
  }
}
```

6. 函数指针

7. 头文件：
  - 使用双引号：
    头文件和包含此头文件的程序代码文件位于同一个磁盘目录下
  - 使用尖括号：
    不在同一个目录下或者是认为是项目专属或者是标准的头文件


# chap3 泛型编程风格
1. STL主要由： 容器和泛型算法构成
  - 容器： vector, list, set, map
  - 泛型算法： find(), sort(), replace(), merge()等
2. vector和array：
  - vector可以是空的，array不可以
  - vector和array都存在连续的内存上
  - 尝试设计适用于vector和array的泛型find()函数，并将之拓展到list类型
3. Iterator泛型指针
  - 每个标准容器都提供有一个名为begin()的操作函数，可返回一个Iterator,指向第一个元素
  - 提供一个名为end()的操作函数，返回一个Iterator,只想最后一个元素的下一个位置
  - 可对Iterator进行赋值(assign)、比较(compare)、递增(increment)、提领(dereference)操作
4. 所有容器都提供了四个函数：
  - begin()
  - end()
  - erase()
  - insert()
5. 容器类的共通操作：
  - equality() 和 ineauality()，返回值是true或者false
  - assignment()，将某个容器赋值给另一个容器
  - empty()容器中无任何元素时返回true
  - size()返回容器内目前持有的元素的个数
  - clear()删除容器中的所有的元素
6. 使用顺序型容器：
  vector, deque, list
  - vector, deque: 都是连续内存存储元素；
  - list中每个元素包含三个字段：value, front指针，back指针
  - 顺序容器的五种定义方式：
    - 产生空的容器：
  `list<string slist>`
  `vctor<int> ivec`
    - 产生特定大小的容器，每个元素都以其默认值作为初值：
  `list<int> ilist(1024)`
  `vector<string> svec(32)`
    - 产生特定大小的容器，为每个容器指定初始值
  `vector<int> ivec(32, -1);`
    - 通过一对iterator产生容器，
7. 使用Map
 - 使用map_test[key]的方式，如果key存在于map中，将会取出value,否则，会将key加入到这个map中
 - insert
 	- `iterator map_name.insert({key, element})\\初始化列表`
	- `iterator map_name.insert(it_begin, it_end)\\一对迭代器`
 - map的迭代器指针对应的first元素为key,second元素为value
 - 查询某个map中是否存在key有三种方式：
	- 将key当成索引使用：缺点是当key不存在于这个map中的时候，key也会被自动加入map中，相应的key会被设置默认属性
	- 使用map的find()函数，如果key存在其中，会返回一个iterator,指向key/value形成的一个pair，反之返回end()
	- 使用map的count()函数，cout()会返回某固定项在map内的个数
8. 使用Set
 - set由一群key组合而成，默认情形下，set皆依据其所属元素类型进行less-that排序存放
 - set中插入元素可使用inser()的一个参数形式，或者两个参数形式将某一个范围内的元素插入set
 - set中查找元素的方法，可使用count
 - find()函数,如果没有找到，返回end()
 - insert类似与map，关联容器的插入操作都是相似的
 
9. Iterator Inserter
- back_inserter()会以容器的push_back()函数取代assignment运算符。传入back_inserter的参数，应该是容器本身
- inserter()会以容器的insert()来取代assignment运算符。接受两个参数，一个是容器，另一个是iterator,指向容器内的插入操作起点
- front_inserter()会以容器的push_front()函数取代assignment运算符

# chap4 基于对象的编程风格
1. class的定义：
- 主体部分由一对大括号括住，以分号结尾；
- 主体内的两个关键字`private`和`public`
2. 构造函数
- 成员初始化列表：紧跟在参数列表之后的冒号里面，以逗号分隔，欲赋值的源数据放在变量后面的小括号中
- 可重载
- 拷贝构造函数：成员需要逐一初始化
3. 析构函数
- 以~开头，无返回值，无参数列表
4. mutable和const
 - const member function
  有些时候希望函数调用不会更改变量的值，除了将变量的值声明为const类型之外，还有一种保险方式是将调用这个变量的相关函数声明为const类型。即在member function上标注const，告诉编译器，这个member function不会更改class object的内容。
  - 语法： const修饰符紧跟在函数参数列表之后
  - 声明和定义：如果不是在类中实现，那么在声明和定义中需要同时制定const
  - const member function函数返回一个非const引用或者变量，在语法层面上是存在问题的，这种时候，需要对这种情况下的函数进行重载，因为函数重载可以根据const或者非const进行区分
  - mutale
  
  -this指针
  
  ## 4.1 静态类成员
- 类数据成员
	- 表示唯一的、共享的member
	- 需要在代码文件中提供其清楚的定义，定义时可以给定初值或者不给初值
	- 也可以在定义的时候直接给定初值
- 类成员函数
	- 在不访问任何non-static member的条件下能够被声明为static
        - 在类的主体之外定义静态数据成员和函数的时候，无需重复加上static关键字
- 运算符重载
 - operator
- 友元
	- 任何class都可以将其他的function或者class指定为friend，所谓friend，具备了与class member function相同的访问权限，可以访问class的private member
	- 声明时可以放在类中任意地方，不受private或者public等修饰符影响

# chap5 面向对象编程风格
## 1. C++的几大特征
- 1.继承
	父类定义了子类所有的共有接口和父类自己的私有实现。每个子类可以增加或者继承而来的东西，实现其自身独特的行为。
- 2.多态
	让基类的指针或者引用十分透明的指向任何一个派生类的对象。
- 3.动态绑定
	只有在程序运行的时候才会决定调用父类还是子类的函数

## 2. 虚析构函数
- 1. 当程序定义一个派生对象，基类和派生类的constructor都会被执行。当派生类对象被销毁的时候，他们的destructor也都会被执行，但是顺序相反。
- 2. 设计的程序里，释放实例对象的时候，如果有【使用某个基类的指针，来释放它指向的继承类的实例】这种用法的话，那么，这个基类的destructor应该设计成virtual的。
	- 如果是一个继承树，只要在树根基类上声明虚析构函数就好。剩下的类自然继承虚析构函数。凡是基类定义了虚函数，应该将destructor声明为virtual。
	- Essential C++ 中表明，凡是基类中定义一个（或者）多个虚函数，应该将其destructor声明为virtual。
```C++
class A
{
public:
  A(int size): m_a_size(size)
  {
    m_a = new int[size];
  }
  ~A();
private:
  int *m_a;
  int m_a_size;
};

A::~A()
{
    cout << "delete m_a" << endl;
    delete []m_a;
}

class B: public A
{
public:
  B(int a_size, int b_size): A(a_size), m_b_size(b_size)
  {
    m_b = new int[b_size];
  }
  ~B();
private:
  int *m_b;
  int m_b_size;
};

B::~B()
{
    cout << "delete m_b" << endl;
    delete []m_b;
}
int main()
{
  A* a = new B(5,10);
  delete a;
  return 0;
}

作者：sethbrin
链接：https://www.zhihu.com/question/41538182/answer/91407683
来源：知乎
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```
运行结果：
`delete m_a`
虚构函数添加virtual 运行结果:
`delete m_b
delete m_a`

## 3. 定义一个派生类
- 1. 如果派生类的函数没有在父类中拥有接口，那么使用父类的引用或者指针进行派生类的操作时，无法访问到派生类中的相关函数的。
- 2. 函数重写：派生类中的函数和基类中的函数名称完全一致，且基类没有声明为virtual。在派生类这个类的任何位置使用这个函数使用的都是派生类本身的函数，如果派生类需要使用基类中的这个函数，需要加class scope限定符加以限定。

## 4. 运用继承体系
- 虚函数的静态解析：
有两种情况，虚函数机制不会出现预期的行为：  
1. 在基类的constructor或者destructor内调用虚函数：  
基类的constructor/destructor中，派生类的虚函数绝对不会被调用。  
2.  当我们使用的是基类的对象：  
即使给基类的对象赋值了一个派生类对象，这个基类对象也只会拷贝派生类对象中的部分内容，不会全部拷贝。

- 运行时的类型鉴定机制：  
	- typeid运算符  
	1. 包含头文件：<typeinfo>  
	2. typeid运算符返回一个type_info对象，对象中存储着与对象类型相关的种种信息，对象的name()函数会返回一个const char*，用以表示类名  
	3. 使用方法实例：  
`typeid(*this).name()`
	- 基类指针指向子类对象：  
	1. 实现多态的调用时可以直接这么使用；  
	2. 如果使用这个指针调用子类的protected函数，或者父类中没有定义的函数，需要将父类指针使用智能指针转换为子类指针。  
	3. 实例：  
	



























