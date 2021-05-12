# 수식 최대화 [2020 카카오 인턴십]

###### 작성자 : O-star

<br/>

<br/>

#### 풀이 : 

**사칙연산에서 연산자의 우선순위를 달리해서 가장 큰 계산값을 도출해내는 문제이다.**

**처음에는 아무래도 사칙연산 문제였기 때문에 전위, 중위, 후위식으로 바꿔가는 형식으로 스택을 활용하는 문제라고 예상했다.**

**하지만 주어진 수식의 형태를 바꾸는 방식이 아닌 연산자의 우선순위에 따라 앞에서부터 차례로 계산을 해가야 했기에 스택보다는 큐를 사용해야된다는 생각이 들었다.**

**그래서 본 문제는 기본적으로 연산자의 우선순위를 완전탐색의 재귀형식을 사용해 구현해준 후 해당 우선순위 순서대로 큐를 사용해 연산 결과값을 계산해주는 방식을 사용했다.**

**<br/>**

**세부 풀이사항**

1. **문자열 연산자, 피연산자 분리 후 저장 : 문제에서 수식은 하나의 문자열로 주어진다. 따라서 연산자와 숫자로 나누어 주어야 한다. 본 문제에서는 음수 형태의 숫자는 제공되지 않다고 가정하고 있기 때문에 무조건 연산자 ('*', '+', '-') 3개를 만나게 되면 현재까지 확인한 앞 숫자들을 모두 long long 형태로 바꾸어 numbervec 벡터에 저장해준다. 연산자 또한 operatorvec에 저장하고 앞 숫자들을 저장해둔 cmpstr은 초기화 시켜주고 문장의 끝까지 반복한다.**
2. **연산자 우선순위 결정 : 3개의 연산자가 해당 수식에 존재할 수 있기 때문에 총 3! = 6가지의 우선순위 경우의 수를 확인해 준다. 물론 3개의 연산자 중 2개 혹은 1개만 사용될 수 있지만 이런 케이스를 따로 분리해 코딩하는 것보다 합쳐서 수식을 계산해줘도 별 다른 문제가 없고 시간복잡도에도 영향을 미치지 않기 때문에 크게 6가지 경우의 수를 모두 따져주었다.**
3. **수식 계산 결과 도출 : 가장 중요한 로직이다. 벡터에 쌓아둔 연산자와 피연산자 집합 각각은 numberq와 operatorq에 저장한다. 그리고 현재 큐에 쌓인 연산자 갯수만큼 반복문을 돌며 operatorq.front()의 값이 현재 계산해야할 우선순위 연산자와 동일하다면 해당 연산자를 계산해준다. 이렇게 반복적으로 큐에서 빼고 다시 계산한 결과나 그대로의 숫자, 연산자를 큐에 넣어주는 과정을 반복하게 되면 (계산해주어야 할 연산자 갯수) * (연산자의 종류 갯수(3개)) 만큼의 적은 반복만으로 해당 수식의 계산 결과를 도출할 수 있다.**

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

