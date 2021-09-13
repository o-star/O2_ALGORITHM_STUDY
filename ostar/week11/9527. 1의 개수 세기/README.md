# 9527. 1의 개수 세기

###### 작성자 : O-star

<br/>

<br/>

### 풀이 : 

**문제 접근이 잘 되지 않아 결국 풀이법을 보고 공부하면서 풀어나갔던 것 같다.**

**비트마스킹을 사용해야 할 것 같다는 생각은 했으나, 누적합을 어떤 형식으로 저장시키고 활용하여야 할지 감이 오지 않았다.**

**<br/>**

**[ 세부 구현사항 ]**

- **누적합은 모든 숫자를 2진수로 나타내보면 반복되는 누적합이 발생한다는 것을 알 수 있다.**

- **2진수에서 1의 개수 합은 (2의 지수) - 1 의 숫자마다 일정한 규칙을 가진다. `  accAry[k] = accAry[k - 1] * 2 + 2^k` (accAry[k]: 최상위 비트가 k인 숫자까지의 1의 개수 합. accAry[3]은 15(1111(2))까지 1의 개수 누적합을 나타냄)**

- **이렇게 구한 accAry 배열을 사용해서 A와 B의 누적합을 구해주어야 한다. 답은 (B 1의 개수 누적합) - ([A - 1] 1의 개수 누적합) 이다.**

- **특정 숫자 1의 개수 누적합 공식 :** 

  ```c++
      count += accAry[t - 1] + number - ((ll) 1 << t) + (ll) 1;
      number -= ((ll) 1 << t);
  //	number = 특정 숫자
  ```

<br/>

<br/>

#### 코드 : 

```c++
#include <iostream>
#include <vector>

#define ll long long

using namespace std;
ll accAry[55];

vector<int> changeBinaryNumber(ll num) {
    vector<int> binaries;
    while (num) {
        binaries.push_back(num % 2);
        num /= 2;
    }
    return binaries;
}

ll countOne(vector<int> *binaries, ll number) {
    ll count = 0;

    if (!number) return 0;

    for (int t = (*binaries).size() - 1; t > 0; t--) {
        if (!(*binaries)[t]) continue;
        count += accAry[t - 1] + number - ((ll) 1 << t) + (ll) 1;
        number -= ((ll) 1 << t);
    }
    count += (*binaries)[0];
    return count;
}

int main() {
    ll start, end, startCnt = 0, endCnt = 0;
    accAry[0] = 1;
    cin >> start >> end;
    start--;

    for (int t = 1; t < 55; t++) {
        accAry[t] = accAry[t - 1] * 2 + ((ll) 1 << t);
    }

    vector<int> endBinaries = changeBinaryNumber(end);
    endCnt = countOne(&endBinaries, end);
    vector<int> startBinaries = changeBinaryNumber(start);
    startCnt = countOne(&startBinaries, start);

    cout << endCnt - startCnt << '\n';

    return 0;
}
```

