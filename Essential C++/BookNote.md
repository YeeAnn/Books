
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
    eg: int *pi = new int[24]
        delete [] pi
