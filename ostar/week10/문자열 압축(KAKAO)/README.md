# 문자열 압축 (2020 KAKAO BLIND)

###### 작성자 : O-star

<br/>

<br/>

### 풀이 : 

**카카오 코테에서 1번으로 출제되는 대표적인 문자열 처리 문제인 듯 하다.**

**문자열 문제 치고는 엄청 쉬운? 문제같지는 않지만 문자열 분할과 완전 탐색 방법을 사용하면 다른 문제들보다 비교적 쉽게 답을 도출해낼 수 있는 듯 하다.**

**문자열 압축은 문제에서 설명한 바와 같이 단위 문자 갯수만큼 앞에서부터 분할하여 연속된 분할 문자열들이 중복된다면 합쳐서 `(중복 문자열 갯수)(분할 문자열)` 형태로 만든다. 여기서 단위 문자 범위는 1~(제공 문자열 길이 / 2) 이다. (단위 문자 갯수가 제공 문자열 길이 / 2 보다 커지면 비교할 문자열이 1개 밖에 안나오기 때문에 굳이 반복 탐색해 줄 필요 없음)**

**curStr 변수에 현재 중복을 검사할 문자열을 검사하고 cmpStr에 다음번 분할 문자열을 저장한다. curStr과 cmpStr이 서로 같다면 sameCnt를 1 증가시키고 같지 않다면 curStr을 cmpStr로 바꾸고 현재까지 쌓인 sameCnt를 문자열로 바꾼 length와 단위 문자열을 누적 길이에 더해서 계산해준다.**

**이렇게 1~(제공 문자열 길이 / 2) 범위동안 문자열 압축과정을 진행하여 가장 짧은 압축 문자열 길이를 도출해낼 수 있다.**

<br/>

<br/>

### 코드 : 

```c++
#include <string>
#include <vector>
#include <algorithm>

using namespace std;

int solution(string s) {
    int strLength = s.length(), splitRange = strLength / 2, answer = strLength;

    for (int splitnum = 1; splitnum <= splitRange; splitnum++) {
        string curStr = s.substr(0, splitnum);
        int startIdx = splitnum, sameCnt = 1, reduceLength = 0;

        while (true) {
            string cmpStr = s.substr(startIdx, splitnum);
            if (curStr == cmpStr) {
                sameCnt++;
            } else {
                curStr = cmpStr;
                if (sameCnt == 1) reduceLength += splitnum;
                else reduceLength += splitnum + to_string(sameCnt).length();
                sameCnt = 1;
            }

            if (strLength - startIdx < splitnum) {
                reduceLength += strLength - startIdx;
                break;
            }
            startIdx += splitnum;
        }

        answer = min(answer, reduceLength);
    }

    return answer;
}
```

