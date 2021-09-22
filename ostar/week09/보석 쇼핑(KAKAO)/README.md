# 보석 쇼핑 (2020 카카오 인턴십)

###### 작성자 : O-star

<br/>

<br/>

### 풀이 : 

**C++ STL set, map 라이브러리를 활용하고 투 포인터 알고리즘을 사용하면 해결할 수 있는 문제이다.**

**효율성 테스트를 통과하기 위해 구간의 첫 번째 위치, 마지막 위치를 표시하는 투 포인터 알고리즘을 활용하였다.**

**<br/>**

**[ 세부 구현사항 ]**

- **Set 활용 : 인자로 전달받은 gems 벡터의 문자열들을 set에 차례로 삽입한 후 set의 사이즈를 통해 중복되지 않는 문자열 갯수를 확인한다. (해당 구간에서 중복되지 않는 문자열 모두를 포함하는 체크하기 위해 setSize와 includeCnt(현재 구간에서 포함된 문자열 갯수 저장 변수) 변수를 활용)**
- **Map 활용 : 보석 이름 String을 key로 가지고 현재 구간에서 해당 보석이 몇 개 포함되는지 카운팅하는 int 변수를 value로 가지는 map 자료 구조 활용. (해당 자료구조를 통해 현재 구간의 각 보석 갯수를 파악한다.)**
- **투 포인터 : 투 포인터를 활용해 최소 포함 구간을 구한다. 현재 includeCnt가 setSize보다 작을 경우에는 계속 endPos를 늘려가며 모든 보석을 포함하는 구간을 만들어준다. 모든 보석을 포함하는 구간이 만들어진 후에는 startPos를 1씩 증가시키며 출발 인덱스의 보석을 하나씩 빼가면서 최소 포함 구간을 파악한다. (출발 인덱스 보석을 하나씩 빼는 과정에서 includeCnt와 map을 잘 업데이트하면서 진행해야 함)**

<br/>

<br/>

### 코드 : 

```c++
#include <string>
#include <vector>
#include <utility>
#include <set>
#include <map>

#define psi pair<string, int>

using namespace std;

vector<int> solution(vector<string> gems) {
    vector<int> ansVec(2);
    set<string> gemSet;
    map<string, int> hash;
    for (string str: gems) {
        gemSet.insert(str);
    }

    int setSize = gemSet.size(), gemsSize = gems.size(), includeCnt = 0, minLength = gemsSize + 1;
    int startPos = 0, endPos = 0;
    while (true) {
        if (includeCnt == setSize) {
            while (startPos <= endPos) {
                auto iter = hash.find(gems[startPos]);
                --(iter->second);
                if (!(iter->second)) {
                    int curLength = endPos - startPos;
                    if (minLength > curLength) {
                        minLength = curLength;
                        ansVec[0] = startPos + 1;
                        ansVec[1] = endPos;
                    }
                    includeCnt--;
                    startPos++;
                    break;
                }
                startPos++;
            }
        } else {
            if (endPos >= gemsSize) break;

            auto iter = hash.find(gems[endPos]);
            if (iter == hash.end()) {
                includeCnt++;
                hash.insert(psi(gems[endPos], 1));
            } else if (!(iter->second)) {
                includeCnt++;
                (iter->second)++;
            } else {
                (iter->second)++;
            }
            endPos++;
        }
    }

    return ansVec;
}
```

