# 키패드 누르기 (2020 카카오 인턴십)

###### 작성자 : O-star

<br/>

<br/>

### 풀이 : 

**단순 구현 문제인 듯 하다. 카카오 코딩테스트 문제에서 1번으로 출제되는 난이도 수준인 듯 하다.**

**몇 가지만 구현에 신경을 쓴다면 크게 어렵지는 않은 문제인 듯 하다.**

**[ 세부 구현사항 ]**

- **키패드는 각 열 별로 우선되는 손가락으로 눌러야 한다. 우선 왼쪽 열인 1, 4, 7은 왼쪽 손가락으로, 오른쪽 열인 3, 6, 9는 오른쪽 손가락으로, 중간 열은 두 손가락 중 가까이 있는 손가락으로 눌려져야 한다.**
- **열별로 3으로 나눈 나머지가 같기 때문에 나머지 값을 이용해서 어느 손가락으로 눌러야 할지를 결정한다. => 0의 경우 나머지 값이 오른쪽 열과 같지만 중간 열로 판단할 수 있게끔 빼서 조건을 달아준다.**
- **각 숫자별 행, 열 인덱스 위치 정보를 저장하는 rowary, colary 배열을 두고 코드를 작성하면 좀 더 편한 듯 하다. => 다음 번에 누를 위치가 숫자로 명시되어있기 때문에 쉽기 행 열 정보를 가져와 사용할 수 있도록 배열에 위치 정보를 저장해두는 용도이다.**
- **중앙 열의 숫자 중 동일한 거리에 있는 경우는 왼손잡이, 오른손잡이를 뜻하는 'hand' 변수를 사용하여 조건을 한 개 더 추가해주면 문제를 해결할 수 있다.**

<br/>

<br/>

#### 코드 : 

```c++
#include <string>
#include <vector>
#include <utility>
#include <cmath>

#define pii pair<int, int>

using namespace std;
int rowary[10] = {3, 0, 0, 0, 1, 1, 1, 2, 2, 2}, colary[10] = {1, 0, 1, 2, 0, 1, 2, 0, 1, 2};
pii curleft = pii(3, 0), curright = pii(3, 2);

string solution(vector<int> numbers, string hand) {
    string answer = "";

    int inputlength = numbers.size();
    for (int i = 0; i < inputlength; i++) {
        int curnum = numbers[i];
        if (curnum % 3 == 1) {
            curleft = pii(rowary[curnum], colary[curnum]);
            answer += "L";
        } else if (!(curnum % 3) && curnum) {
            curright = pii(rowary[curnum], colary[curnum]);
            answer += "R";
        } else {
            int leftdif = abs(curleft.first - rowary[curnum]) + abs(curleft.second - colary[curnum]);
            int rightdif = abs(curright.first - rowary[curnum]) + abs(curright.second - colary[curnum]);

            if (leftdif < rightdif || (leftdif == rightdif && hand == "left")) {
                curleft = pii(rowary[curnum], colary[curnum]);
                answer += "L";
            } else {
                curright = pii(rowary[curnum], colary[curnum]);
                answer += "R";
            }
        }
    }

    return answer;
}
```

