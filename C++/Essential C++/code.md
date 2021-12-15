chap3 
- 将文件中单词存入map,并剔除个别单词，显示一份单词清单，并支持用户查询：
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
- 从文件中读取所有的家庭信息，存入map，家庭姓氏为key，孩子集合为value，允许用户根据姓氏来查询一个家庭的成员情况，并在最后打印map中的所有内容。
```C++
#include <iostream>
#include <map>
#include <vector>
#include <string>
#include <fstream>

using namespace std;

typedef vector<string> vstring;
map<string, vstring> families;

void populate_map(ifstream& namefile, map<string, vstring>& families)
{
	string textline;
	while (getline(namefile, textline))
	{
		string fam_name;
		vector<string> child;
		string::size_type pos = 0, prev_pos = 0, textsize = textline.size();

		while ((pos = textline.find_first_of(' ', pos)) != string::npos)
		{
			string::size_type len = pos - prev_pos;
			if (! prev_pos)
			{
				fam_name = textline.substr(prev_pos, len);
			}
			else
			{
				child.push_back(textline.substr(prev_pos, len));
			}
			prev_pos = ++pos;
		}
		
		//no child
		if (prev_pos == 0)
		{
			fam_name = textline.substr(prev_pos, string::npos);
			if (!families.count(fam_name))
			{
				vstring nochild = {};
				families.insert({ fam_name, nochild});
			}
			
			continue;
		}
		
		//have child
		if (prev_pos < textsize)
			child.push_back(textline.substr(prev_pos, pos - prev_pos));
		
		if (!families.count(fam_name))
		{
			families[fam_name] = child;
		}
		else
		{
			cout << "family repeat." << endl;
		}
	}
}


void display_map(const map<string, vstring>& families, ostream& os)
{
	map<string, vstring>::const_iterator it = families.begin(), end_it = families.end();
	while (it != end_it)
	{
		os << "The " << it->first << " family: ";
		if (it->second.empty())
		{
			os << "no child" << endl;
		}
		else
		{
			os << "num: " << it->second.size() << ", ";
			vstring::const_iterator it_c = it->second.begin(), ite_c = it->second.end();
			while (it_c != ite_c)
			{
				cout << *it_c << ", ";
				++it_c;
			}
			cout << endl;
		}
		++it;
	}
}

void query_map(const map<string, vstring>&families, const string& family)
{
	map<string, vstring>::const_iterator it = families.find(family);
	if (it == families.end())
	{
		cout << "The " << family << " has no record." << endl;
		return;
	}
	cout << "The " << family << " has " << it->second.size() << " children: ";

	vector<string>::const_iterator it_c = it->second.begin(), ite_c = it->second.end();
	while (it_c != ite_c)
	{
		cout << *it_c << ", ";
		++it_c;
	}
	cout << endl;

}

int main()
{
	map<string, vstring> families;
	ifstream namefile("input_file.txt");
	if (!namefile)
	{
		cerr << "file not exist" << endl;
		return -1;
	}

	populate_map(namefile, families);
	string name;
	while (true)
	{
		cout << "enter family name: " << endl;
		cin >> name;
		if (name == "q")
		{
			break;
		}
		query_map(families, name);
	}

	display_map(families, cout);

	return 0;
}
```
