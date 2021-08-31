# 13424. 비밀 모임

###### 작성자 : O-star

<br/>

<br/>

### 풀이 : 

**본 문제는 모든 정점에서 모든 정점까지의 최단 경로를 알아야 답을 도출할 수 있다고 판단해 플로이드 와샬 알고리즘을 활용했다.**

**우선 플로이드 와샬 알고리즘을 활용해 adj 배열을 완성시킨다.**

**이후 adj 배열을 활용해서 특정 정점에서 친구들이 대기하고 있는 정점들까지의 최소 경로들의 합을 구해 이 경로합이 가장 작은 노드를 구하면 답을 도출해낼 수 있다.**

**정점 자체가 100개 이하이기 때문에 n^3 시간 복잡도를 가지는 플로이드 알고리즘을 활용해도 충분히 시간 내에 답을 도출해 낼 수 있다.**

<br/>

<br/>

#### 코드 : 

```c++
#include <iostream>
#include <algorithm>

#define INF 987654321

using namespace std;
int adj[101][101];

int testCase() {
    bool friendsRoom[101]{false};
    int n, m, fir, sec, val, nums, num;

    cin >> n >> m;
    for (int k = 0; k <= n; k++) {
        fill_n(adj[k], n + 1, INF);
        adj[k][k] = 0;
    }

    while (m--) {
        cin >> fir >> sec >> val;
        adj[fir][sec] = val;
        adj[sec][fir] = val;
    }

    cin >> nums;
    while (nums--) {
        cin >> num;
        friendsRoom[num] = true;
    }

    for (int k = 1; k <= n; k++) {
        for (int i = 1; i <= n; i++) {
            for (int j = 1; j <= n; j++) {
                adj[i][j] = min(adj[i][j], adj[i][k] + adj[k][j]);
            }
        }
    }

    int minDistSum = INF, answer;
    for (int k = 1; k <= n; k++) {
        int curSum = 0;

        for (int t = 1; t <= n; t++) {
            if (friendsRoom[t]) {
                curSum += adj[k][t];
            }
        }

        if (minDistSum > curSum) {
            minDistSum = curSum;
            answer = k;
        }
    }

    return answer;
}

int main() {
    int T;
    cin >> T;
    while (T--) {
        cout << testCase() << '\n';
    }
}
```

