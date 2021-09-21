# 2589. 보물섬

###### 작성자 : O-star

<br/>

<br/>

### 풀이 : 

**카카오 코테 2차를 위해 파이썬으로 문제를 해결하였다.**

**문제는 매우 간단하다. board 크기 자체가 최대 50*50 이기 때문에 단순히 값이 'L'인 칸에서만 경로를 탐색해주어 가장 긴 거리를 구해주면 되는 BFS 문제이다.**

**하지만 문제를 해결할 때 확실히 파이썬 문법에 익숙치 않아 큐 모듈 사용에서 실수를 하였고 잠시 헤맸던 것 같다.**

**파이썬에서 큐를 사용할 시 일반 리스트를 사용하지 않기. deque를 사용하여 삽입 시 append(), pop의 경우 popleft() 메소드를 사용하기**

**우선순위 큐도 사용해보기. 익숙해지기**

<br/>

<br/>

#### 코드 : 

```python
from collections import deque

height, width = map(int, input().split())
board = [list(input()) for i in range(height)]
di = [0, 1, 0, -1]
dj = [1, 0, -1, 0]


def findPath(startX, startY):
    visited = [[False for i in range(width)] for i in range(height)]
    maxDist = 0
    queue = deque()
    queue.append((startX, startY, 0))
    while len(queue) != 0:
        curX, curY, curDist = queue.popleft()
        maxDist = max(maxDist, curDist)
        if visited[curX][curY] == True:
            continue
        visited[curX][curY] = True
        for k in range(4):
            nextX, nextY = curX + di[k], curY + dj[k]
            if nextX >= 0 and nextX < height and nextY >= 0 and nextY < width and visited[nextX][nextY] == False and board[nextX][nextY] == 'L':
                queue.append((nextX, nextY, curDist + 1))
    return maxDist


answer = 0
for i in range(height):
    for j in range(width):
        if board[i][j] == 'L':
            answer = max(answer, findPath(i, j))

print(answer)
```

