# 4577. 소코반

###### 작성자 : O-star

<br/>

<br/>

### 풀이 : 

**소코반 게임의 최소 이동횟수를 구하는 문제가 아니고, 이동방향을 가리키는 문자열을 보고 해당 이동으로 게임을 해결할 수 있는지 없는지에 관한 문제로 구현 카테고리의 문제였다.**

**박스가 목표점에 도달해도 다시 벗어날 수 있으며, 유저가 목표점에 서있을 수 있거나 박스가 벽이나 박스로 가로막힌 경우 등의 여러 변수 상황들만 잘 캐치한다면 구현은 크게 어렵지 않은 문제였다.**

**[ 주의사항 ]**

- **유저가 이동한 방향 뿐만 아니라 전에 있던 위치를 잘 업데이트 해주는 것도 중요하다. 전의 위치를 업데이트 해주는 것은 크게 두 가지 분류가 있다. 유저가 목표점에 서 있는 경우, 서 있지 않은 경우. 목표점에 서 있는 경우에는 유저가 이동하고 난 후 기존의 위치에 '+' 문자를 넣어주어야 한다. 이 외의 경우에는 '.' 문자를 넣어준다.**
- **처음에는 이동방향을 가리키는 문자열을 보고 첨부터 끝까지 이동시켜준 후 게임 complete 여부를 따져주었다. 하지만 문제를 잘 읽어보면 이동방향 문자열 중간에 박스를 모두 목표점에 이동시킨 경우에는 해당 지점에서 게임을 종료하고 결과값을 출력해주면 된다. 따라서 매 이동 순간마다 목표점에 다다르지 못한 박스의 갯수를 세고 있어야 한다.**

**[ 세부 구현사항 ]**

1. **map을 입력받음과 동시에 유저의 위치와 목표점에 도달하지 못한 'b'의 갯수를 확인한다.**
2. **입력받은 이동 방향 문자열을 첨부터 차례로 이동시켜나가면서 벽일 경우, 목표점일 경우, 박스일 경우, 빈 공간일 경우에 대해 세부적으로 케이스를 만들어 모든 경우의 수 조건문을 세세하게 구현한다.**
3. **이동 방향 진행 중 모든 박스가 목표점에 다다르면 곧바로 게임을 종료시킨다.**

<br/>

<br/>

#### 코드 : 

```c++
#include <iostream>
#include <string>

using namespace std;
int rows, cols, di[4] = {0, 1, 0, -1}, dj[4] = {1, 0, -1, 0};
char map[15][15];

int changeDirNumber(char ch) {
    switch (ch) {
        case 'R':
            return 0;
        case 'D':
            return 1;
        case 'L':
            return 2;
        case 'U':
            return 3;
    }
}

void testCase(int casenum) {
    int curx, cury, remains = 0;
    bool iscomplete = true;
    string movestr;

    for (int i = 0; i < rows; i++)
        for (int j = 0; j < cols; j++) {
            cin >> map[i][j];
            if (map[i][j] == 'w' || map[i][j] == 'W') {
                curx = i;
                cury = j;
            } else if (map[i][j] == 'b') remains++;
        }

    cin >> movestr;
    int length = movestr.length();

    for (int k = 0; k < length; k++) {
        int curdir = changeDirNumber(movestr[k]), cmpx = curx + di[curdir], cmpy = cury + dj[curdir];
        char cmpch = map[cmpx][cmpy];

        if (cmpch == '#') continue;
        if (cmpch == '+') map[cmpx][cmpy] = 'W';
        else if (cmpch == '.') map[cmpx][cmpy] = 'w';
        else if (cmpch == 'b' || cmpch == 'B') {
            int nextx = cmpx + di[curdir], nexty = cmpy + dj[curdir];
            char nextch = map[nextx][nexty], flagch = (cmpch == 'b') ? 'w' : 'W';

            if (nextch == '+') {
                map[nextx][nexty] = 'B';
                map[cmpx][cmpy] = flagch;
                if (flagch == 'W') remains++;
                remains--;
            } else if (nextch == '.') {
                map[nextx][nexty] = 'b';
                map[cmpx][cmpy] = flagch;
                if (flagch == 'W') remains++;
            } else continue;
        }

        if (map[curx][cury] == 'W') map[curx][cury] = '+';
        else map[curx][cury] = '.';
        // 기존의 자리 문자 교체

        if (!remains) break;

        curx = cmpx;
        cury = cmpy;
    }

    cout << "Game " << casenum << ": " << (remains ? "incomplete\n" : "complete\n");

    for (int i = 0; i < rows; i++) {
        for (int j = 0; j < cols; j++)
            cout << map[i][j];
        cout << '\n';
    }
}

int main() {
    int cnt = 1;

    while (true) {
        cin >> rows >> cols;
        if (!rows && !cols) break;
        testCase(cnt++);
    }

    return 0;
}
```

