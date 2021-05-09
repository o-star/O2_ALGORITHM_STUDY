# 5052. 전화번호 목록

## 작성자 : Hwa-ning

<br/>

### <풀이>

트라이를 사용하였다. 트라이 구조체를 선언하여 탐색을 하면서 삽입하도록 코드를 구현했다.

### <주의>

처음 25%에서 틀렸습니다가 나와서 잘 이해하지 못했었는데 내가 구현한 코드상 삽입하면서 찾는 과정을 진행하기 때문에<br>
[testcase_1] <br>
1 <br>
2 <br>
9999 <br>
9 <br>
[testcase_1]의 경우 9999, 9의 순서로 삽입하는 경우에 삽입하면서 탐색을 진행하기 때문에 prefix가 존재하는지 알지 못한다. 따라서, vector<string>을 선언한 후 vector에
입력값들을 저장 한 후 sort하여 9를 삽입한 후 9999를 삽입하도록 순서를 바꿔준 후 삽입하도록 구현했다.
<br> +성공 후 메모리 사용이 제법 크길래 소멸자를 선언해줘서 메모리 사용을 줄여줬다.

### <코드>

```C++
#include <iostream>
#include <vector>
#include <algorithm>

using namespace std;

struct Trie {
	bool lastNum;
	Trie* nextNum[10];

	Trie() {
		this->lastNum = false;
		for (int i = 0; i < 10; i++)
			this->nextNum[i] = NULL;
	}
	~Trie() {
		for (int i = 0; i < 10; i++)
			if (this->nextNum[i] != NULL)
				delete nextNum[i];
	}
};

int main()
{
	int test_case;
	cin >> test_case;
	int N;
	for (int t = 0; t < test_case; t++) {
		Trie *root = new Trie();
		vector<string>v;
		cin >> N;
		string input;
		for (int i = 0; i < N; i++) {
			cin >> input;
			v.push_back(input);
		}
		sort(v.begin(), v.end());
		bool Exit = false;
		for (int i = 0; i < N; i++) {
			Trie* now = root;
			for (int j = 0; j < v[i].size(); j++) {
				if (now->lastNum)
					Exit = true;
				else if(now->nextNum[v[i][j]-'0']==NULL){
					Trie* newTrie = new Trie();
					now->nextNum[v[i][j] - '0'] = newTrie;
					now = newTrie;
				}
				else
					now = now->nextNum[v[i][j]-'0'];
			}
			now->lastNum = true;
		}
		if (Exit)
			cout << "NO" << endl;
		else
			cout << "YES" << endl;

		delete root;
	}
	return 0;
}
```
