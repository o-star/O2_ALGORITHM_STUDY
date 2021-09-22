# 3745. 오름세

###### 작성자 : O-star

<br/>

<br/>

### 풀이 : 

**문제는 LIS (Longest Increasing Subsequence) 증가하는 최장 부분 수열을 구현해봤다면 쉽게 해결할 수 있는 문제였다.**

**하지만 나 역시 LIS 문제를 푼지 오래 지나서인지 기억이 가물가물했다.**

**그래서 이참에 LIS 문제 풀이 방식 2가지를 정리하고 문제를 해결했다.**

**[ LIS ]**

- **LIS의 경우 두 가지 방식으로 풀 수 있다. DP와 이분 탐색 방법으로 풀 수 있는데(세그먼트 트리 방법도 있긴 하지만 오늘 정리는 본 두가지 방식만 정리해보았다.) DP 방식의 경우 O(n^2) 시간 복잡도를 가져서 본 문제에서는 시간 내에 해결할 수 없다. 따라서 이분 탐색을 활용해 문제를 해결해야 한다.**
- **이분 탐색을 사용하는 방식은 수열의 앞에서부터 벡터에 하나씩 집어 넣는 식으로 문제를 해결하는데, 이 과정에서 벡터의 가장 마지막 원소보다 큰 숫자가 들어올 경우 벡터 끝에 삽입해주고 그렇지 않을 경우에는 lower bound 위치의 인덱스에 해당 숫자를 교체해준다(삽입이 아니고 교체임!)**
- **Lower bound : 오름차순 정렬된 배열에서 해당 숫자보다 크거나 같은(이상) 숫자 위치를 말함**
- **Upper bound : 오름차순 정렬된 배열에서 해당 숫자보다 큰(초과) 숫자 위치를 말함**

**본 문제는 전형적인 LIS 문제이기 때문에 위처럼 이분 탐색의 LIS 방식을 사용하면 문제를 해결할 수 있다!**

<br/>

<br/>

#### 코드 : 

```c++
#include <iostream>
#include <vector>

using namespace std;
vector<int> ansvec;
int n;

void binarySearch(int input, int left, int right) {
    if (!ansvec.size() || ansvec[ansvec.size() - 1] < input) {
        ansvec.push_back(input);
        return;
    }
    if (left >= right) {
        if (ansvec[left] > input) ansvec[left] = input;
        return;
    }

    int mid = (left + right) / 2;
    if (ansvec[mid] >= input) binarySearch(input, left, mid);
    else binarySearch(input, mid + 1, right);
}

int main() {
    int input;

    while (cin >> n) {
        ansvec.clear();
        for (int i = 0; i < n; i++) {
            cin >> input;
            binarySearch(input, 0, ansvec.size() - 1);
        }
        cout << ansvec.size() << '\n';
    }
    return 0;
}
```

