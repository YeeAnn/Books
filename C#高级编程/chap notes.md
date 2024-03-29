# 1.C# Books reference
 [C#编程指南中文版](https://docs.microsoft.com/zh-cn/dotnet/csharp/async)  

# 2.C# 基本语法 
## chpa2 核心C#
  1. C# 中变量未初始化会当成一个错误来对待
  - 变量位于类或结构中，未显示初始化，创建这些变量时，默认的值就是0
  - 局部变量必须显示初始化
  
  2. `var`
  编译器根据变量的初始化值推断变量的类型
  
  3. 值类型 & 引用类型
  - 值类型： 存储在堆栈中
  - 引用类型： 存储在托管堆上
  类似于C++的引用。C#中所有数据类型都以与平台无关的方式进行定义
  
  4.预定义的引用类型：
  - `object`类型
    - `object`类型是所有类的父类型
    - `object`类实现了许多一般用途的基本方法
    
  - `string`类型
    - 引用类型，**待验证，在vs2010上验证的并不是引用类型**
  
  5. `foreach`
  - `foreach(var in vararry)`

## chap3 对象和类型
1. 属性
2. 类 & 结构
  - 类
    - 引用类型
  - 结构
    - 值类型
3. 弱引用
4. 部分类
  `partial`，将`partial`放在`class`，`struct`，`interface`前面
5. 静态类

## chap4 继承
1.  C#不支持私有继承，因此不需要在基类名字前面加上`public`或者`private`

2.  虚函数：
  - 将某个基类函数声明为`virtual`，就可以在任何派生类中重写该函数
  - 在派生类中重写函数的时候，需要使用`override`关键字显示声明     **区别于C++的地方**

3.  隐藏方法：
  如果基类和派生类都声明了相同的方法，但是不是虚函数的使用方式，需要注意避免，有可能会造成错误。

4.  调用函数的基类版本：
  C#可使用`base.<MethodName>()`来调用基类的任何方法
  
5.  抽象函数：
  - `abstract`，抽象函数本身也是虚拟的，但是不需要添加`virtual`来修饰，否则会报错
  - 类中包含抽象函数，该类也必须声明为抽象的
  - 抽象类不能实例化，抽象函数不能直接实现

6.  密封类和密封方法：
  - `sealed`

7.  接口：
  - 接口成员总是公有的

8. 
| 修饰符      | 应用于     | 说明     |
| ---------- | :-----------:  | :-----------: |
| public     | 所有类型或成员     | 任何代码均可访问     |
| protected     |类型和内嵌类型的所有成员     | 只有派生的类型能访问该项     |
| internal     | 所有类型或成员     | 只能在包含它的程序集中访问该项     |
| private     | 类型或内嵌类型的所有成员     | 只能在它所属的类型中访问该项     |
| protected internal     | 类型或内嵌类型的所有成员     | 只能在包含它的程序集和派生类型的任何代码中访问该项     |

- 方法默认访问级别： private
- 类默认访问级别 : internal
- 类的成员默认访问修饰符为private; 构造函数默认为public访问修饰符
- 枚举类型成员默认为public访问修饰符，且不能显示使用修饰符
- 结构成员默认为private修饰符

## chap5 泛型
1. 与C++的区别：
  对于C++而言，模板在实例化的时候，需要模板的源代码，但是泛型在C#中是一种内置的结构
2. 泛型类型可以实现泛型接口，也可以派生自一个类。但要求必须要重复接口的泛型类型或者必须指定基类的类型：
```C#
public class Base<T>
{
}
public class Derived<T>:Base<T>
{
}

//class
public class Derived<T>:Base<string>
{
}
```
3. 静态成员：
 泛型类的静态成员只能在类的一个实例中共享
 
 ## chap6 数组
 1. Array 类
  (1)CreateInstance()创建数组
  (2)SetValue()\GetValue()
  (3)Clone()
    数组是引用类型，如果直接将两个数组赋值，将会得到两个引用。所以需要使用Clone将每个值进行赋值。如果数组中存储的是引用，使用Clone()也会将所有的引用赋值一遍。
  (4)Copy()
      Copy与Clone的区别在于，Copy必须要存在已知数组。
  (5)Sort()
  
2. 枚举
  (1)yield return:
    不需要重新开辟内存
  (2)foreach语句

3. 元组Tuple

## chap7 运算符和类型强制转换
1. checked & unchecked运算符：
  要不要执行类型检查
  
2. is 运算符：
  检查对象是否与特定的类型相兼容
  
3. as 运算符：
  用于执行引用类型的显示类型转换，如果类型兼容，则转换成功，如果类型不兼容，最后转换的结果为NULL
  
4. 可空类型：
  int?  bool?
  
5. 空合并运算符：
  ??
  
## chap8 委托，lambda表达式和事件
1. 委托
(1)定义：
  `delegate`： `delegate void IntMethodInvoker(int x)`
  
(2)使用：
  - 委托在语法上总是接受一个参数的构造函数，这个参数是委托引用的方法，且这个方法必须匹配最初定义委托时的签名。
  - 如果不使用构造函数的方式，还可以选择使用委托推断的方式来实例化一个委托
  - 给委托实例提供圆括号的与调用委托类的Invoke()方法完全相同
  - 使用委托的一种方式： 把方法整合到一组数组中，通过循环调用不同的方法
  - BeginInvoke()
     - Artical: Call a Method Asynchronously using Delegate BeginInvoke and EndInvoke Pattern
          - link: https://www.dotnetcurry.com/ShowArticle.aspx?ID=634
     - Artical: Call a Method Asynchronously in .NET using Polling Pattern and Callback Pattern      
          - link: https://www.dotnetcurry.com/ShowArticle.aspx?ID=642
2. Action<T>
  不带返回值的委托
3. Function<T>
  带返回值的委托

4. 多播委托

5. lambda表达式：
  - lambda表达式的左边列出了需要的参数，右边定义了赋予lambda变量的方法的实现代码
  - 如果只有一个参数，只写出参数名即可
  - 如果有多个参数，就把参数名放在花括号中(x, y)
  - 如果lambda表达式只有一条语句，在方法块内就不需要花括号和return语句
  - 如果lambda表达式有多条语句，必须添加花括号和return语句
  
6. 事件
  - 事件基于委托，为委托提供了一种发布\订阅机制，关键字： `event`
  - EventHandler<TEventArgs>:  事件委托，第一个参数必须是object类型，第二个参数是T类型，且T必须派生自基类EventArgs
  - 对于事件，可以使用add和remove关键字添加和删除委托的处理程序
  
## chap9 字符串和正则表达式

## chap10 集合
  数组和Array类在定义时大小是固定的，如果元素的个数是动态的，应该使用集合类。List<T>,队列，栈，链表，字典和集

## chap14 内存管理和指针
1. 垃圾回收器回收的资源：
2. 释放非托管的资源：
  - 非托管资源：例如文件句柄，网络连接，数据库链接等
  - 定义一个类时，可以使用两种机制来自动释放非托管的资源，通常这两种机制放在一起实现，因为每种机制为问题提供了不同的解决方法：
    - 声明一个析构函数，作为类的一个成员
    - 在类中实现System.IDisposable接口


## chap15 反射
反射是一个术语，描述了在运行过程中检查和处理程序元素的功能。
 反射允许完成以下的任务：
 - 枚举类型的成员
 - 实例化新对象
 - 执行对象的成员
 - 查找类型的信息
 - 查找程序集的信息
 - 检查应用于某种类型的自定义特性
 - 创建和编译新程序集
 
 
 ## chap16 错误和异常
 1. 一般将程序的相关部分分成3中不同类型的操作：
    - try: 程序的正常操作部分
    - catch: 包含代码处理的各种错误情况
    - finally: 代码资源清理或者需要在try和catch之后执行的操作。**无论是否抛出异常，都会执行finally块。如果在finally块中放置return语句，编译器会报错**

  
  ## chap17 异步编程
  ### 17.1 `Async`和`await`
  ref: https://zetcode.com/csharp/async-await/  
  [C#编程指南中文版](https://docs.microsoft.com/zh-cn/dotnet/csharp/async)  
  #### 概述
  1. `Async`会一直执行，直到遇见一个`await`运算符，遇见`await`运算符时，整个task会挂起等待`await`完成，整个call函数会返回并且继续工作。`Async`修饰符会向编译器发出信号，该方法中包含`await`语句。  也同样暗示编译器这是异步操作。
  2. 异步代码可用于IO绑定和CPU绑定，在每个方案中有所不同；
    2.1 如果代码需要等待某些内容，比如数据库中的数据，则为IO绑定；
    2.2 如果代码需要进行开销巨大的计算，则为CPU绑定；
  3. 异步代码使用`Task<T>`或者`Task`，它们是对后台所完成的工作建模的结构
  4. `async`关键字将方法转换成异步方法，使得在正文中可以使用`await`关键字
  5. 应用`await`关键字之后，将挂起被调用的方法，并将控制权还给调用方，等待直到任务完成
  6. 仅允许在异步方法中使用`await`关键字
  7. `await Task.WhenAll(Task1, Task2, Task3)`它将返回一个其参数列表中的所有任务都已完成时才完成的 Task  
  8. `await Task.WhenAny(TaskList)`当其中一个task完成了该函数都会返回，可以处理这个返回的结果，并将这个task从传给`WhenAll`的TaskList中删除
  9. 添加`Async`作为编写每个异步方法名称的后缀
  ```C#
  using System;
using System.Collections.Generic;
using System.Threading.Tasks;

namespace AsyncBreakfast
{
    class Program
    {
        static async Task Main(string[] args)
        {
            Coffee cup = PourCoffee();
            Console.WriteLine("coffee is ready");

            var eggsTask = FryEggsAsync(2);
            var baconTask = FryBaconAsync(3);
            var toastTask = MakeToastWithButterAndJamAsync(2);

            var breakfastTasks = new List<Task> { eggsTask, baconTask, toastTask };
            while (breakfastTasks.Count > 0)
            {
                Task finishedTask = await Task.WhenAny(breakfastTasks);
                if (finishedTask == eggsTask)
                {
                    Console.WriteLine("eggs are ready");
                }
                else if (finishedTask == baconTask)
                {
                    Console.WriteLine("bacon is ready");
                }
                else if (finishedTask == toastTask)
                {
                    Console.WriteLine("toast is ready");
                }
                breakfastTasks.Remove(finishedTask);
            }

            Juice oj = PourOJ();
            Console.WriteLine("oj is ready");
            Console.WriteLine("Breakfast is ready!");
        }

        static async Task<Toast> MakeToastWithButterAndJamAsync(int number)
        {
            var toast = await ToastBreadAsync(number);
            ApplyButter(toast);
            ApplyJam(toast);

            return toast;
        }

        private static Juice PourOJ()
        {
            Console.WriteLine("Pouring orange juice");
            return new Juice();
        }

        private static void ApplyJam(Toast toast) =>
            Console.WriteLine("Putting jam on the toast");

        private static void ApplyButter(Toast toast) =>
            Console.WriteLine("Putting butter on the toast");

        private static async Task<Toast> ToastBreadAsync(int slices)
        {
            for (int slice = 0; slice < slices; slice++)
            {
                Console.WriteLine("Putting a slice of bread in the toaster");
            }
            Console.WriteLine("Start toasting...");
            await Task.Delay(3000);
            Console.WriteLine("Remove toast from toaster");

            return new Toast();
        }

        private static async Task<Bacon> FryBaconAsync(int slices)
        {
            Console.WriteLine($"putting {slices} slices of bacon in the pan");
            Console.WriteLine("cooking first side of bacon...");
            await Task.Delay(3000);
            for (int slice = 0; slice < slices; slice++)
            {
                Console.WriteLine("flipping a slice of bacon");
            }
            Console.WriteLine("cooking the second side of bacon...");
            await Task.Delay(3000);
            Console.WriteLine("Put bacon on plate");

            return new Bacon();
        }

        private static async Task<Egg> FryEggsAsync(int howMany)
        {
            Console.WriteLine("Warming the egg pan...");
            await Task.Delay(3000);
            Console.WriteLine($"cracking {howMany} eggs");
            Console.WriteLine("cooking the eggs ...");
            await Task.Delay(3000);
            Console.WriteLine("Put eggs on plate");
            
            return new Egg();
        }

        private static Coffee PourCoffee()
        {
            Console.WriteLine("Pouring coffee");
            return new Coffee();
        }
    }
}
  ```
  ####  取消task
  cancel  
  CancelAfter
  
  ####  
  
  
