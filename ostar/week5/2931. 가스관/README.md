# 2931. 가스관

###### 작성자 : O-star

<br/>

<br/>

### 풀이 : 

처음에는 문제가 그리 어려워보이진 않았는데 ... 문제를 해결하려 하면 할수록, 보이지 않았던 케이스들이 보여서 많은 시간이 소요된 문제였다..

처음에 로직을 꼼꼼하게 짜보려고 시도했지만 놓친 로직을 다시 포함해가면서 짜다보니 코드가 길어진 것 같다. 연습이 더 필요하다.

**[ 주의사항 ]**

- 가스의 흐름은 유일하며 지운 블럭은 단 하나이다. 따라서 출발위치를 잡아서(출발위치는 M, Z 위치에 인접하고 있는 블록 하나를 선택한다.) 경로를 따라가다 '.' 위치를 찾으면 7개의 블럭을 대입하며 가능 경로가 만들어지는지 판단한다. **여기서 가장 주의해야할 점은 단순히 M -> Z로 이어지는 경로라고 해서 답이 되지는 않는다. M->Z로 이어지는 경로이면서 모든 블럭을 거쳐가는 경로를 찾아야 답이다.**
- 추가로 주의해야할 점은 블록을 모두 거쳐갔는지 판단해줄 때 '+' 블록의 경우 두 번 지나칠 수 있기 때문에 사용 여부를 boolean 배열과 count를 같이 사용해 구현해주어야 한다.
- 블럭을 코드로 구현할 때에 나는 배열로 구현하였다. 배열의 인덱스 번호를 출입 방향으로 가정하고, 해당 인덱스에 저장된 값을 출구 방향으로 가정하여 2차원 배열로 구현해주었다.

**[ 세부 구현사항 ]**

1. 모스크바 자그레브 인접 블록 중 하나를 골라서 경로 탐색을 시작한다. -> 여기서 모스크바 혹은 자그레브에 인접한 블록이 지워졌을 수 있기 때문에 모스크바 인접 블록이 없으면 자그래브 인접 블록을 탐색하는 코드까지 구현해야 한다.
2. 빈칸을 만날 경우 7개 블럭을 하나씩 넣어보면서 연결 경로가 만들어지는지 파악한다 -> 연결경로 파악에는 재귀 함수를 구현해서 경로탐색을 진행하였다.
3. 연결되는 경우가 되면 해당 대입 블록을 출력해준다.

코드를 좀 더 깔끔하게 짜려는 노력을 더 기울여보자 !

<br/>

<br/>

#### 코드 : 

```c++
#include <iostream>

using namespace std;
int inoutary[7][4] = {
        {-1, -1, 1,  0},
        {-1, 0,  3,  -1},
        {3,  2,  -1, -1},
        {1,  -1, -1, 2},
        {-1, 1,  -1, 3},
        {0,  -1, 2,  -1},
        {0,  1,  2,  3}
};
int rows, cols, di[4] = {0, 1, 0, -1}, dj[4] = {1, 0, -1, 0}, blocknums = 1;
char map[25][25], blocks[7] = {'1', '2', '3', '4', '|', '-', '+'}, finch;
bool usedblocks[25][25];

int findOutDirection(int indir, int blocknum) {
    return (blocknum == -1) ? -1 : inoutary[blocknum][indir];
}

int returnBlockNumber(char ch) {
    switch (ch) {
        case '1':
            return 0;
        case '2':
            return 1;
        case '3':
            return 2;
        case '4':
            return 3;
        case '|':
            return 4;
        case '-':
            return 5;
        case '+':
            return 6;
        default:
            return -1;
    }
}

bool insertNewBlock(int curx, int cury, int curdir, int curcnt) {
    if (curx < 0 || curx >= rows || cury < 0 || cury >= cols) return false;
    if (map[curx][cury] == finch) return (blocknums == curcnt);
    if (map[curx][cury] == '.' || curdir == -1) return false;

    bool retval;

    if (!usedblocks[curx][cury]) {
        usedblocks[curx][cury] = true;
        retval = insertNewBlock(curx + di[curdir], cury + dj[curdir],
                                findOutDirection(curdir, returnBlockNumber(map[curx + di[curdir]][cury + dj[curdir]])),
                                curcnt + 1);
        usedblocks[curx][cury] = false;
    } else
        retval = insertNewBlock(curx + di[curdir], cury + dj[curdir],
                                findOutDirection(curdir, returnBlockNumber(map[curx + di[curdir]][cury + dj[curdir]])),
                                curcnt);

    return retval;
}

int main() {
    char input;
    int mx, my, zx, zy, curx, cury, curdir = -1, blockcnt = 1;
    cin >> rows >> cols;

    for (int i = 0; i < rows; i++)
        for (int j = 0; j < cols; j++) {
            cin >> input;
            map[i][j] = input;
            if (input == 'M') {
                mx = i;
                my = j;
            } else if (input == 'Z') {
                zx = i;
                zy = j;
            } else if (input != '.') blocknums++;
        }

    for (int k = 0; k < 4; k++) {
        int cmpx = mx + di[k], cmpy = my + dj[k];
        if (cmpx < 0 || cmpx >= rows || cmpy < 0 || cmpy >= cols) continue;
        int cmpdir = findOutDirection(k, returnBlockNumber(map[cmpx][cmpy]));
        if (map[cmpx][cmpy] != '.' && map[cmpx][cmpy] != 'Z' && cmpdir != -1) {
            curx = cmpx;
            cury = cmpy;
            curdir = cmpdir;
            finch = 'Z';
            usedblocks[curx][cury] = true;
            break;
        }
    } // 모스크바 출발 블럭 찾기

    if (curdir == -1) {
        for (int k = 0; k < 4; k++) {
            int cmpx = zx + di[k], cmpy = zy + dj[k];
            if (cmpx < 0 || cmpx >= rows || cmpy < 0 || cmpy >= cols) continue;
            int cmpdir = findOutDirection(k, returnBlockNumber(map[cmpx][cmpy]));
            if (map[cmpx][cmpy] != '.' && map[cmpx][cmpy] != 'M' && cmpdir != -1) {
                curx = cmpx;
                cury = cmpy;
                curdir = cmpdir;
                finch = 'M';
                usedblocks[curx][cury] = true;
                break;
            }
        }
    }   // 자그레브 출발 블럭 찾기

    while (true) {
        curx += di[curdir];
        cury += dj[curdir];
        if (map[curx][cury] == '.') break;
        if (!usedblocks[curx][cury]) blockcnt++;
        usedblocks[curx][cury] = true;
        curdir = findOutDirection(curdir, returnBlockNumber(map[curx][cury]));
    }   // 빈 칸 찾을 때 까지 경로 탐색

    for (int k = 0; k < 7; k++) {
        map[curx][cury] = blocks[k];
        if (insertNewBlock(curx, cury, findOutDirection(curdir, returnBlockNumber(map[curx][cury])), blockcnt)) {
            cout << curx + 1 << ' ' << cury + 1 << ' ' << map[curx][cury] << '\n';
            break;
        }
    }

    return 0;
}
```

