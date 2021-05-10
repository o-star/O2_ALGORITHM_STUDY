# 1941. 소문난 칠공주

## 작성자 : Hwa-ning

<br/>

### <풀이>

1. S(이다솜 무리)를 pair<int, int>의 vector에 push_back 해준다.
2. DFS를 활용하여 pickSom함수로 1의 vector에서 이다솜 무리(S)를 4명이상 선택한다. 이 때, 이들이 선택됐는지 확인하는 bool 배열 selected를 사용한다.
3. 선택된 이다솜 무리들과 solve함수에서 임도연 무리(Y)를 선택하여 이다솜 무리(S)로 만들어준다
4. 이다솜 무리(S)가 7명이 된다면 BFS를 활용한 check함수로 이들이 가로 세로로 인접하는지 확인한다. cnt변수가 7이 된다면 가로 세로로 인접하므로 res++
   <br/>

### 주의

조합도 조합이지만 포인터로 선언한 bool 배열 selected가 memset으로 똑바로 초기화되지않아서 시간을 많이 잡아먹었다.
정확한 이유를 모르겠으나 알아보고 앞으로는 memset으로 초기화시 유의해야겠다.

### <코드>

```C++
#include <iostream>
#include <cstring>
#include <vector>
#include <queue>

using namespace std;

vector<pair<int, int> >v;
string student[5];
bool* selected;
int res = 0;

int dx[4] = { 0,0,1,-1 };
int dy[4] = { 1,-1,0,0 };
void IsConnect(string s[])
{
	bool visited[5][5];
	memset(visited, false, sizeof(visited));
	int qx, qy;
	for (int i = 0; i < 5; i++)
		for (int j = 0; j < 5; j++)
			if (s[i][j] == 'S')
			{
				qy = i;
				qx = j;
				break;
			}
	int cnt = 1;
	queue<pair<int, int> >q;
	q.push({ qx,qy });
	visited[qy][qx] = true;
	while (!q.empty()) {
		int x = q.front().first;
		int y = q.front().second;
		q.pop();

		for (int i = 0; i < 4; i++) {
			int nx = x + dx[i];
			int ny = y + dy[i];

			if (0 > nx || nx >= 5 || 0 > ny || ny >= 5 || visited[ny][nx] || s[ny][nx] == 'Y')
				continue;

			visited[ny][nx] = true;
			q.push({ nx,ny });
			cnt++;
		}
	}
	if (cnt == 7)
		res++;
}
void Solve(int y, int x, int cnt, string s[])
{
	if (cnt == 7)
		IsConnect(s);
	else if (cnt > 7)
		return;

	for (int i = y; i < 5; i++) {
		for (int j = (i == y ? x : 0); j < 5; j++) {
			if (s[i][j] == 'Y' && student[i][j] == 'Y') {
				s[i][j] = 'S';
				Solve(i, j, cnt+1, s);
				s[i][j] = 'Y';
			}
		}
	}
}
void PickSom(int idx, int select)
{
	if (select >= 4) {
		string temp[5];
		for (int i = 0; i < 5; i++)
			temp[i] = "YYYYY";

		int cnt = 0;
		for (int i = 0; i < v.size(); i++) {
			if (selected[i]) {
				int x = v[i].first;
				int y = v[i].second;
				temp[y][x] = 'S';
				cnt++;
			}
		}
		Solve(0, 0, cnt, temp);
	}
	for (int i = idx; i < v.size(); i++){
		if (!selected[i]) {
			selected[i] = true;
			PickSom(i + 1, select + 1);
			selected[i] = false;
		}
	}
}
int main()
{
	for (int i = 0; i < 5; i++) {
		cin >> student[i];
		for (int j = 0; j < 5; j++)
			if (student[i][j] == 'S')
				v.push_back({j,i});
	}
	selected = new bool[v.size()];
	for (int i = 0; i < v.size(); i++) {
		selected[i] = false;
	}
	PickSom(0,0);
	cout << res << endl;
}
```
