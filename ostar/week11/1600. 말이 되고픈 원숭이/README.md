# 1600. 말이 되고픈 원숭이

###### 작성자 : O-star

<br/>

<br/>

### 풀이 : 

**카카오 2차 코테 준비를 위해 파이썬으로 문제를 해결해보았다.**

**문제 자체는 크게 어려운 문제는 아닌 듯 하다. bfs를 활용하여 가장 적은 이동 횟수로 목적지에 도착하는 경우를 찾아야 한다.**

**단 말의 움직임처럼 움직이는 횟수가 최대 K번만큼 허용되기 때문에 이동 경우의 수가 8가지 늘어난다. 이를 위해 기존의 방향을 표현하는 di, dj 배열 말고 diHorse, djHorse 배열을 추가로 만들어 주었다. 그리고 방문 여부를 판단하는 visited 배열 또한 말의 움직임 최대 횟수를 몇 번 사용했는지에 따라 다르게 체크해주어야 하기 때문에 3차원 배열로 만들어 준다.**

**파이썬 문법을 좀 더 공부하자! 카카오 2차 코테에서 문법때문에 못 푸는 일은 없도록 하자**

<br/>

<br/>

#### 코드 : 

```python
from collections import namedtuple, deque

Qnode = namedtuple('Qnode', 'count posX posY remain')

limit = int(input())
width, height = map(int, input().split())
isSuccess = False
board = []
queue = deque()
di = [0, 1, 0, -1]
dj = [1, 0, -1, 0]
diHorse = [1, 2, 2, 1, -1, -2, -2, -1]
djHorse = [2, 1, -1, -2, -2, -1, 1, 2]

visited = [[[False for j in range(width)]
            for i in range(height)] for t in range(limit + 1)]

for i in range(height):
    board.append([])
    inputList = list(map(int, input().split()))
    for j in range(width):
        board[i].append(inputList[j])

queue.append(Qnode(0, 0, 0, limit))

while len(queue) != 0:
    curNode = queue.popleft()

    if(visited[curNode.remain][curNode.posX][curNode.posY] == True):
        continue
    if curNode.posX == height - 1 and curNode.posY == width - 1:
        print(curNode.count)
        isSuccess = True
        break

    visited[curNode.remain][curNode.posX][curNode.posY] = True
    if curNode.remain > 0:
        for t in range(8):
            nextX = curNode.posX + diHorse[t]
            nextY = curNode.posY + djHorse[t]
            if(nextX < 0 or nextX >= height or nextY < 0 or nextY >= width):
                continue
            if(board[nextX][nextY] == 1):
                continue
            queue.append(Qnode(curNode.count + 1, nextX, nextY, curNode.remain - 1))

    for t in range(4):
        nextX = curNode.posX + di[t]
        nextY = curNode.posY + dj[t]
        if(nextX < 0 or nextX >= height or nextY < 0 or nextY >= width):
            continue
        if(board[nextX][nextY] == 1):
            continue
        queue.append(Qnode(curNode.count + 1, nextX, nextY, curNode.remain))


if isSuccess == False:
    print(-1)

```

