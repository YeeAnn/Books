
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
    ```C++
    #include <iostream>
#include <fstream>
#include <set>
#include <string>
#include <map>

using namespace std;

void initialize_exclusion_set(set<string>&);
void process_file(map<string, int>&, const set<string>&, ifstream&);
void user_query(const map<string, int>&);
void display_word_count(const map<string, int>&, ofstream &);

int main()
{
	ifstream in_file("input_file.txt");
	ofstream out_file("output_file.map");
	if (!in_file || !out_file)
	{
		cerr << "fiele exsitence error" << endl;
		return -1;
	}

	set<string> exclude_set;
	initialize_exclusion_set(exclude_set);

	map<string, int> word_count;
	process_file(word_count, exclude_set, in_file);
	user_query(word_count);
	display_word_count(word_count, out_file);

	return 0;
}

void initialize_exclusion_set(set<string>& exs)
{
	static string _excluded_words[3] = {"a", "an", "the"};
	exs.insert(_excluded_words, _excluded_words+3);
}

void process_file(map<string, int>& word_cout, const set<string>& exclude_set, ifstream& infile)
{
	string word;
	while (infile >> word)
	{
		if (exclude_set.count(word))
		{
			continue;
		}
		word_cout[word] += 1;
	}
}

void user_query(const map<string, int>& word_count)
{
	string search_word;
	cout << "Enter your word: " << endl;
	cin >> search_word;
	while (search_word.size() && search_word != "q")
	{
		map<string, int>::const_iterator it;
		if ((it = word_count.find(search_word)) != (word_count.end()))
		{
			cout << "Yes.[" << it->first << ", " << it->second << "]" << endl;
		}
		else
		{
			cout << "No. press q to exit." << endl;
		}
		cin >> search_word;
	}
}

void display_word_count(const map<string, int>& word_count, ofstream & outfile)
{
	map<string, int>::const_iterator it = word_count.begin(), end_it = word_count.end();
	while (it != end_it)
	{
		outfile << "[" << it->first << ", " << it->second << "]" << endl;
		++it;
	}
	outfile << endl;
}

    ```
  





































