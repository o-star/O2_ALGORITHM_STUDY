# 1826. 연료 채우기

###### 작성자 : O-star

<br/>

<br/>

### 풀이 : 

**풀이 자체는 엄청 간단한 문제였지만 그리디 방식으로 접근하지 못해 감을 잡지 못한 문제였다.**

**문제는 매우 간단하다. 연료 1L가 1km 이동 시 새어나가기 때문에 주유소에서 얻어내는 누적 연료합이 목적지까지의 거리보다 큰 경우의 최소 정지 횟수를 구하면 답을 도출할 수 있다.**

**따라서 먼저 거리 오름차순으로 정렬된 주유소 연료 정보 pair vector를 준비한다.**

**본 벡터를 활용해서 현재 누적 연료합으로 갈 수 있는 주요소의 연료 정보를 모두 우선순위 큐에 넣어 저장한다. 우선순위 큐에 저장된 연료 정보들은 모두 방문해서 얻을 수 있는 연료 정보들이기 때문에 내림차순으로 정렬해 도착지까지 갈 수 있는 연료 합이 만들어질 때까지 pop과정을 거쳐주면 최소 정지 횟수로 도달할 수 있는 경우를 찾아낼 수 있다.**

<br/>

<br/>

#### 코드 : 

```python
#include <iostream>
#include <queue>
#include <vector>
#include <utility>
#include <algorithm>

#define pii pair<int, int>

using namespace std;

int main() {
    vector<pii > stations;
    priority_queue<int> pq;
    int n, dist, fuel, dest, curFuel, curIdx = 0, answer = -1;

    cin >> n;
    for (int i = 0; i < n; i++) {
        cin >> dist >> fuel;
        stations.emplace_back(dist, fuel);
    }
    cin >> dest >> curFuel;
    sort(stations.begin(), stations.end());
    pq.push(0);

    while (!pq.empty()) {
        curFuel += pq.top();
        pq.pop();
        answer++;

        if (curFuel >= dest) {
            cout << answer << '\n';
            return 0;
        }

        while (curIdx < n && curFuel >= stations[curIdx].first) {
            pq.push(stations[curIdx].second);
            curIdx++;
        }
    }
    cout << "-1\n";
    return 0;
}
```

