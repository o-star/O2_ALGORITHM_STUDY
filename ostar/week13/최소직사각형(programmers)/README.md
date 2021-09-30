# 최소직사각형 (Programmers)

###### 작성자 : O-star

<br/>

<br/>

### 풀이 : 

**그리디 방식을 사용한 매우 간단한 문제이다.**

**문제에서 주어지는 명함들을 눕히거나 세워서 정렬해서 모두 넣을 수 있는 명함지갑의 크기를 구해야 한다.**

**문제는 간단하다. 모든 명함들을 순서대로 현재까지의 최고 너비, 높이와 현재 명합을 세웠을 때 눕혔을 때 너비 높이를 비교하여 더 작은 크기가 되는 경우를 그리디하게 선택해나가며 답을 도출한다.**

**즉, 해당 명함을 눕힐지 세워서 정렬할지를 즉각적으로 결정해나가면서 답을 도출해나가는 것이다.**

**이 경우, 명함 갯수를 n이라고 할 경우, O(n) 시간복잡도로 문제를 해결할 수 있다.**

<br/>

<br/>

#### 코드 : 

```c++
#include <string>
#include <vector>

using namespace std;

int solution(vector<vector<int>> sizes) {
    int answer = 1000000, cardNum = sizes.size(), maxWidth = 0, maxHeight = 0;

    for (int k = 0; k < cardNum; k++) {
        int width = sizes[k][0], height = sizes[k][1];
        int cmpWidth1 = max(maxWidth, width), cmpHeight1 = max(maxHeight, height);
        int cmpWidth2 = max(maxWidth, height), cmpHeight2 = max(maxHeight, width);
        if (cmpWidth1 * cmpHeight1 <= cmpWidth2 * cmpHeight2) {
            maxWidth = cmpWidth1;
            maxHeight = cmpHeight1;
            answer = cmpWidth1 * cmpHeight1;
        } else {
            maxWidth = cmpWidth2;
            maxHeight = cmpHeight2;
            answer = cmpWidth2 * cmpHeight2;
        }
    }

    return answer;
}
```

