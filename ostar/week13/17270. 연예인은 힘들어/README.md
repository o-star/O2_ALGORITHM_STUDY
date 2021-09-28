# 17270. 연예인은 힘들어

###### 작성자 : O-star

<br/>

<br/>

### 풀이 : 

**풀이 방식은 비슷하게 접근했으나 문제를 잘 이해하고 조건을 순서대로 구현하여야 정확하게 풀어낼 수 있는 문제였다.**

**두 스팟에서 최소 거리합이 가장 가까운 스팟 찾기? -> 지헌과 성하가 위치한 지점에서 각 지점까지의 최소 거리를 구하기 위해 다익스트라 알고리즘을 두 번 실행시켰음.**

**만족해야 하는 조건이 4가지 -> 처음에는 이 4가지를 순차적으로 구현하지 않고 한 번의 반복문에서 모두 선별하려 했으나 계속 fail 발생. 조건을 계속 자세히 읽어보게 되면 2번 조건에서 최단시간의 합이 "최소"가 되게 하고 싶다고 적혀있음. 즉, 3번 조건인 지헌이가 성하보다 더 빨리 도착하는 조건을 먼저 구현하게 되면 지헌이 성하보다 더 빨리 도착하는 지점 중 "최소"를 구하게 되기 때문에 조건이 미묘하게 달라지게 됌. 따라서 본 문제는 첫 번째 조건부터 차례대로 구현해주어야 Success를 받을 수 있음.**

<br/>

<br/>

#### 코드 : 

```c++
#include <iostream>
#include <vector>
#include <utility>
#include <queue>

#define pii pair<int, int>
#define MAX 100000000

using namespace std;
vector<vector<pii>> adj;

void dijkstra(int startPos, int distAry[]) {
    bool visited[101]{false};
    priority_queue<pii, vector<pii >, greater<>> pq;
    pq.push(pii(0, startPos));

    while (!pq.empty()) {
        int curTime = pq.top().first, curSpot = pq.top().second;
        pq.pop();

        if (visited[curSpot]) continue;
        visited[curSpot] = true;
        distAry[curSpot] = curTime;
        int roadNum = adj[curSpot].size();
        for (int k = 0; k < roadNum; k++) {
            int nextSpot = adj[curSpot][k].first, nextTime = adj[curSpot][k].second;
            if (!visited[nextSpot]) {
                pq.push(pii(curTime + nextTime, nextSpot));
            }
        }
    }
}

int main() {
    int jDist[101], sDist[101];
    int spots, roads, start, end, time, jStart, sStart;
    cin >> spots >> roads;
    adj.resize(spots + 1);
    fill_n(jDist, spots + 1, MAX);
    fill_n(sDist, spots + 1, MAX);

    while (roads--) {
        cin >> start >> end >> time;
        adj[start].emplace_back(end, time);
        adj[end].emplace_back(start, time);
    }
    cin >> jStart >> sStart;

    dijkstra(jStart, jDist);
    dijkstra(sStart, sDist);

    int minSum = MAX, minJtime = MAX, minSpot = -1;
    for (int t = 1; t <= spots; t++) {
        if (t == jStart || t == sStart) continue;
        int cmpSum = jDist[t] + sDist[t];
        if (cmpSum < minSum) minSum = cmpSum;
    }
    for (int t = 1; t <= spots; t++) {
        if (t == jStart || t == sStart) continue;
        int cmpSum = jDist[t] + sDist[t];
        if (minSum == cmpSum) {
            if (jDist[t] > sDist[t]) continue;
            if (minJtime > jDist[t]) {
                minJtime = jDist[t];
                minSpot = t;
            }
        }
    }
    cout << minSpot << '\n';
    return 0;
}
```

