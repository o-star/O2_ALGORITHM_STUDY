# 후보키 (2019 카카오 블라인드)

###### 작성자 : O-star

<br/>

<br/>

### 풀이 : 

**문제는 비트 마스킹을 잘 활용해서 후보키 조합을 구현해내고 해당 후보키 조합이 후보키에 해당하는 조합인지 판별해야 하는 문제이다 (비트 마스킹을 적절하게 사용하여야 문제 해결이 쉬운 듯 하다.)**

**또 특정 조합이 후보키에 해당할 경우에는 이 조합에 다른 속성이 추가된 조합은 후보키 조합 테스트를 거치지 않도록 표시를 해주는 impossibility 배열을 활용해 체크해주었다.**

**<br/>**

**[ 세부 구현사항 ]**

- **후보키 조합의 경우의 수는 총 2^8 = 256 가지가 존재한다. (비트마스킹으로 계산시 최대 column 갯수가 8개이기 때문에 최대 경우의 수는 다음과 같다.) 이 경우의 수를 모두 후보키에 적합한지 판단해주더라도 많은 시간이 소요되지 않기 때문에 완전 탐색을 진행해준다.**
- **후보키 조합을 만드는 경우에는 비트마스킹을 활용한다. 또 추가적으로 selected 배열을 사용해 어느 column을 선택한 조합인지 좀 더 사용하기 쉽도록 구현하였다.**
- **키 조합을 만든 이후에는 해당 키 조합이 후보키에 해당하는지 판별해주어야 한다. 이 과정에서는 모든 속성이 string이기 때문에 단순히 선택된 column 속성들을 이어붙인 string을 set을 활용해 중복유무를 판단해주는 식으로 구현했다.**
- **이렇게 해당 키 조합이 후보키에 적합한지 판별한 후 후보키에 해당할 경우에는 카운트를 1 증가 후, 현재 키 조합 비트마스크 숫자 curCandi부터 최대   비트마스크 숫자 256까지의 숫자 중 해당 후보키조합과 중복된 키 조합을 가지는 숫자는 impossibility에 표시해주어 최소성을 위반하는 조합을 배제시켜준다.**

<br/>

<br/>

#### 코드 : 

```c++
#include <string>
#include <vector>
#include <set>

using namespace std;
vector<vector<string>> *relations;
bool impossibility[256], selected[8];
int keyCnt, cols, rows;

void isPossibleCandidateSet(int curCandi) {
    if (impossibility[curCandi]) return;

    set<string> set;

    for (int i = 0; i < rows; i++) {
        string str = "";

        for (int j = 0; j < cols; j++) {
            if (selected[j]) {
                str += (*relations)[i][j];
            }
        }

        if (set.find(str) != set.end()) return;
        set.insert(str);
    }

    keyCnt++;
    for (int num = curCandi; num < 256; num++)
        if ((num & curCandi) == curCandi)
            impossibility[num] = true;
}

void createCandidateSet(int curidx, int curcnt, int totalcnt, int curCandi) {
    if (curcnt == totalcnt) {
        isPossibleCandidateSet(curCandi);
        return;
    }
    if (curidx == cols) return;

    selected[curidx] = true;
    createCandidateSet(curidx + 1, curcnt + 1, totalcnt, curCandi + (1 << curidx));
    selected[curidx] = false;
    createCandidateSet(curidx + 1, curcnt, totalcnt, curCandi);
}

int solution(vector<vector<string>> relation) {
    relations = &relation;
    cols = relation[0].size();
    rows = relation.size();

    for (int k = 1; k <= cols; k++) {
        createCandidateSet(0, 0, k, 0);
    }
    return keyCnt;
}
```

