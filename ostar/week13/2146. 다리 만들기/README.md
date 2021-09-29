# 2146. 다리 만들기

###### 작성자 : O-star

<br/>

<br/>

### 풀이 : 

**문제는 각 육지별 가장 인접해 있는 바다에서 시작해서 가장 가까이 있는 육지와의 거리를 측정하면 답을 도출할 수 있다.**

**즉, 문제에서는 BFS 알고리즘을 두 번 활용하게 되면 문제를 해결할 수 있다.**

**<br/>**

**[ 세부 구현사항 ]**

- **explored 배열 : board 배열을 참고해서 탐색해본 육지(섬)의 여부를 체크해두는 배열이다. 본 배열을 통해 탐색한 섬을 중복해서 탐색 진행을 하지 않도록 구현하였으며, 인접한 바다에서 가장 가까이 있는 육지까지의 거리를 측정할 때 현재 시작한 섬과의 거리를 측정하지 않도록 해준다.**
- **visited 배열 : visited 배열은 바다 상에서 다음 섬까지의 거리를 측정할 때 이미 탐색한 바다 칸을 중복해서 체크하지 않도록 해주는 배열이다.**
- **findMinBridge 함수에서 보듯이 가장 먼저 아직 탐색하지 않은 육지에서 시작해서 섬의 영역을 BFS 알고리즘으로 탐색한다. 이 과정에서 섬과 바로 인접해있는 바다 칸(0)을 bridgeQ에 별도로 저장시킨다.**
- **이렇게 해당 섬과 바로 인접해 있는 바다 칸 정보를 모두 모은 후에는 이 큐를 활용해서 2차 BFS를 탐색한다. 여기서 오직 바다 칸으로 다른 섬으로 이동할 수 있는 최소 거리를 구한다.**
- **이렇게 각 섬 별로 다리 최소 길이를 구해 이 중 가장 작은 값을 도출하면 답에 해당한다.**

<br/>

<br/>

#### 코드 : 

```c++
#include <iostream>
#include <queue>
#include <utility>
#include <algorithm>

#define pii pair<int, int>

using namespace std;
struct Space {
    int posX, posY, dist;
};
bool board[100][100], explored[100][100];
int n, di[4] = {0, 1, 0, -1}, dj[4] = {1, 0, -1, 0};

int findMinBridge(int startX, int startY) {
    queue<pii > islandQ;
    queue<Space> bridgeQ;
    bool visited[100][100]{false};

    islandQ.push(pii(startX, startY));
    explored[startX][startY] = true;
    while (!islandQ.empty()) {
        int curX = islandQ.front().first, curY = islandQ.front().second;
        islandQ.pop();

        for (int k = 0; k < 4; k++) {
            int nextX = curX + di[k], nextY = curY + dj[k];
            if (nextX < 0 || nextX >= n || nextY < 0 || nextY >= n) continue;
            if (explored[nextX][nextY] || visited[nextX][nextY]) continue;
            if (board[nextX][nextY]) {
                islandQ.push(pii(nextX, nextY));
                explored[nextX][nextY] = true;
            } else {
                bridgeQ.push(Space{nextX, nextY, 1});
                visited[nextX][nextY] = true;
            }
        }
    }

    while (!bridgeQ.empty()) {
        int curX = bridgeQ.front().posX, curY = bridgeQ.front().posY, curDist = bridgeQ.front().dist;
        bridgeQ.pop();

        for (int k = 0; k < 4; k++) {
            int nextX = curX + di[k], nextY = curY + dj[k];
            if (nextX < 0 || nextX >= n || nextY < 0 || nextY >= n) continue;
            if (explored[nextX][nextY] || visited[nextX][nextY]) continue;
            if (board[nextX][nextY]) {
                return curDist;
            } else {
                bridgeQ.push(Space{nextX, nextY, curDist + 1});
                visited[nextX][nextY] = true;
            }
        }
    }
    return 100000;
}

int main() {
    int answer = 100000;
    cin >> n;
    for (int i = 0; i < n; i++)
        for (int j = 0; j < n; j++)
            cin >> board[i][j];

    for (int i = 0; i < n; i++)
        for (int j = 0; j < n; j++)
            if (!explored[i][j] and board[i][j])
                answer = min(answer, findMinBridge(i, j));
    cout << answer;
    return 0;
}
```

