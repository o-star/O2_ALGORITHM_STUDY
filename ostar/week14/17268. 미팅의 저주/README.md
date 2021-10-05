# 17268. 미팅의 저주

###### 작성자 : O-star

<br/>

<br/>

### 풀이 : 

![image](https://user-images.githubusercontent.com/57346455/135965894-ddc4420c-4426-4392-adb0-208f090b18f2.png)

**다음 문제는 그림으로 원탁을 그려가면서 이해하면 빠르게 점화식을 구할 수 있는 문제이다.**

**문제는 원탁형이기 때문에 기준 사람이 임의로 매칭되는 짝에 의해 두 부분으로 분할 되게 된다.**

**이렇게 분할되 두 파트는 각각 n명보다 적은 짝수가 되기 때문에 해당 부분을 memoization으로 통해 답을 도출해낼 수 있다.**

**점화식 : dp[t] = dp[t] * dp[t - k - 2] (0 <= k < t)**

<br/>

<br/>

#### 코드 : 

```c++
#include <iostream>

#define MOD 987654321
#define ll long long

using namespace std;
ll dp[10001];

int main() {
    int n;
    cin >> n;

    dp[0] = 1;
    dp[2] = 1;
    for (int t = 4; t <= n; t++) {
        for (int k = 0; k < t; k += 2) {
            dp[t] = (dp[t] + dp[k] * dp[t - k - 2]) % MOD;
        }
    }
    cout << dp[n] << '\n';
    return 0;
}
```

