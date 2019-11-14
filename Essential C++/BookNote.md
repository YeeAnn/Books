
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
