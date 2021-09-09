# 순위 검색 (2021 KAKAO BLIND)

###### 작성자 : O-star

<br/>

<br/>

### 풀이 : 

**데이터베이스에서 조건에 따라 데이터를 검색하는 것을 알고리즘을 구현해야하는 듯한 느낌이 드는 문제였다.**

**문제 자체에 엄청 어려운 로직이 숨어져 있는 문제이진 않았지만, 각 조건(개발언어, 지원직군, 경력, 선호음식)에 따라 인덱스를 잘 배정해주는 것이 효율성을 높이는 가장 큰 기준이었지 않나 생각한다.**

**개인적으로는 각 조건별로 map을 생성해주어 인덱스를 배정해주었고 해당 인덱스들을 하나의 문자열로 이어준 후, 이 인덱스 문자열에 해당하는 진짜 인덱스 번호를 저장하는 벡터에 다시 한번 검색해준 후 실제 저장 벡터의 인덱스를 찾아가는 방법을 사용했다.**

**인덱스를 정해주는 더 좋은 방법이 있을 거라 생각한다. 좀 더 생각해보고 효율적인 방법으로 한 번 더 풀어봐야겠다는 생각이 든다.**

<br/>

<br/>

### 코드 : 

```c++
#include <string>
#include <vector>
#include <sstream>
#include <map>
#include <utility>
#include <algorithm>

using namespace std;
vector<vector<int>> sortInput(109);
map<string, string> languageMap = {
        {"cpp",    "1"},
        {"java",   "2"},
        {"python", "3"},
        {"-",      "4"}
};
map<string, string> fieldMap = {
        {"backend",  "1"},
        {"frontend", "2"},
        {"-",     "3"}
};
map<string, string> careerMap = {
        {"junior", "1"},
        {"senior", "2"},
        {"-",      "3"}
};
map<string, string> foodMap = {
        {"chicken", "1"},
        {"pizza",   "2"},
        {"-",       "3"}
};
vector<string> queryIdxMap = {
        "1111", "1112", "1113", "1121", "1122", "1123", "1131", "1132", "1133",
        "1211", "1212", "1213", "1221", "1222", "1223", "1231", "1232", "1233",
        "1311", "1312", "1313", "1321", "1322", "1323", "1331", "1332", "1333",
        "2111", "2112", "2113", "2121", "2122", "2123", "2131", "2132", "2133",
        "2211", "2212", "2213", "2221", "2222", "2223", "2231", "2232", "2233",
        "2311", "2312", "2313", "2321", "2322", "2323", "2331", "2332", "2333",
        "3111", "3112", "3113", "3121", "3122", "3123", "3131", "3132", "3133",
        "3211", "3212", "3213", "3221", "3222", "3223", "3231", "3232", "3233",
        "3311", "3312", "3313", "3321", "3322", "3323", "3331", "3332", "3333",
        "4111", "4112", "4113", "4121", "4122", "4123", "4131", "4132", "4133",
        "4211", "4212", "4213", "4221", "4222", "4223", "4231", "4232", "4233",
        "4311", "4312", "4313", "4321", "4322", "4323", "4331", "4332", "4333",
};

vector<string> split(string input, char delimiter) {
    vector<string> answer;
    stringstream ss(input);
    string temp;

    while (getline(ss, temp, delimiter)) {
        answer.push_back(temp);
    }

    return answer;
}

void fillSortVec(int curIdx, string queryNum, int score) {
    if (curIdx >= 4) {
        sortInput[find(queryIdxMap.begin(), queryIdxMap.end(), queryNum) - queryIdxMap.begin()].push_back(score);
        return;
    }

    fillSortVec(curIdx + 1, queryNum, score);
    if (!curIdx) {
        queryNum[0] = '4';
    } else {
        queryNum[curIdx] = '3';
    }
    fillSortVec(curIdx + 1, queryNum, score);
}

vector<int> solution(vector<string> info, vector<string> query) {
    vector<int> answer;

    for (string str: info) {
        vector<string> splitVec = split(str, ' ');
        string queryNum = "";
        queryNum += languageMap.find(splitVec[0])->second;
        queryNum += fieldMap.find(splitVec[1])->second;
        queryNum += careerMap.find(splitVec[2])->second;
        queryNum += foodMap.find(splitVec[3])->second;
        fillSortVec(0, queryNum, stoi(splitVec[4]));
    }

    for (int k = 0; k < 109; k++) {
        sort(sortInput[k].begin(), sortInput[k].end());
    }

    for (string str: query) {
        vector<string> splitQuery = split(str, ' ');
        string queryNum = "";
        queryNum += languageMap.find(splitQuery[0])->second;
        queryNum += fieldMap.find(splitQuery[2])->second;
        queryNum += careerMap.find(splitQuery[4])->second;
        queryNum += foodMap.find(splitQuery[6])->second;

        int queryIdx = find(queryIdxMap.begin(), queryIdxMap.end(), queryNum) - queryIdxMap.begin();
        answer.push_back(sortInput[queryIdx].end() -
                         lower_bound(sortInput[queryIdx].begin(), sortInput[queryIdx].end(), stoi(splitQuery[7])));
    }

    return answer;
}
```

