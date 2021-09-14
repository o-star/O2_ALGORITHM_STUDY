# 1029. 그림 교환

###### 작성자 : O-star

<br/>

<br/>

### 풀이 : 

**비트마스킹과 DP를 적절하게 활용해서 문제에 접근해야 된다.**

**우선 DFS 형식으로 완전 탐색할 경우 시간초과가 발생할 것이라 예상하고 DFS 경로를 효과적으로 줄이기 위해 DP와 비트 마스킹을 활용했다.**

**</br>**

**[ 세부 구현사항 ]**

- **비트마스크 : 각 아티스트들은 두 번 이상 같은 그림을 구매할 수 없다. 따라서 구매 이력을 저장하고 중복 구매가 일어나지 않게 구현해야 한다. 이를 위해 16개의 비트를 가진 비트마스크 숫자를 만들어 각 비트별로 아티스트의 구매 이력을 표시한다.**
- **DP : 비트마스크를 활용하여 DP 배열을 만든다. `dp[bitmask][curArtist]` : 현재 구매 이력(비트마스크)과 현재 아티스트가 다음과 같을 때 최소 구매 가격.**
- **dp 배열을 활용해서 DFS 탐색 시 현재 저장된 메모이제이션 최소 구매 가격보다 큰 구매 가격으로 이동하려 할 경우에는 해당 경로를 뛰어넘어도 된다. 왜냐하면 이미 더 적은 가격으로 경우의 수를 탐색했기 때문에 더 큰 가격으로 굳이 탐색해볼 필요가 없기 때문이다.**

<br/>

<br/>

#### 코드 : 

```c++
#include <iostream>
#include <algorithm>

#define MAX 100

using namespace std;
int adj[16][16], dp[65600][15], n, maxPeople;

void findMaxPeople(int curArtist, int curPrice, int count, int purchased) {
    if (dp[purchased][curArtist] <= curPrice) return;
    dp[purchased][curArtist] = curPrice;
    maxPeople = max(maxPeople, count);

    for (int k = 1; k <= n; k++) {
        if (purchased & (1 << (k - 1)) || adj[curArtist][k] < curPrice) continue;
        findMaxPeople(k, adj[curArtist][k], count + 1, purchased | (1 << (k - 1)));
    }
}

int main() {
    char input;
    cin >> n;

    for (int k = 0; k < 65600; k++)
        fill_n(dp[k], 15, MAX);

    for (int i = 1; i <= n; i++) {
        for (int j = 1; j <= n; j++) {
            cin >> input;
            adj[i][j] = input - '0';
        }
    }
    findMaxPeople(1, 0, 1, 1);

    cout << maxPeople << '\n';
    return 0;
}
```

