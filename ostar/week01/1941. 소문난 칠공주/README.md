# 1941. 소문난 칠공주

###### 작성자 : O-star

<br/>

<br/>

#### 풀이 : 

**음.. 우선 사실 간단한 백트래킹 DFS 문제라고 생각하고 접근했으나 나에겐 그리 쉽지 않은 문제였다.** 

**처음에는 DFS로 경우의 수를 찾는다고 생각했기에 중복되는 경우의 수(즉, 본 문제는 조합을 사용해 경우의 수를 구해주어야 함) 어떻게 체크해주어야할지 전혀 감이 오지 않았따.**

**결국 풀이를 보고 간단하게 힌트를 얻은 후 문제를 해결할 수 있었다.**

**문제는 분명 간단한 그래프 탐색, 이론 문제보다 훨씬 많은 생각을 하게 해주는 문제인 듯하다.**

1. **우선 본 문제는 조합문제이기 때문에 단순히 처음부터 그래프를 탐색해 나가선 안된다. 왜냐하면 단순히 그래프를 탐색해 나가게 되면 조합으로 경우의 수를 밝힐 수 없다. 따라서 조합을 구현하는 재귀 함수를 사용해 가장 먼저 7명의 학생 칸을 선택해 준다. => 이 부분이 생각해내기 굉장히 어려웠던 것 같다. 그래프 탐색 전에 7명을 미리 택해둔 후 그 7명의 위치가 서로 인접해있는지 탐색은 뒤에 해줘야 하는 것이다.**
2. **조합 경우의 수를 찾는 재귀함수를 구현할 때에는 백트래킹을 사용해 효과적으로 불필요한 과정을 줄여야 한다. => selectS(현재까지 선택한 이다솜파 학생 수) + remains(아직 선택하지 않은 나머지 학생 수) < 4 일 경우 더 이상 해당 조합을 탐색해볼 이유가 없다.(4번 조건에서 7명의 학생 중 적어도 4명 이상이 이다솜파 학생이어야한다고 명시되어있음.)**
3. **이렇게 학생을 미리 선택해서 인접 유무를 판단하는 아이디어만 잘 생각해낸다면 구현에서는 크게 어려움이 없는 문제였던 것 같다. 조합을 사용하는 문제를 더 풀어볼 필요가 있는 듯 하다.**

<br/>

<br/>

#### 코드 : 

```c++
#include <iostream>
#include <queue>
#include <utility>

#define pii pair<int, int>

using namespace std;
char map[5][5];
bool selected[5][5];
int cntans, di[4] = {0, 1, 0, -1}, dj[4] = {1, 0, -1, 0};

bool isConnect(int stx, int sty) {
    bool visit[5][5]{false};
    int counts = 1;
    queue<pii > q;
    q.push(pii(stx, sty));
    visit[stx][sty] = true;

    while (!q.empty()) {
        int curx = q.front().first, cury = q.front().second;
        q.pop();

        for (int k = 0; k < 4; k++) {
            int cmpx = curx + di[k], cmpy = cury + dj[k];
            if (cmpx < 0 || cmpx >= 5 || cmpy < 0 || cmpy >= 5) continue;
            if (!selected[cmpx][cmpy] || visit[cmpx][cmpy]) continue;
            if (++counts == 7) return true;

            q.push(pii(cmpx, cmpy));
            visit[cmpx][cmpy] = true;
        }
    }

    return false;
}

void selectStu(int selectS, int remains, int stx, int sty) {
    if (selectS + remains < 4) return;
    if (remains <= 0) {
        if (isConnect(stx, sty - 1)) cntans++;
        return;
    }

    for (int i = stx; i < 5; i++) {
        while (sty < 5) {
            selected[i][sty] = true;

            if (map[i][sty] == 'S') selectStu(selectS + 1, remains - 1, i, sty + 1);
            else selectStu(selectS, remains - 1, i, sty + 1);

            selected[i][sty] = false;
            sty++;
        }
        sty = 0;
    }
}

int main() {
    for (int i = 0; i < 5; i++)
        for (int j = 0; j < 5; j++)
            cin >> map[i][j];

    selectStu(0, 7, 0, 0);
    cout << cntans << '\n';
    return 0;
}
```

