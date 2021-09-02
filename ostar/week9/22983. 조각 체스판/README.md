# 22983. 조각 체스판

###### 작성자 : O-star

<br/>

<br/>

### 풀이 : 

**문제 자체의 접근 방식은 빠르게 찾아냈지만 이유 모른 에러 케이스 때문에 상당히 고심해야했던 문제였다.**

**우선 조각 체스판을 찾아내는 DP 문제 유형인데, 비슷한 유형들을 백준에서도 꽤 찾아볼 수 있어서 그런지 문제 해결 방법은 빠르게 찾았던 것 같다.**

**점화식을 살펴보게 되면 누적 정사각형 길이를 각 인덱스에 저장해두기 때문에 결론적으로,**

**`dp[i][j] = min(dp[i + 1][j], dp[i][j + 1], dp[i + 1][j + 1])` 형태로 길이가 2인 작은 정사각형의 최소 누적 정사각형 길이만을 구해주면 된다. => 물론 길이가 2인 최소 정사각형은 체스판의 조건을 만족시키는 조건에 한해서 계산해준다.(맞닿아있는 정사각형의 색깔이 다른 조건)**

**위 조건으로 dp 배열을 채우게 되면 해당 인덱스 칸에서 만들 수 있는 정사각형의 갯수가 모두 구해진다. 결론적으로 dp 배열 인덱스 value들을 모두 더해주면 답을 도출할 수 있다.**

<br/>

<br/>

### 코드 : 

```c++
#include <iostream>
#include <algorithm>

#define ll long long

using namespace std;
int dp[3001][3001];
char board[3001][3001];

int main() {
    ios_base::sync_with_stdio(false);
    cin.tie(NULL);
    cout.tie(NULL);

    int n, m;
    ll answer = 0;
    cin >> n >> m;

    for (int i = 0; i < n; i++) {
        fill_n(dp[i], m, 1);
        for (int j = 0; j < m; j++) {
            cin >> board[i][j];
            if ((i + j) % 2) board[i][j] = (board[i][j] == 'W') ? 'B' : 'W';
        }
    }

    for (int i = 1; i < n; i++) {
        for (int j = 1; j < m; j++) {
            char flag = board[i][j];
            int minVal = 987654321;
            bool isContinue = true;

            for (int k = 0; k < 2; k++) {
                for (int t = 0; t < 2; t++) {
                    if (!k && !t) continue;
                    if (board[i - k][j - t] != flag) {
                        isContinue = false;
                        break;
                    }
                    minVal = min(minVal, dp[i - k][j - t]);
                }
                if (!isContinue) break;
            }

            if (isContinue) {
                dp[i][j] += minVal;
            }
        }
    }

    for (int i = 0; i < n; i++)
        for (int j = 0; j < m; j++)
            answer += static_cast<ll>(dp[i][j]);

    cout << answer << '\n';
    return 0;
}
```

