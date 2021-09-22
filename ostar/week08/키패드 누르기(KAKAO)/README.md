# 키패드 누르기 (2018 카카오 블라인드)

###### 작성자 : O-star

<br/>

<br/>

### 풀이 : 

**부분 문자열을 활용해 두 문자열의 교집합, 합집합을 풀어내는 문제이다.**

**우선 부분 문자열의 경우 두 문자열을 두 글자씩 끊어서 만들어내는데, 여기서 두 글자 모두 알파벳이어야만 부분 문자열로 채택할 수 있다. 또 문제에서 소문자, 대문자를 구분하지 않기 때문에 나는 코드에서 모든 부분 문자열을 대문자로 통일하였다.**

**두 문자열의 교집합은 각 부분 문자열 집합 벡터를 오름차순으로 정렬한 후 앞에서부터 차례로 비교해가면 동일 문자열을 찾아갔다. => 더 작은(사전 순으로 앞에 있는) 문자열일 경우 해당 벡터의 포인터를 1 증가 시킴. 같을 경우에는 두 벡터 포인터 모두 1 증가.**

**이렇게 구한 교집합의 갯수와 합집합 갯수(두 벡터의 사이즈 합 - 교집합 원소 갯수)를 사용하여 답을 도출 할 수 있다. 문제에서 두 문자열의 부분 문자열 집합이 모두 공집합일 경우 1로 간주해야한다는 조건이 제시 되었기 때문에 본 조건을 빼먹지 않고 추가해주어야 한다.**

<br/>

<br/>

#### 코드 : 

```c++
#include <string>
#include <vector>
#include <algorithm>

using namespace std;

bool isAlpabet(char ch) {
    return ('a' <= ch && ch <= 'z') || ('A' <= ch && ch <= 'Z');
}

void makeElements(vector<string> *group, string *str) {
    int range = str->length() - 1;

    for (int k = 0; k < range; k++) {
        if (!isAlpabet((*str)[k])) continue;
        if (!isAlpabet((*str)[k + 1])) k++;
        else group->push_back("" + string(1, toupper((*str)[k])) + string(1, toupper((*str)[k + 1])));
    }
}

int solution(string str1, string str2) {
    vector<string> group1, group2;

    makeElements(&group1, &str1);
    makeElements(&group2, &str2);

    sort(group1.begin(), group1.end());
    sort(group2.begin(), group2.end());

    int intersectCnt = 0, point1 = 0, point2 = 0, range1 = group1.size(), range2 = group2.size();

    if (!range1 && !range2) return 65536;
    while (point1 < range1 && point2 < range2) {
        if (group1[point1] < group2[point2]) point1++;
        else if (group1[point1] > group2[point2]) point2++;
        else {
            intersectCnt++;
            point1++;
            point2++;
        }
    }

    double jaquard = static_cast<double>(intersectCnt) / (range1 + range2 - intersectCnt);
    return jaquard * 65536;
}
```

