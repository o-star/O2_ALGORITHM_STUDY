# 문자열 압축 (Programmers)

###### 작성자 : O-star

<br/>

<br/>

### 풀이 : 

**ide 없이 프로그래머스 문제를 풀어보았다.**

**문자열 문제의 경우 프로그래머스로 풀면 디버깅 자체가 어렵기 때문에 각별히 문자열 관련 함수와 로직을 신경써서 짤 필요가 있다.**

**본 문제는 반복 문자열을 압축시켜 가장 짧은 문자열을 만들 때 길이를 구해야 한다.**

**문자열의 길이가 최대 1000이기 때문에 완전 탐색으로 단위 문자열이 1 ~ s.length / 2 일 때를 모두 완전 탐색해보더라도 시간복잡도 측면에서는 문제가 없다.**

**따라서 본 문제는 단위 문자열 길이를 1~ s.length / 2 일 때까지 압축시켜나가보며 그 최소 길이를 구하면 된다.**

**최근 코테에서 1~2번 문제에서 거의 필수로 문자열 문제가 나오기 때문에 문자열 관련 함수와 sscanf와 같은 함수 사용법을 정확하게 익혀두자.**

<br/>

<br/>

#### 코드 : 

```c++
#include <string>
#include <vector>
#include <algorithm>

using namespace std;

int solution(string s) {
    int sLength = s.length(), limit = sLength / 2, answer = sLength;
    for(int unit=1; unit<=limit; unit++) {
        string curStr = s.substr(0, unit);
        int startPoint = unit, count = 1, reduceLength = 0;
        while(startPoint < sLength) {
            string cmpStr = s.substr(startPoint, unit);
            startPoint += unit;
            if(curStr != cmpStr) {
                reduceLength += unit;
                if(count != 1) reduceLength += to_string(count).length();
                count = 1;
                curStr = cmpStr;
            } else {
                count++;
            }
        }
        reduceLength += curStr.length() + (count != 1 ? to_string(count).length():0);
        answer = min(answer, reduceLength);
    }
    return answer;
}
```

