# 16985. Maaaaaaaaaze

###### 작성자 : O-star

<br/>

<br/>

#### 풀이 : 

**문제가 정말 괜찮았던 것 같다.**

**이 정도 수준이면 사실 웬만한 코테에서도 출제될만큼 괜찮은 난이도였지 않나 생각한다.**

**문제는 미로 탐색이기 때문에 기본적으로 bfs 방식을 활용하지만 사실은 bfs 구현보다는 미로의 각 평면을 재배치하고 회전시켜주는 경우의 수를 잘 구현하여 완전 탐색을 시행해주는 것이 주된 문제였던 것 같다. 또한 미로 크기 자체가 크지도 않기 때문에 시간복잡도보다는 구현에 사실 더 초점이 가는 문제가 아니였나 생각한다.**

**<br/>**

**세부 구현사항 :** 

- **우선 처음에 문제를 정확하게 이해하지 못해 미로의 각 평면을 회전시켜주는 경우의 수만을 모두 따져 bfs를 해주면 되는 줄 알았다. 하지만 뒤늦게 알고 보니 각 평면의 층도 다시 재배치해서 경우의 수를 따져주어야 했다.**
- **각 평면의 층 재배치의 경우 일반적인 재귀 방식으로 순서를 재배치해주면 된다. 층이 총 5개이기에 재귀함수를 5번 반복호출하여 해당 순서를 정해준다. => 난 본 문제에서는 미로 맵을 3차원 벡터로 구현하여 층 재배치시에는 오리지널 맵 층에서 한 층씩 재배치하여 새로운 map 벡터를 생성해주었다. 하지만 사실 단순히 재배치 순서를 저장해둔 크기 5의 배열만을 생성해 bfs 탐색에서 z축 이동 시, 본 배열을 참조하는 방식이 시간, 공간 복잡도 측면에서 훨씬 효율적이라는 생각이 든다.**
- **층 재배치가 끝날 경우 각 층의 회전 경우의 수를 모두 고려해주어야 한다. 회전의 경우 0, 90, 180, 270 총 4가지의 경우의 수가 있기 때문에 회전 경우의 수는 4^5 = 1024가지의 경우의 수가 존재한다. 회전 구현에는 `rotatemap[4-j][i] = map[i][j]` 의 수식을 활용하여 90도 회전을 구현해주었다.**
- **1024가지의 경우의 수를 모두 bfs 탐색을 진행해줄 필요는 없다. (0,0,0) => (4,4,4)로 향할 수 있는 미로의 최소 이동 경로가 존재하는지 bfs를 탐색해주는 것이기 때문에 (0,0,0), (4,4,4) 두 위치는 무조건 map에서 true 값을 가져야 한다. 이렇듯 출발지와 도착지가 우선적으로 이동할 수 있는 인덱스일 경우를 판단하고 BFS 탐색을 모두 진행해주어 최소 이동경로를 파악한다.**

<br/>

<br/>

#### 코드 : 

```c++
#include <iostream>
#include <queue>
#include <string>
#include <algorithm>
#include <cmath>
#include <vector>

#define ll long long

using namespace std;
vector<ll> numbervec;
vector<char> operatorvec;
ll maxresult;
char opary[3] = {'*', '+', '-'};
bool usedop[3];

ll charToCalculResult(ll a, ll b, char op) {
    switch (op) {
        case '*':
            return a * b;
        case '+':
            return a + b;
        case '-':
            return a - b;
    }
}

void calculExpression(string priorities) {
    queue<ll> numberq;
    queue<char> operatorq;

    int size = numbervec.size();
    for (int i = 0; i < size; i++)
        numberq.push(numbervec[i]);
    size = operatorvec.size();
    for (int i = 0; i < size; i++)
        operatorq.push(operatorvec[i]);

    for (int i = 0; i < 3; i++) {
        char orderopt = priorities[i];
        int size = operatorq.size();
        ll operand1 = numberq.front();
        numberq.pop();
        while (size--) {
            ll operand2 = numberq.front();
            numberq.pop();
            char curopt = operatorq.front();
            operatorq.pop();

            if (orderopt == curopt) operand1 = charToCalculResult(operand1, operand2, curopt);
            else {
                numberq.push(operand1);
                operatorq.push(curopt);
                operand1 = operand2;
            }
        }
        numberq.push(operand1);
    }
    maxresult = max(maxresult, abs(numberq.front()));
}

void selectPriority(string orderstr) {
    if (orderstr.length() == 3) {
        calculExpression(orderstr);
        return;
    }

    for (int i = 0; i < 3; i++) {
        if (usedop[i]) continue;
        usedop[i] = true;
        selectPriority(orderstr + opary[i]);
        usedop[i] = false;
    }
}

long long solution(string str) {
    string cmpstr = "";
    
    for (int i = 0; i < str.length(); i++) {
        if (str[i] == '*' || str[i] == '+' || str[i] == '-') {
            numbervec.push_back(stoll(cmpstr));
            cmpstr = "";
            operatorvec.push_back(str[i]);
        } else cmpstr += str[i];
    }
    numbervec.push_back(stoll(cmpstr));

    selectPriority("");
    return maxresult;
}
```

