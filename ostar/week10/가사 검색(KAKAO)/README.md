# 가사 검색 (2020 KAKAO BLIND)

###### 작성자 : O-star

<br/>

<br/>

### 풀이 : 

**처음에는 문제를 어떻게 접근해야 할지 감이 안 왔던 것 같다. 트라이와 같은 자료구조를 사용해볼려고 했으나 방법이 보이지 않았다.**

**결론적으로 이분탐색의 방법을 활용하여 효율성과 정확성의 점수를 얻어냈다.**

- **우선 키워드는 물음표가 접두에 붙은 경우, 접미에 붙은 경우 두 가지 경우가 존재한다. 두 경우 모두 이분 탐색으로 풀기 위해서는 words 벡터의 단어들을 원형대로 정렬한 벡터 sortWords, reverse 문자열을 정렬한 reverseWords 두 가지 벡터를 준비해야 한다.**
- **각 키워드에 해당하는 문자열은 쉽게 생각해서 물음표 자리에 모두 'a'로 교체한 문자열부터 'z'로 교체한 문자열까지의 범위에 속한 문자열 갯수를 파악하면 도출할 수 있다.**
- **따라서 키워드의 첫 글자부터 물음표로 구성된 접두사 물음표 문자열은 revers 시켜 해당 범위의 단어들이 몇 개가 존재하는지 reverseWords 벡터에서 갯수를 파악한다. 접미어 물음표 문자열은 문자열을 뒤집을 필요 없이 각 물음표를 'a'와 'z'로 교체한 문자열 사이의 문자열 갯수를 파악해주면 답을 도출할 수 있다.**

<br/>

<br/>

### 코드 : 

```c++
#include <string>
#include <vector>
#include <algorithm>

using namespace std;

string makePosKeyword(string orgKeyword, char ch) {
    int orgSize = orgKeyword.length();
    for (int k = 0; k < orgSize; k++)
        orgKeyword[k] = orgKeyword[k] == '?' ? ch : orgKeyword[k];
    return orgKeyword;
}

int calculKeywordRange(vector<vector<string>> *wordVec, string keyword) {
    int keyLength = keyword.length();
    string startKeyword = makePosKeyword(keyword, 'a');
    string endKeyword = makePosKeyword(keyword, 'z');
    int startIdx = lower_bound((*wordVec)[keyLength].begin(), (*wordVec)[keyLength].end(), startKeyword) -
                   (*wordVec)[keyLength].begin();
    int endIdx = upper_bound((*wordVec)[keyLength].begin(), (*wordVec)[keyLength].end(), endKeyword) -
                 (*wordVec)[keyLength].begin();
    return endIdx - startIdx;
}

vector<int> solution(vector<string> words, vector<string> queries) {
    vector<vector<string>> sortWords(10001), reverseWords(10001);
    vector<int> answer;

    for (string word: words) {
        int length = word.length();
        sortWords[length].push_back(word);
        reverse(word.begin(), word.end());
        reverseWords[length].push_back(word);
    }
    for (int k = 0; k < 10001; k++) {
        sort(sortWords[k].begin(), sortWords[k].end());
        sort(reverseWords[k].begin(), reverseWords[k].end());
    }

    for (string keyword: queries) {
        if (keyword[0] == '?') {
            reverse(keyword.begin(), keyword.end());
            answer.push_back(calculKeywordRange(&reverseWords, keyword));
        } else {
            answer.push_back(calculKeywordRange(&sortWords, keyword));
        }
    }

    return answer;
}
```

