# 경주로 건설 [2020 카카오 인턴십]

###### 작성자 : O-star

<br/>

<br/>

#### 풀이 : 

쉬운 문제였는데 초반 접근이 잘못되는 바람에 생각보다 헤맨 문제였다.

초반에는 DFS를 사용하여 경로를 탐색하려 했다. 하지만 DFS의 경우는 중복 탐색 노드를 효과적으로 선별해낼 수 없기 때문에 시간초과가 떴다.

사실 본 문제는 직선 경로와 코너경로의 비용을 누적해가면서 해당 누적 비용보다 많을 경우 다음 탐색 노드로 선택하지 않으면 되는 생각보다 간단한 BFS 문제이다. 

항상 느끼지만 처음부터 정확하게 문제를 분석해서 옳바른 로직을 짜는 연습이 필요하다.

<세부 로직 사항>

- 기본적인 BFS 방식의 코드를 구성해야한다. 특이점이 있다면 우선 BFS 다음 탐색 노드의 정보들이 담길 큐에 각 노드에는 인덱스 위치와 각 노드별 현재 누적값 정보 뿐만 아니라 이전 탐색방향이 저장되어 있어야 한다. 이렇게 저장된 이전 탐색방향과 현재 탐색방향이 같은 경우에는 누적값에 100을 더해주고 다를 경우에는 600을 더해주어야 한다. (코너 비용이 추가적으로 발생)
- 이렇게 누적 비용을 계속 저장해가면서 BFS 탐색을 구현해 나가면서 큐가 빌 때까지 (nsize - 1, nsize - 1)의 위치에 도달한 최소 비용을 계산해주면 닶을 도출할 수 있다.

<br/>

<br/>

#### 코드 : 

```c++
#include <string>
#include <vector>
#include <algorithm>
#include <queue>

#define MAX 987654321

using namespace std;
typedef struct Qnode {
    int x, y, prevdir, val;
}Qnode;

int solution(vector<vector<int>> board) {
    int answer = MAX, dp[25][25], nsize = board.size();
    int di[4] = {0, 1, 0, -1}, dj[4] = {1, 0, -1, 0};
    queue<Qnode> q;

    for (int i = 0; i < nsize; i++)
        fill_n(dp[i], nsize, MAX);

    dp[0][0] = 0;
    if (!board[0][1]) {
        dp[0][1] = 100;
        q.push(Qnode{0, 1, 0, 100});
    }
    if (!board[1][0]) {
        dp[1][0] = 100;
        q.push(Qnode{1, 0, 1, 100});
    }

    while (!q.empty()) {
        int curx = q.front().x, cury = q.front().y, prevdir = q.front().prevdir, curval = q.front().val;
        q.pop();
        if (curx == nsize - 1 && cury == nsize - 1) {
            answer = min(answer, curval);
            continue;
        }

        for (int k = 0; k < 4; k++) {
            int cmpx = curx + di[k], cmpy = cury + dj[k], cmpval = (prevdir == k) ? curval + 100 : curval + 600;
            if (cmpx < 0 || cmpx >= nsize || cmpy < 0 || cmpy >= nsize) continue;
            if (dp[cmpx][cmpy] < cmpval || board[cmpx][cmpy]) continue;

            dp[cmpx][cmpy] = cmpval;
            q.push(Qnode{cmpx, cmpy, k, cmpval});
        }
    }
    return answer;
}
```

