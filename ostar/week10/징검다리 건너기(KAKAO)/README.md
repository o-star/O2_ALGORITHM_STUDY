# 징검다리 건너기 (2019 카카오 겨울 인턴십)

###### 작성자 : O-star

<br/>

<br/>

### 풀이 : 

**정확도와 효율성을 모두 따져야 하는 문제이기 때문에 단순히 각 징검다리를 1씩 감소시키며 시뮬레이션을 실행해보는 식의 풀이 방법으로는 시간 복잡도를 맞춰줄 수 없다.**

**그래서 나는 본 문제를 해결할 때 각 돌다리별 밟을 수 있는 이전 돌다리, 다음 돌다리를 저장하는 beforeStones, afterStones 벡터를 준비하였다.**

**<br/>**

**[ 세부 구현사항 ]**

- **보다 자세하게 구현사항을 설명하자면 각 돌다리들이 0에 도달하여 beforeStones, afterStones 벡터 값을 업데이트해 주어야 하는 경우에는 Union-find 알고리즘을 유사하게 활용하여 다음 번, 이전 번 밟을 수 있는 돌다리 번호를 빠르게 업데이트 해주었다.**
- **beforeStones는 각 인덱스가 자신의 인덱스 값을 저장하는 형태로 초기화 한다. 그래서 재귀 방식의 find() 함수에서 이전 번 돌다리를 탐색할 때 특정 인덱스가 자신의 인덱스 값을 저장하고 있는 돌다리에 도달 시 해당 돌다리가 밟을 수 있는 돌다리로 판단한다.**
- **0에 도달하는 돌다리는 우선순위 큐에 모든 돌다리를 삽입한 후 오름차순으로 꺼내면 0이 되는 돌다리들을 순서대로 탐색할 수 있다. 건너갈 수 있는 사람은 더 이상 거너갈 수 없는 경우가 되었을 때 우선순위에서 나온 돌다리의 카운트와 동일하다.**

<br/>

<br/>

### 코드 : 

```c++
#include <string>
#include <vector>
#include <queue>
#include <utility>

#define pii pair<int, int>

using namespace std;
int stoneSize;
vector<int> beforeStones, afterStones;

int findBeforeAfterStone(int index, int nextStone) {
    if (index == -1) return -1;
    afterStones[index] = nextStone;

    if (beforeStones[index] == index) return index;
    return beforeStones[index] = findBeforeAfterStone(beforeStones[index], nextStone);
}

int solution(vector<int> stones, int k) {
    priority_queue<pii, vector<pii >, greater<>> pq;
    int answer = 0;
    stoneSize = stones.size();
    beforeStones.resize(stoneSize + 1);
    afterStones.resize(stoneSize + 1);

    for (int t = 0; t < stoneSize; t++) {
        beforeStones[t] = t;
        afterStones[t] = t + 1;
        pq.push(pii(stones[t], t));
    }
    beforeStones[stoneSize] = stoneSize;
    afterStones[stoneSize] = stoneSize;

    while (!pq.empty()) {
        int emptyStones = pq.top().first, emptyIdx = pq.top().second;
        pq.pop();

        beforeStones[emptyIdx]--;
        int beforeStone = findBeforeAfterStone(emptyIdx, afterStones[emptyIdx]);
        if (k < afterStones[emptyIdx] - beforeStone) return emptyStones;
        answer = emptyStones;
    }

    return answer;
}
```

