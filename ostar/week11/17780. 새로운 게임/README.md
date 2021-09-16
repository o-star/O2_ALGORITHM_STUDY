# 17780. 새로운 게임

###### 작성자 : O-star

<br/>

<br/>

### 풀이 : 

**구현에 가까운 문제인 듯 하다.**

**우선 각 말의 정보를 담을 HorseInfo 구조체를 만들어준다. HorseInfo에는 현재 말의 방향과 인덱스 정보, 본 말 위에 타고 있는 말들을 저장할 벡터와 현재 말이 가장 아래에 위치해있는지의 유무를 판단하는 isRoot 변수가 존재한다.**

**우선 말이 움직이는 경우의 수를 잘 분석하여야 한다. 칸은 흰색, 빨간색, 파란색 총 3가지 경우의 수가 존재한다. 또 각 색 칸 경우의 수들에는 다음 칸에 이미 말이 존재하는 경우와 다음 칸에 말이 존재하지 않는 경우 2가지를 모두 분할해서 구현해주어야 한다.**

- **흰 칸 : 흰 칸일 경우에는 방향 전환없이 일반적으로 1칸 이동한다. 단, 이동하려는 칸에 말이 존재하는 경우에는 말의 위에 올려준다. (말의 위에 올려주는 것을 구현하는 데에는 isRoot와 말의 정보를 담은 구조체의 st 벡터를 활용한다. 가장 밑에 깔린 말만 isRoot를 true로 표시한다.)**
- **빨간 칸 : 빨간 칸일 경우에도 역시 이동하려는 칸에 말이 존재하는 경우와 존재하지 않는 경우를 고려한다. 단 말을 빨간 칸으로 이동할 시에는 st 벡터의 정보를 거꾸러 다시 담아준다.**
- **파란 칸 : 파란 칸의 경우에는 이동이 없을 경우도 있다. 방향을 반대로 한 칸 이동한 칸 또한 파란 칸일 경우에는 이동 없이 방향 전환만 있기 때문에 이동 과정을 담은 moveHorse 함수와 별개로 구현해주어야 구현이 편한다.**

<br/>

<br/>

#### 코드 : 

```c++
#include <iostream>
#include <vector>
#include <stack>

using namespace std;
struct HorseInfo {
    int x, y, dir;
    vector<int> st;
    bool isRoot = true;
};
int board[13][13], rootNum[13][13], horses, n;
int di[5] = {0, 0, 0, -1, 1}, dj[5] = {0, 1, -1, 0, 0};
vector<HorseInfo> horseVec;

int reverseDir(int dir) {
    switch (dir) {
        case 1:
            return 2;
        case 2:
            return 1;
        case 3:
            return 4;
        default:
            return 3;
    }
}

bool moveHorse(int t, int nextX, int nextY) {
    if (board[nextX][nextY] == 1) {
        int curRoot;
        if (rootNum[nextX][nextY]) {
            curRoot = rootNum[nextX][nextY];
            horseVec[t].isRoot = false;

        } else {
            curRoot = horseVec[t].st.back();
            horseVec[t].isRoot = false;
            horseVec[curRoot].isRoot = true;
            rootNum[nextX][nextY] = curRoot;
            horseVec[curRoot].x = nextX;
            horseVec[curRoot].y = nextY;
        }

        if (curRoot != t) {
            for (int k = horseVec[t].st.size() - 1; k >= 0; k--) {
                horseVec[curRoot].st.push_back(horseVec[t].st[k]);
            }
            horseVec[t].st.clear();
        }
        if (horseVec[curRoot].st.size() >= 4) return true;
    } else {
        if (rootNum[nextX][nextY]) {
            int curRoot = rootNum[nextX][nextY];
            horseVec[t].isRoot = false;

            for (int num: horseVec[t].st) {
                horseVec[curRoot].st.push_back(num);
            }
            horseVec[t].st.clear();
            if (horseVec[curRoot].st.size() >= 4) return true;
        } else {
            rootNum[nextX][nextY] = t;
            horseVec[t].x = nextX;
            horseVec[t].y = nextY;
        }
    }
    return false;
}

bool oneMoveTurn() {
    for (int t = 1; t <= horses; t++) {
        if (!horseVec[t].isRoot) continue;
        int curDir = horseVec[t].dir, curX = horseVec[t].x, curY = horseVec[t].y;
        int nextX = curX + di[curDir], nextY = curY + dj[curDir];

        rootNum[curX][curY] = 0;
        if (nextX <= 0 || n < nextX || nextY <= 0 || n < nextY || board[nextX][nextY] == 2) {
            curDir = reverseDir(curDir);
            horseVec[t].dir = curDir;

            int finX = curX + di[curDir], finY = curY + dj[curDir];
            if (0 < finX && finX <= n && 0 < finY && finY <= n && board[finX][finY] != 2) {
                if (moveHorse(t, finX, finY)) return true;
            } else {
                rootNum[curX][curY] = t;
            }
        } else {
            if (moveHorse(t, nextX, nextY)) return true;
        }
    }
    return false;
}

int main() {
    int posX, posY, dir;
    cin >> n >> horses;

    for (int i = 1; i <= n; i++) {
        for (int j = 1; j <= n; j++) {
            cin >> board[i][j];
        }
    }

    horseVec.push_back(HorseInfo{-1, -1, -1});
    for (int k = 1; k <= horses; k++) {
        cin >> posX >> posY >> dir;
        horseVec.push_back(HorseInfo{posX, posY, dir, {k}});
        rootNum[posX][posY] = k;
    }

    for (int k = 1; k <= 1000; k++) {
        if (oneMoveTurn()) {
            cout << k << '\n';
            return 0;
        }
    }
    cout << "-1\n";

    return 0;
}
```

