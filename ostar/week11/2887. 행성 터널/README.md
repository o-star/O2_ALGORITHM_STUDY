# 2887. 행성 터널

###### 작성자 : O-star

<br/>

<br/>

### 풀이 : 

**3차원 좌표계로 x, y, z 축 간의 거리를 고려해줘야 하는 MST 문제인 듯 했다.**

**우선 행성이 10만 개이고, 행성 간의 거리는 min(|xA-xB|, |yA-yB|, |zA-zB|)이다. 따라서 MST를 구할 때 특정 행성에서 x, y, z축 3가지 기준으로 가장 근접해있는 행성과의 거리를 우선순위 큐에 삽입하여 가장 가까운 거리에 있는 행성들끼리 연결시키며 최소 스패닝 트리를 만들어 내야 한다.**

- **sortX, sortY, sortZ 벡터 : x, y, z 축 좌표를 기준으로 정렬한 벡터 3개를 준비한다.**
- **sortInfos 벡터 : 정렬한 sortX, sortY, sortZ 벡터의 인덱스를 각 행성별로 저장해두는 벡터. 본 벡터를 사용하여 각 축 별로 가장 근접해 있는 행성과의 거리를 계산해서 연결해주는 역할을 한다.**
- **축이 세 가지인 프림 알고리즘 형태인 듯 하다. 하지만 정렬한 후 인덱스를 통해 접근해야한다는 점이 문제를 간혹 헷갈리게 할 수 있을 것 같다.**

<br/>

<br/>

#### 코드 : 

```c++
#include <iostream>
#include <vector>
#include <utility>
#include <algorithm>
#include <queue>
#include <cstdlib>

#define pii pair<int, int>
#define ll long long

using namespace std;
struct Planet {
    int idxX, idxY, idxZ;
};
vector<pii > sortX, sortY, sortZ;
vector<Planet> sortInfos;
bool connected[100000];
int n;

ll makeMST() {
    priority_queue<pii, vector<pii >, greater<>> pq;
    ll distSum = 0;
    int count = 0;

    pq.push(pii(0, 0));

    while (!pq.empty()) {
        int curIdx = pq.top().second, curDist = pq.top().first;
        pq.pop();

        if (connected[curIdx]) continue;
        distSum += curDist;
        connected[curIdx] = true;
        if (++count >= n) return distSum;

        if (sortInfos[curIdx].idxX - 1 >= 0)
            pq.push(pii(abs(sortX[sortInfos[curIdx].idxX].first - sortX[sortInfos[curIdx].idxX - 1].first), sortX[sortInfos[curIdx].idxX - 1].second));
        if (sortInfos[curIdx].idxX + 1 < n)
            pq.push(pii(abs(sortX[sortInfos[curIdx].idxX].first - sortX[sortInfos[curIdx].idxX + 1].first), sortX[sortInfos[curIdx].idxX + 1].second));
        if (sortInfos[curIdx].idxY - 1 >= 0)
            pq.push(pii(abs(sortY[sortInfos[curIdx].idxY].first - sortY[sortInfos[curIdx].idxY - 1].first), sortY[sortInfos[curIdx].idxY - 1].second));
        if (sortInfos[curIdx].idxY + 1 < n)
            pq.push(pii(abs(sortY[sortInfos[curIdx].idxY].first - sortY[sortInfos[curIdx].idxY + 1].first), sortY[sortInfos[curIdx].idxY + 1].second));
        if (sortInfos[curIdx].idxZ - 1 >= 0)
            pq.push(pii(abs(sortZ[sortInfos[curIdx].idxZ].first - sortZ[sortInfos[curIdx].idxZ - 1].first), sortZ[sortInfos[curIdx].idxZ - 1].second));
        if (sortInfos[curIdx].idxZ + 1 < n)
            pq.push(pii(abs(sortZ[sortInfos[curIdx].idxZ].first - sortZ[sortInfos[curIdx].idxZ + 1].first), sortZ[sortInfos[curIdx].idxZ + 1].second));

    }
}

int main() {
    int posX, posY, posZ;
    cin >> n;

    sortInfos.resize(n);
    for (int k = 0; k < n; k++) {
        cin >> posX >> posY >> posZ;
        sortX.emplace_back(posX, k);
        sortY.emplace_back(posY, k);
        sortZ.emplace_back(posZ, k);
    }

    sort(sortX.begin(), sortX.end());
    sort(sortY.begin(), sortY.end());
    sort(sortZ.begin(), sortZ.end());

    for (int k = 0; k < n; k++) {
        sortInfos[sortX[k].second].idxX = k;
        sortInfos[sortY[k].second].idxY = k;
        sortInfos[sortZ[k].second].idxZ = k;
    }

    cout << makeMST() << '\n';
    return 0;
}
```

