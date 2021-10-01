# 3678. 카탄의 개척자

###### 작성자 : O-star

<br/>

<br/>

### 풀이 : 

**풀이 방식은 생각보다 빨리 깨우쳤지만 생각치 못한 조건을 고려해주지 않아 꽤나 오래 시간이 걸린 문제이다.**

**문제는 육각형의 층(겹)을 활용해서 해결하였다.**

**각 육각형은 안의 겹의 육각형 칸을 일정한 규칙을 가지고 맞닿아있다. 그 규칙은 한 변을 기준으로 해당 겹의 순서만큼은 안 쪽 겹의 두 칸과 맞닿아있으며 마지막의 꼭지점에 해당하는 칸은 한 칸과 맞닿아있다.**

**이 규칙을 활용하여 바깥의 겹의 숫자를 결정할 때 안쪽의 육각형을 두 개씩 비교해보고 꼭지점에 해당하는 칸만 한 칸씩 비교하면 문제를 해결할 수 있다.**

**단, 내가 실수한 부분은 같은 층(겹)에서 맞닿아있는 칸을 고려해주지 않아 계속 실패가 떴다.**

**즉 가장 처음 칸부터 마지막 전 칸까지는 직전에 결정한 숫자와 겹치면 안되고 마지막의 칸은 직전 숫자와 처음 숫자 두 개와 겹치면 안되게 구현한다.**

<br/>

<br/>

#### 코드 : 

```c++
#include <iostream>
#include <vector>
#include <cmath>

using namespace std;
int resourceCnt[6];
vector<vector<int>> spiral(100);

void makeSpiral() {
    int depth = 0, totalCnt = 1;
    spiral[0] = {1};
    resourceCnt[1]++;

    while (++depth) {
        int pastLength = spiral[depth - 1].size(), adjIdx = pastLength - 1, curLength = depth * 6;
        while (spiral[depth].size() < curLength) {
            for (int t = 1; t < depth; t++) {
                int cmp1 = spiral[depth - 1][adjIdx], cmp2 = spiral[depth - 1][(adjIdx + 1) % pastLength];
                int minCnt = 10000, minNum;
                for (int k = 1; k < 6; k++) {
                    if (k == cmp1 || k == cmp2 || (!spiral[depth].empty() && k == spiral[depth].back())) continue;
                    if (minCnt > resourceCnt[k]) {
                        minCnt = resourceCnt[k];
                        minNum = k;
                    }
                }
                spiral[depth].push_back(minNum);
                resourceCnt[minNum]++;
                adjIdx = (adjIdx + 1) % pastLength;
            }
            int cmp = spiral[depth - 1][adjIdx];
            int minCnt = 10000, minNum;
            for (int k = 1; k < 6; k++) {
                if (k == cmp || (!spiral[depth].empty() && k == spiral[depth].back())) continue;
                if (spiral[depth].size() == curLength - 1 && k == spiral[depth][0]) continue;
                if (minCnt > resourceCnt[k]) {
                    minCnt = resourceCnt[k];
                    minNum = k;
                }
            }
            spiral[depth].push_back(minNum);
            resourceCnt[minNum]++;
        }

        totalCnt += curLength;
        if (totalCnt >= 10000) break;
    }
}

int main() {
    makeSpiral();

    int tc, num;
    cin >> tc;
    while (tc--) {
        cin >> num;
        for (int depth = 0; depth < 100; depth++) {
            int curDepthLength = depth ? depth * 6 : 1;
            if (num <= curDepthLength) {
                cout << spiral[depth][num - 1] << '\n';
                break;
            }
            num -= curDepthLength;
        }
    }
    return 0;
}
```

