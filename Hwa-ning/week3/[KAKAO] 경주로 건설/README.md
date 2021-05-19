# 2020 KAKAO 인턴십 코딩테스트 1. 경주로 건설

## 작성자 : Hwa-ning

## [문제 링크](https://programmers.co.kr/learn/courses/30/lessons/67259)

<br/>

### <풀이>

DP 배열을 사용한 BFS로 풀이한 문제, 그렇게 어려운 문제는 아니었는데 디테일을 조금 놓쳐서 처음에 틀렸다.
queue에 x,y 좌표, 현재까지의 건설 비용, 그 전의 방향(코너 비용 계산을 위함)을 저장하여 BFS로 진행한다.

1. x,y좌표 1,0 / 0,1 은 각각 0,0에서 동쪽/남쪽으로 이동한결과 이므로 초기 Queue에 {0, 1, 100, 0} ,{ 1, 0, 100, 1 } 값을 넣고 시작하는데 이 때, (1,0) / (0,1)이 만약 벽이라면 해당 좌표에서 시작할 수 없으므로 확인을 해야한다.(_처음 틀린 이유_)
2. int i를 이용한 for문으로 4방향에 대하여 Queue에 push해주는데 이 때 만약 이 전의 방향과 반대방향 (exDir+2)%4 == exDir 인 경우 역주행을 하는것이므로 고려할 사항이 아니므로 continue해줬다.<br>
   그리고 그 전의 방향과 같다면 비용을 100추가 / 다르다면 600을 추가한다.(코너를 건설하고 직선도로를 만들어야하기 때문)
3. DP 배열에 현재까지의 비용을 저장하여 다음번에 비용이 더 큰 경로가 해당 좌표에 방문하는 일이 없도록한다.
4. 그렇게 (N-1, N-1)에 도달하면 현재까지의 최소 비용과 해당 경로의 비용을 비교하여 값을 갱신해준다.<br>
   \*\*\* BFS를 진행하며 이 때 저장된 최소 비용보다 큰 비용의 경로가 있다면 해당경로는 최소 비용이 될 수 없으므로 무시하도록 구현했다.

### <코드>

```C++
#include <string>
#include <vector>
#include <queue>
#include <cstring>
#define MAX 987654321
using namespace std;

struct node {
    int x;
    int y;
    int cost;
    int exDir;
};

int solution(vector<vector<int>> board) {
    int answer = MAX;
    int DP[25][25];
    int N = board[0].size();
    int dx[4] = { 0,1,0,-1 };
    int dy[4] = { 1,0,-1,0 };
    for (int i = 0; i < N; i++)
        for (int j = 0; j < N; j++)
            DP[i][j] = MAX;
    queue<node>q;
    if (!board[1][0]) {
        q.push(node{ 1, 0, 100, 1 });
        DP[1][0] = 100;
    }
    if (!board[0][1]) {
        q.push(node{0, 1, 100, 0});
        DP[0][1] = 100;
    }
    while (!q.empty()) {
        int x = q.front().x;
        int y = q.front().y;
        int cost = q.front().cost;
        int exDir = q.front().exDir;
        q.pop();
        if (cost >= answer)
            continue;

        if (x == N - 1 && y == N - 1) {
            answer = min(answer, cost);
            continue;
        }

        for (int i = 0; i < 4; i++) {
            if ((exDir + 2) % 4 == i)
                continue;
            int nx = x + dx[i];
            int ny = y + dy[i];
            int ncost = (exDir == i ? cost + 100 : cost + 600);

            if (0 > nx || nx >= N || 0 > ny || ny >= N || board[nx][ny] || DP[nx][ny] < ncost)
                continue;

            q.push(node{ nx, ny, ncost, i });
            DP[nx][ny] = ncost;
        }
    }
    return answer;
}
```
