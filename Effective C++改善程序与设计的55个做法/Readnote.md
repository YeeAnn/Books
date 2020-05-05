# 1. C++ 为一个语言联邦
- C语言
- Object-Oriented C++: C with classes: 包含构造函数，析构函数，封装，继承，多态，虚函数等等
- Template C++，泛型编程
- STL: 

# 2. 尽量以const, enum, inline替换#define
原因： #define可能没有被编译器看见，然后将变量替换进代码中，导致追踪十分复杂 
