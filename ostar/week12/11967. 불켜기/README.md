# 11967. 불켜기

###### 작성자 : O-star

<br/>

<br/>

### 풀이 : 

**카카오 코테 2차를 위해 파이썬으로 문제를 해결하였다.**

**문제는 이미 방문했던 방, 방문해보아야 하는 방(방문하려 했으나 불이 켜져있지 않은 방 or 처음 방문해봐야 하는 방), 방문할 수 없는 방(현재 이동할 수 있는 방에서 상하좌우로 움직여서 갈 수 없는 방) 으로 잘 나누어야 해결할 수 있다.**

- **불이 켜진 방은 turnOn 배열을 통해 표시해둔다. 방문한 방에서 켤 수 있는 스위치들은 모두 켜서 turnOn 배열에 그 여부를 저장해둔다.**
- **큐에는 방문해보아야 하는 방만 들어있어야 한다. 즉, 방문완료한 방은 pop과정을 진행해주어야 하지만 방문하려했으나 불이 켜져있지 않은 방은 큐에 다시 넣어주는 과정을 거쳐야한다는 의미이다.**
- **이렇게 방을 방문해보아야 하는 방만 지속적으로 체크해주면 최대로 켤 수 있는 스위치 갯수를 도출할 수 있다. 방문해보아야 하는 방을 모두 돌아봐도 더 이상 켤 수 있는 스위치가 없는 경우에는 과정을 끝마친다.**

<br/>

<br/>

#### 코드 : 

```python
from collections import deque

n, m = map(int, input().split())
inserted = [[False for _ in range(n + 1)] for _ in range(n + 1)]
turnOn = [[False for _ in range(n + 1)] for _ in range(n + 1)]
switch = [[[] for _ in range(n + 1)] for _ in range(n + 1)]
di = [0, 1, 0, -1]
dj = [1, 0, -1, 0]
q = deque()
answer = 1

for _ in range(m):
    curX, curY, switchX, switchY = map(int, input().split())
    switch[curX][curY].append((switchX, switchY))

q.append((1, 1))
inserted[1][1] = True
turnOn[1][1] = True
while True:
    isConnect = False
    qSize = len(q)
    for _ in range(qSize):
        curX, curY = q.popleft()
        if turnOn[curX][curY] == False:
            q.append((curX, curY))
            continue

        for needSwitch in switch[curX][curY]:
            if turnOn[needSwitch[0]][needSwitch[1]] == False:
                turnOn[needSwitch[0]][needSwitch[1]] = True
                answer += 1
        for k in range(4):
            nextX, nextY = curX + di[k], curY + dj[k]
            if 0 < nextX and nextX <= n and 0 < nextY and nextY <= n and inserted[nextX][nextY] == False:
                inserted[nextX][nextY] = True
                isConnect = True
                q.append((nextX, nextY))
    if isConnect == False:
        break

print(answer)

```

