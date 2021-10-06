# 다단계 칫솔 판매 (Programmers)

###### 작성자 : O-star

<br/>

<br/>

### 풀이 : 

**ide 없이 프로그래머스 문제를 풀어보았다.**

**우선 문제 접근 방식은 다음과 같다.**

**[세부 구현사항]**

- **모든 사람들은 이름으로 저장되어있기 때문에 자료 구조를 조직하기 위해선 모든 사람들을 인덱스화해주는 것이 편리하다. 따라서 map을 이용하여 각 사람들의 이름과 인덱스를 저장시켜주어서 활용했다.**
- **트리 형태라고 생각하고 seller의 판매 이력들을 하나씩 확인해가면서 부모 노드로 10%의 값을 올려주고 다시 부모 노드에서 부모 노드의 부모 노드에서 10% 이익을 올려주는 형태로 과정을 진행해준다.**
- **여기서 내가 간과한 점은 칫솔 하나의 가격이 100원이고 최대 판매 수량이 100개이기 때문에 부모 노드로 값을 전달해주는 것은 최대 4번까지만 반복해주면 된다. 여기서 전달 이익 값이 0이 되었을 때에는 반복문을 멈춰주어야 하는데 이 과정을 생략해서 시간 초과를 발생시켰다. 주의하자.**

**ide와 검색 없이 코테를 보기 위해서 STL 라이브러리 숙지와 오타, 실수를 줄이는 것이 큰 효과를 낼 것이다. 연습을 좀 더 신중히 해볼 필요가 있을 것이다.**

<br/>

<br/>

#### 코드 : 

```c++
#include <string>
#include <vector>
#include <map>
#include <utility>

#define psi pair<string, int>

using namespace std;

vector<int> solution(vector<string> enroll, vector<string> referral, vector<string> seller, vector<int> amount) {
    int memberNum = enroll.size(), unitGain = 100;
    vector<int> answer(memberNum, 0);
    map<string, int> enrollMap;
    
    for(int k=0; k<memberNum; k++)
        enrollMap.insert(psi(enroll[k], k));
    
    int sellerSize = seller.size();
    for(int k=0; k<sellerSize; k++) {
        int curMember = enrollMap[seller[k]], curAmount = amount[k] * unitGain;
        while(curAmount) {
            int giveAmount = curAmount / 10, gainAmount = curAmount - giveAmount;
            answer[curMember] += gainAmount;
            
            if(referral[curMember] == "-") break;
            curMember = enrollMap[referral[curMember]];
            curAmount = giveAmount;
        }
    }
    
    return answer;
}
```

