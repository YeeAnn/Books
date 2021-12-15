在书店程序之前的代码都是基础型
========================================

# 书店程序
要求
1. 调用一个名为isbn的函数从Sales_item对象中提取ISBN书号；
2. 使用`>>`和`<<`对Sales_item对象进行读写；
3. 使用`=`将一个Sales_item对象的值赋予另一个Sales_item对象；
4. 使用`+`将两个Sales_item对象相加。两个对象必须表示同一本书（ISBN相同），加法的结果是一个新的Sales_item对象，其ISBN与两个运算对象相同，而总销售额和售出册数则是两个运算对象的对应值之和。
5. 使用`+=`将一个Sales_item对象加到两一个对象上
```C++
#include <iostream>
#include <string>

class Sales_item {

friend std::istream& operator>>(std::istream&, Sales_item&);
friend std::ostream& operator<<(std::ostream&, const Sales_item&);
friend Sales_item operator+(const Sales_item& lhs, const Sales_item& rhs);


public:
   
	Sales_item():revenue(0.0), num(0){}
	 Sales_item(const std::string &book) : bookNo(book), num(0), revenue(0.0) { }

public:
    Sales_item& operator+=(const Sales_item&);
    std::string isbn() const { return bookNo; }


private:
    std::string bookNo;
	double revenue;
	unsigned int  num;
};


Sales_item operator+(const Sales_item& lhs, const Sales_item& rhs) 
{
    Sales_item ret(lhs);  
    ret += rhs;           
    return ret;           
}

std::istream& operator>>(std::istream& in, Sales_item& s)
{
   in >> s.bookNo ;
   return in;
}

std::ostream& 
operator<<(std::ostream& out, const Sales_item& s)
{
    out << s.isbn();
    return out;
}

Sales_item& Sales_item::operator+=(const Sales_item& rhs)
{
	revenue += rhs.revenue;
	num += rhs.num;
	return *this;
}
```

小Tips：在编写`=`号的重载，要求`=`必须作为成员函数而存在
