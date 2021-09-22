# 20056. 마법사 상어와 파이어볼

###### 작성자 : O-star

<br/>

<br/>

#### 풀이 : 

**골드 5 구현 문제이지만 음... 문제가 그리 쉽진 않았다. 삼성 역량 테스트 문제인데 사실 실제 시험에 나왔어도 꽤나 고민했어야 할 문제같았다.**

**문제는 큐에 파이어볼 위치, 질량, 속력, 방향을 잘 정리해 저장해 나가야 헷갈리지 않고 풀 수 있다. 또 중간에 같은 위치에 있는 파이어볼은 다음 이동 전에 합쳐서 4개로 나누어주어야 하는데 이 과정에서 실수 없이 로직을 잘 짜야만 에러 케이스 없이 문제를 해결할 수 있다.**

**<br/>**

**세부 구현사항 :** 

1. **초반에 입력으로 들어오는 파이어볼의 행, 열 위치정보, 질량, 이동 방향, 이동 속도를 정보로 가지는 노드들의 큐를 구성한다. 큐를 구성하고 나면 큐에 들어있는 노드를 차례대로 빼면서 파이어볼을 이동후 Ballnode 구조체로 구성된 map 배열에 저장한다.**
2. **여기서 map 배열은 현재 움직인 파이어 볼들의 질량, 스피드, 이동 방향 외에도 추가 정보를 담고 있다. 만약 같은 위치에 두 개 이상 파이어볼이 위치할 경우 파이어 볼 갯수를 저장하는 ballnums와 해당 위치에 같이 위치하고 있는 파이어볼 들의 방향이 모두 같은 짝수, 홀수 방향을 띄는지를 의미하는 bool 변수 issamedir 정보를 저장한다.**
3. **큐에서 꺼낸 파이어볼들을 모두 이동하여 map 배열에 정보를 저장하고 나면 map 배열들을 차례로 탐색하며 파이어볼이 담긴 위치들에 도착할 경우 해당 인덱스의 정보들을 가지고 다시금 큐에 노드를 추가한다. => 다음 이동에 쓰기 위해**
4. **1~3번 과정을 입력 케이스에 명시된 반복 횟수만큼 반복하고 난 후에 큐에 아직 남은 노드들의 데이터를 가지고 질량의 합을 구해주면 답을 도출할 수 있다.**

<br/>

<br/>

#### 코드 : 

```c++
#include <iostream>
#include <queue>

using namespace std;
typedef struct Qnode {
    int x, y, mass, dir, speed;
} Qnode;

typedef struct Ballnode {
    int mass = 0, speed = 0, dir = -1, ballnums = 0;
    bool issamedir = true;
};
Ballnode map[51][51];
queue<Qnode> q;
int di[8] = {-1, -1, 0, 1, 1, 1, 0, -1}, dj[8] = {0, 1, 1, 1, 0, -1, -1, -1}, n;

void fireballMoveProcess() {

    while (!q.empty()) {
        int curx = q.front().x, cury = q.front().y, curmass = q.front().mass, curdir = q.front().dir, curspeed = q.front().speed;
        int cmpx = ((curx + di[curdir] * curspeed) % n + n) % n, cmpy = ((cury + dj[curdir] * curspeed) % n + n) % n;
        q.pop();

        if (map[cmpx][cmpy].issamedir && map[cmpx][cmpy].dir != -1)
            map[cmpx][cmpy].issamedir = (map[cmpx][cmpy].dir % 2 == curdir % 2);
        map[cmpx][cmpy].mass += curmass;
        map[cmpx][cmpy].speed += curspeed;
        map[cmpx][cmpy].ballnums++;
        map[cmpx][cmpy].dir = curdir;
    }   // fireball 이동 후 map에 정보 저장

    for (int i = 0; i < n; i++)
        for (int j = 0; j < n; j++) {
            if (map[i][j].ballnums == 0) continue;
            else if (map[i][j].ballnums == 1) q.push(Qnode{i, j, map[i][j].mass, map[i][j].dir, map[i][j].speed});
            else {
                int startdir = (map[i][j].issamedir) ? 0 : 1;
                int cmpmass = map[i][j].mass / 5, cmpspeed = map[i][j].speed / map[i][j].ballnums;
                if (cmpmass) {
                    for (int k = 0; k < 4; k++) {
                        q.push(Qnode{i, j, cmpmass, startdir, cmpspeed});
                        startdir += 2;
                    }
                }
            }
            map[i][j] = Ballnode{0, 0, -1, 0, true};
        }
    // 파이어볼들 다시 큐에 삽입 (2개이상 같은 칸에 있는 경우 4개로 분할해서 큐에 삽입)
}

int main() {
    int m, repeats, totalmass = 0;
    cin >> n >> m >> repeats;

    int x, y, mass, dir, speed;
    while (m--) {
        cin >> x >> y >> mass >> speed >> dir;
        q.push(Qnode{x - 1, y - 1, mass, dir, speed});
    }

    while (repeats--) {
        fireballMoveProcess();
    }

    while (!q.empty()) {
        totalmass += q.front().mass;
        q.pop();
    }

    cout << totalmass << '\n';
}
```

