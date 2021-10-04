# 16565. N포커

###### 작성자 : O-star

<br/>

<br/>

### 풀이 : 

**우선 다른 분들의 코드를 보니 시간 보잡도에 훨씬 효율적인 풀이 방식이 존재했다.**

**나의 풀이방식만 우선 정리해보았다.**

**우선 나는 처음부터 포카드의 경우의 수 조합을 셀려고 했으나 포카드의 쌍으로 조합을 계산할 경우 중복으로 세어지는 경우가 존재하기 때문에 애초에 전체 경우의 수에서 포카드 쌍이 생기지 않는 경우의 수를 빼는 것이 구현하기 더 편하다고 생각했다.**

**따라서 뽑는 카드의 수 N개가 주어질 경우 totalCase() 함수를 통해 전체 경우의 수를 구해준 후 loseCase() 함수로 포카드가 생기지 않는 경우의 수를 빼주게 구현하였다.**

**여기서 포카드가 생기지 않는 경우의 수는 포카드가 생기는 경우의 수보다 비교적 구현하기 쉽다. 왜냐하면 같은 숫자, 문양 조합에서 4개 이상 선택하지 않도록 0~3개 선택하는 경우의 수를 세면 되기 때문이다.**

**단, 시간 복잡도 면에서 포카드가 생기는 경우의 수에서 중복으로 센 경우의 수를 빼는 것이 훨씬 효율적이었다.**

**추가적으로 조합을 재귀로 구현하는 방법을 더 공부할 필요가 있다고 생각했다.**

<br/>

<br/>

#### 코드 : 

```c++
#include <iostream>

#define ll long long
#define DVD 10007

using namespace std;
int loseDp[14][53], oneCase[4] = {1, 4, 6, 4};

int totalCase(int n) {
    ll total = 1;
    int dvdNum = 2, num = 52;
    for (int k = 0; k < n; k++) {
        while (dvdNum <= n && !(total % dvdNum))
            total /= dvdNum++;
        total *= num--;
    }
    return total % DVD;
}

int loseCase(int index, int remain) {
    if (!remain) return 1;
    if (index == 14) return 0;
    if (loseDp[index][remain]) return loseDp[index][remain];

    int curTotal = 0, curRemain = remain >= 3 ? 3 : remain;
    for (int k = 0; k <= curRemain; k++)
        curTotal = (curTotal + loseCase(index + 1, remain - k) * oneCase[k]) % DVD;
    return loseDp[index][remain] = curTotal;
}

int main() {
    int n;
    cin >> n;
    int total = totalCase(n);
    int loseTotal = loseCase(1, n);
    int answer = totalCase(n) - loseCase(1, n);
    cout << answer + (answer < 0 ? DVD : 0) << '\n';
    return 0;
}
```

