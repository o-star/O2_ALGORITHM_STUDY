# 1944. 복제 로봇

###### 작성자 : O-star

<br/>

<br/>

### 풀이 : 

**매우 흥미로운 문제인 듯 하다.**

**문제는 얼핏 보게 되면 BFS 응용 문제인 듯 하지만 MST의 개념을 활용해야 하는 문제이다.**

**로봇은 출발점과 열쇠 위치에서 복제를 할 수 있다고 명시되어 있는데, 이는 곧 한 로봇이 움직이는 한 붓 그리기 형태가 아닌 그래프(트리) 형태를 띄고 있을 수 있다는 의미이기 때문에 사실상 출발점과 키 위치를 노드로 생각하는 MST를 구하는 문제이다.**

**따라서 나는 본 문제를 다음과 같이 해결하였다.**

1. **출발점과 각 키의 위치에서 출발해 다른 키의 위치까지 갈 수 있는 경로의 길이를 구하는 BFS 과정을 거쳐 adj 배열을 생성한다.**
2. **본 adj 배열을 사용하여 MST의 길이를 구한다.**
3. **본 MST의 길이 합이 로봇이 가장 최소로 움직이는 거리에 해당한다.**

<br/>

<br/>

#### 코드 : 

```c++
#include <iostream>
#include <queue>
#include <algorithm>
#include <utility>

#define pii pair<int, int>
#define MAX 100000

using namespace std;
struct Qnode {
    int posX, posY, dist;
};
char board[50][50];
int keyNumber[50][50], adj[251][251], n, keyNum;
int di[4] = {0, 1, 0, -1}, dj[4] = {1, 0, -1, 0};

void BFS(int startX, int startY) {
    bool visited[50][50]{false};
    queue<Qnode> q;
    int startNum = keyNumber[startX][startY];
    q.push(Qnode{startX, startY, 0});
    visited[startX][startY] = true;

    while (!q.empty()) {
        int curX = q.front().posX, curY = q.front().posY, curDist = q.front().dist;
        q.pop();

        if ((board[curX][curY] == 'K' || board[curX][curY] == 'S') && curDist) {
            adj[startNum][keyNumber[curX][curY]] = min(adj[startNum][keyNumber[curX][curY]], curDist);
            continue;
        }
        for (int k = 0; k < 4; k++) {
            int nextX = curX + di[k], nextY = curY + dj[k];
            if (board[nextX][nextY] != '1' && !visited[nextX][nextY]) {
                q.push(Qnode{nextX, nextY, curDist + 1});
                visited[nextX][nextY] = true;
            }
        }
    }
}

int findMST(int startX, int startY) {
    priority_queue<pii, vector<pii >, greater<>> pq;
    bool gained[251]{false};
    int gainKeyCnt = -1, accSum = 0;
    pq.push(pii(0, 0));

    while (!pq.empty()) {
        int curDist = pq.top().first, curNum = pq.top().second;
        pq.pop();

        if (gained[curNum]) continue;
        gained[curNum] = true;
        accSum += curDist;
        if (++gainKeyCnt == keyNum) return accSum;

        for (int t = 1; t <= keyNum; t++) {
            if (adj[curNum][t] == MAX) continue;
            pq.push(pii(adj[curNum][t], t));
        }
    }
    return -1;
}

int main() {
    int keyCnt = 1, startX, startY;
    char input;
    cin >> n >> keyNum;

    for (int t = 0; t <= keyNum; t++)
        fill_n(adj[t], keyNum + 1, MAX);

    for (int i = 0; i < n; i++)
        for (int j = 0; j < n; j++) {
            cin >> input;
            board[i][j] = input;
            if (input == 'S') {
                startX = i;
                startY = j;
            }
            if (input == 'K') keyNumber[i][j] = keyCnt++;
        }

    BFS(startX, startY);
    for (int i = 0; i < n; i++)
        for (int j = 0; j < n; j++) {
            if (board[i][j] == 'K')
                BFS(i, j);
        }
    cout << findMST(startX, startY) << '\n';

    return 0;
}
```

