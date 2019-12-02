chap3 将文件中单词存入map,并剔除个别单词，显示一份单词清单，并支持用户查询：
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
