# 5052. 전화번호 목록

###### 작성자 : O-star

<br/>

<br/>

#### 풀이 : 

**입력 스케일을 보나 문제 형식 자체로 보나 사실 트라이(Trie)를 사용하라고 적혀있는 문제라서 문제 접근에는 크게 어렵지 않았다.**

**하지만 트라이 자체가 사실 보통 알고리즘 문제들에서 쉽게 출제되는 형식이 아니다 보니 오랜만에 구현해보았고 또 구조체에 포인터를 사용해서 구현하다 보니 오히려 구현에 많은 시간이 걸린 문제였다.**

- **테스트 케이스는 최대 50개 전화번호 갯수 최대 10000개, 하나의 전화번호 최대길이 10자리이기 때문에 50 * 10000 * 10 = 5000000으로 O(n)에 문제를 해결하는 것이 원하는 요구사항이 아닐까 생각했고 O(n)의 탐색시간을 가진 트라이를 사용하기로 했다.**
- **우선 문자열을 모두 입력받은 후 길이가 긴 문자열부터 차례로 트라이 구조를 통해 해당 접두사 사용 유무를 체크해주었다. 여기서 길이가 긴 문자열부터 시행해준 이유는 길이가 긴 문자열부터 트라이에 채워넣은 후 짧은 접두사들이 처음부터 끝까지 등장한 문자열일 경우 사용된 접두사라고 판단하기 위해서이다.**
- **구조체의 경우 접두사 사용 유무 bool 변수 하나와 0~9까지의 숫자들의 자식 노드로 뻗어나갈 Node 포인터를 두었다.**

<br/>

<br/>

#### 코드 : 

```c++
#include <iostream>
#include <string>
#include <vector>
#include <algorithm>

using namespace std;

typedef struct Node {
    bool isused = false;
    Node *nodeary = NULL;
};
string phonenumber;

bool isFirstUsing(int curidx, Node *curnode, bool isfirstused) {
    curnode->isused = true;
    if (curidx >= phonenumber.length()) return isfirstused;

    if (curnode->nodeary == NULL) {
        curnode->nodeary = new Node[10];
        return isFirstUsing(curidx + 1, &curnode->nodeary[phonenumber[curidx] - '0'], true);
    } else if (!curnode->nodeary[phonenumber[curidx] - '0'].isused) {
        return isFirstUsing(curidx + 1, &curnode->nodeary[phonenumber[curidx] - '0'], true);
    } else {
        return isFirstUsing(curidx + 1, &curnode->nodeary[phonenumber[curidx] - '0'], isfirstused);
    }
}

void testCase() {
    vector<string> vec;
    string str;
    Node root;
    root.nodeary = NULL;
    int n;
    cin >> n;

    for (int i = 0; i < n; i++) {
        cin >> str;
        vec.push_back(str);
    }

    sort(vec.begin(), vec.end(), greater<>());

    for (int i = 0; i < n; i++) {
        phonenumber = vec[i];
        if (!isFirstUsing(0, &root, false)) {
            cout << "NO\n";
            return;
        }
    }
    cout << "YES\n";
}

int main() {
    int tc;
    cin >> tc;
    while (tc--)
        testCase();
    return 0;
}
```

