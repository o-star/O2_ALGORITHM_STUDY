# 1132. 합

###### 작성자 : O-star

<br/>

<br/>

#### 풀이 : 

시험기간이라 계속 알고리즘 공부를 못하다가 오랜만에 풀었는데 음 ... 접근이 꽤나 어려웠다.

이렇다 할 복잡한 로직이 사용되는 문제는 아니였으나, 구상에 많은 시간이 소요되었다.

1. **처음에는 완전 탐색 방법으로 구상했다. 물론 알파벳과 숫자의 매칭에 완전탐색을 사용하면 시간 초과가 날 것이라고 예상은 했으나 효과적인 가지치기를 로직에 포함시키면 시간초과가 나지 않을 수도 있을 것이라 예상했으나 역시 시간초과가 발생했다 ...**
2. **문제 해결에는 생각보다 재밌게 쉬운 로직이 숨어있었다. 어려운 로직이라기 보단 각 알파벳별로 자릿수를 누적 합해주어 저장해둔 후 누적 합이 낮은 순으로 1 -> 9 순으로 대입해주면 되는 문제였다.**
3. **물론 0의 경우, 각 문자열의 가장 앞에 있는 문자가 아닌 경우 중 가장 작은 누적합에 해당하는 알파벳에 대입해주면 문제를 해결할 수 있다.**
4. **사실 문제 분류가 그리디인 것을 보고 힌트를 얻지 않으면 자릿수로 계산해주는 로직을 떠올리기 쉽지 않았다. 연산, 수학 알고리즘 문제를 좀 더 다양하게 풀어보면서 생각의 폭을 넓힐 필요가 있어보인다.**

<br/>

<br/>

#### 코드 : 

```c++
#include <iostream>
#include <string>
#include <algorithm>
#include <cmath>
#include <queue>
#include <utility>

#define pli pair<long long, int>
#define ll long long

using namespace std;
bool poszero[10]; // A to Z possible zero?
ll numpossums[10];

int main() {
    int n;
    ll maxsum = 0;
    priority_queue<pli, vector<pli >, greater<pli > > pq;
    string input;

    cin >> n;
    fill_n(poszero, 10, true);

    while (n--) {
        cin >> input;
        poszero[input[0] - 'A'] = false;
        reverse(input.begin(), input.end());

        int length = input.length();
        for (int k = 0; k < length; k++) {
            numpossums[input[k] - 'A'] += (ll) pow(10, k);
        }
    }

    for (int i = 0; i < 10; i++)
        pq.push(pli(numpossums[i], i));

    int curidx = 1;
    bool zeroused = false;
    while (!pq.empty()) {
        ll cmpsum = pq.top().first;
        int cmpidx = pq.top().second;
        pq.pop();

        if (!zeroused && poszero[cmpidx]) {
            zeroused = true;
            continue;
        } else {
            maxsum += cmpsum * (curidx++);
        }
    }

    cout << maxsum << '\n';
    return 0;
}
```

