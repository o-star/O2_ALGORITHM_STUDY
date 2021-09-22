# 무지의 먹방 라이브 (2019 카카오 블라인드)

###### 작성자 : O-star

<br/>

<br/>

### 풀이 : 

**효율성을 따져야 하는 문제였기 때문에 단순히 순환하면서 1씩 줄여가는 식의 기본 구현으로는 문제를 완벽히 해결할 수 없다.**

**나는 본 문제를 주어진 food_times 벡터를 오름차순으로 정렬해 작은 양의 음식부터 차례로 없어지는 만큼 k를 줄여가면서 반복 횟수를 줄임으로써 효율성을 통과할 수 있었다.**

**<br/>**

**[ 세부 구현사항 ]**

- **주어진 food_times 벡터를 정렬한 sort_times 벡터를 생성한다.**
- **curlow = 현재 가장 작은 양의 음식 sort_times 인덱스, curCycle = 현재 가장 작은 양의 음식이 없어질 때까지 먹는 양(본 음식이 없어질 때까지 순환하면서 먹는 양을 말함)**
- **k는 아직 남은 반복 횟수이기 때문에 k가 curCycle보다 클 경우에는 현재 가장 작은 양의 음식이 없어져도 더 먹어치울 수 있다는 의미이기 때문에 k에서 curCycle을 뺀 후 계속 while문을 돌려준다.**
- **k가 curCycle보다 작을 경우에는 현재 가장 작은 양의 음식이 없어지기 전에 방송이 중단된다. 따라서 while문을 빠져나와 food_times에서 현재 남아있는 음식을 판별해 remain_times 벡터에 저장한 후 remain_times에서 남은 사이클만큼 돌고 난 다음 번 원소를 가져오면 답을 도출할 수 있다.**
- **무지의 먹방 사이클을 기본 반복문을 사용하는 것이 아닌 적은 양의 음식이 사라지는 사이클까지 한 번에 계산해줌으로써 효율성을 높이는 방법이라고 판단하고 구현하였다.**

<br/>

<br/>

#### 코드 : 

```c++
#include <vector>
#include <algorithm>

#define ll long long

using namespace std;

int solution(vector<int> food_times, ll k) {
    vector<int> sort_times = food_times;
    sort_times.push_back(0);
    sort(sort_times.begin(), sort_times.end());

    int orgSize = food_times.size(), curlow = 1;
    while (curlow <= orgSize) {
        ll timeSize = orgSize - curlow + 1;
        ll curCycle = timeSize * (sort_times[curlow] - sort_times[curlow - 1]);
        if (k >= curCycle) {
            k -= curCycle;
            curlow++;
        } else {
            break;
        }
    }

    if (curlow > orgSize) return -1;

    vector<int> remain_times;
    int normnum = sort_times[curlow];
    for (int t = 0; t < orgSize; t++) {
        if (food_times[t] >= normnum) {
            remain_times.push_back(t + 1);
        }
    }

    return remain_times[k % remain_times.size()];
}
```

