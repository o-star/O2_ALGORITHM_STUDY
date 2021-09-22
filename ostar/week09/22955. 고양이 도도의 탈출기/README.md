# 22955. 고양이 도도의 탈출기

###### 작성자 : O-star

<br/>

<br/>

### 풀이 : 

**배열에서 고양이 도도가 탈출할 수 있는 경로를 완전 탐색하는 방식을 사용하면 문제를 해결할 수 있다.**

**단, 문제 자체에서 현재 칸과 이동할 칸의 조건들을 잘 정리해 설계하지 않으면 칸의 종류가 다양하고 이동 과정도 다채롭게 이어지기 때문에 에러케이스가 생길 수 있다. 이러한 점을 유의해야 한다.**

**</br>**

**[ 세부 구현 사항 ]**

- **priority queue를 활용해서 현재 비용이 가장 적은 이동 경우의 수부터 완전 탐색하도록 설계**
- **`visited[1000][1000]` 배열을 활용해 방문한 노드는 재 방문해서 탐색하지 않도록 설계**
- **고양이 칸인 'C' 칸은 catX, catY 변수에 위치값을 저장 후 'F'칸으로 변경 -> 'C' case를 'F' case로 통일해서 코드 설계에 용이**
- **현재 칸의 종류에 따른 경우의 수를 정리해야 함.**
  - **좌, 우 이동이 가능한 경우: 현재 칸이 'F' or 'L'인 경우.**
  - **위로 이동이 가능한 경우: 현재 칸이 'L'인 경우.**
  - **아래로 이동이 가능한 경우: 현재의 아래 칸이 'L'인 경우**
- **현재 칸의 종류에 따라 해당 방향으로 이동할 수 있는지의 유무를 판단해주는 코드를 isPossibleDir 함수로 분리**
- **이동이 가능한 방향일 경우, 이동 후의 칸의 종류에 따라 다시 경우의 수를 정리해야 함.**
  - **이동한 칸이 'D'인 경우: 강아지가 있는 경우로 이동이 불가능한 칸. pq에 저장하지 않음**
  - **이동한 칸이 'X'인 경우: 뚫린 칸인 경우이기 때문에 더 이상 뚫린 칸이 아닐 때까지 아래로 떨어뜨림**
  - **나머지의 경우는 모두 pq에 정보를 저장함.**

<br/>

<br/>

### 코드 : 

```c++
#include <iostream>
#include <queue>

using namespace std;
struct Qnode {
    int cost, posX, posY;
};

struct compare {
    bool operator()(Qnode a, Qnode b) {
        return a.cost > b.cost;
    }
};

char map[1000][1000];
bool visited[1000][1000];
int h, w, di[4] = {0, 1, 0, -1}, dj[4] = {1, 0, -1, 0};

bool isPossibleDir(int dir, char curCh, char downCh) {
    switch (dir) {
        case 0:
            return curCh == 'F' || curCh == 'L';
        case 1:
            return downCh == 'L';
        case 2:
            return curCh == 'F' || curCh == 'L';
        default:
            return curCh == 'L';
    }
}

int bfs(int catX, int catY) {
    priority_queue<Qnode, vector<Qnode>, compare> pq;
    pq.push(Qnode{0, catX, catY});

    while (!pq.empty()) {
        int curCost = pq.top().cost, curX = pq.top().posX, curY = pq.top().posY;
        pq.pop();

        if (map[curX][curY] == 'E') return curCost;

        if (visited[curX][curY]) continue;
        visited[curX][curY] = true;

        for (int k = 0; k < 4; k++) {
            if (!isPossibleDir(k, map[curX][curY], map[curX + 1][curY])) continue;

            int cmpX = curX + di[k], cmpY = curY + dj[k], cmpCost = curCost + ((k % 2) ? 5 : 1);
            bool isHole = false;
            while (true) {
                if (cmpX < 0 || cmpX >= h || cmpY < 0 || cmpY >= w) break;
                char cmpCh = map[cmpX][cmpY];
                if (cmpCh == 'D' || visited[cmpX][cmpY]) break;
                if (cmpCh == 'X') {
                    cmpX++;
                    isHole = true;
                } else {
                    pq.push(Qnode{isHole ? cmpCost + 10 : cmpCost, cmpX, cmpY});
                    break;
                }
            }
        }
    }
    return -1;
}

int main() {
    int catX, catY;
    char input;
    cin >> h >> w;

    for (int i = 0; i < h; i++) {
        for (int j = 0; j < w; j++) {
            cin >> input;
            map[i][j] = input;
            if (input == 'C') {
                catX = i;
                catY = j;
                map[i][j] = 'F';
            }
        }
    }

    int answer = bfs(catX, catY);
    if (answer == -1) cout << "dodo sad\n";
    else cout << answer << '\n';

    return 0;
}
```

