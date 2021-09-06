# 호텔 방 배정 (2019 카카오 겨울 인턴십)

###### 작성자 : O-star

<br/>

<br/>

### 풀이 : 

**union-find 알고리즘을 활용해서 문제를 해결할 수 있다.**

**단, union-find 알고리즘을 설게할 때 배열을 통해 각 root(본 문제에서는 해당 방의 다음 비어있는 방의 번호)를 저장하는 방식을 사용해서는 호텔 방 번호가 최대 10^12 이기 때문에 메모리 초과를 발생시킨다.**

**따라서 나는 본 문제를 해결할 때 C++ STL 라이브러리 Map을 활용하였다. Map에 <room number, next room number> 형태로 저장하여 해당 룸의 다음 빈 룸 번호를 저장했다.**

**이렇게 설계한 Map을 union find 방식을 활용하여 재귀방식 업데이트를 진행하면서 답을 도출했다.**

<br/>

<br/>

### 코드 : 

```c++
#include <string>
#include <vector>
#include <map>
#include <utility>

#define ll long long

using namespace std;
map<ll, ll> roomMap;

ll findEmptyRoom(ll wantRoom) {
    auto iter = roomMap.find(wantRoom);
    if (iter == roomMap.end()) {
        roomMap.insert(pair<ll, ll>(wantRoom, wantRoom + 1));
        return wantRoom;
    } else {
        return iter->second = findEmptyRoom(iter->second);
    }
}

vector<ll> solution(ll k, vector<ll> room_number) {
    int roomSize = room_number.size();
    vector<ll> answer(roomSize);

    for (int k = 0; k < roomSize; k++) {
        answer[k] = findEmptyRoom(room_number[k]);
    }

    return answer;
}
```

