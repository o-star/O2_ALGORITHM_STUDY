# 2800. 괄호 제거

###### 작성자 : O-star

<br/>

<br/>

### 풀이 : 

**카카오 코테 2차를 위해 파이썬으로 문제를 해결하였다.**

**우선 문제 자체는 스택과 재귀 함수 구현으로 해결할 수 있는 어렵지 않은 문제이다.**

**우선 왼쪽 괄호의 위치를 모두 담은 leftBracketList list를 준비하고 재귀 함수를 통해 해당 괄호를 없앨 것인가 없애지 않을 것인가 selectList 비트마스크를 만들어내는 과정을 거친다.** 

**이 과정 후, 스택을 활용하여 없애야 하는 괄호에 해당할 경우 왼쪽 괄호와 오른쪽 괄호를 제외시킨 문자열을 만드는 makeEraseStr 함수를 수행한다.**

**중복되는 문자열이 만들어질 경우에는 만든 문자열을 저장하는 answerList가 set 형태이기 때문에 해결할 수 있다. 이후로 sort 과정을 거쳐 사전 순으로 출력한다.**

**파이썬 문법 enumerate에 익숙해져보자.**

<br/>

<br/>

#### 코드 : 

```python
inputStr = input()
leftBracketList = []
answerList = set()
bracketNum = 0


def makeEraseStr(selectList):
    if selectList == 0:
        return

    global answerList
    stack = []
    changeStr = ""
    for idx, ch in enumerate(inputStr):
        if ch == '(':
            stack.append(idx)
            if(selectList & (1 << leftBracketList.index(idx)) == 0):
                changeStr += ch
        elif ch == ')':
            if(selectList & (1 << leftBracketList.index(stack[-1])) == 0):
                changeStr += ch
            stack.pop()
        else:
            changeStr += ch

    answerList.add(changeStr)


def selectEraseLeft(selectList, curIdx):
    if curIdx == bracketNum:
        makeEraseStr(selectList)
        return

    selectEraseLeft(selectList, curIdx + 1)
    selectEraseLeft(selectList | (1 << curIdx), curIdx + 1)


for idx, ch in enumerate(inputStr):
    if ch == '(':
        leftBracketList.append(idx)
bracketNum = len(leftBracketList)

selectEraseLeft(0, 0)
answerList = list(answerList)
answerList.sort()
for str in answerList:
    print(str)

```

