# 2109. 순회강연

###### 작성자 : O-star

<br/>

<br/>

### 풀이 : 

**카카오 코테 2차를 위해 파이썬으로 문제를 해결하였다.**

**Union-find 알고리즘을 활용하면 쉽게 풀 수 있는 문제 유형이었다.**

**문제의 풀이과정은 다음과 같다.**

1. **Union-find 알고리즘에 활용한 disjoint set 배열을 생성한다. => 각 인덱스별로 배열 값에 자신의 인덱스 숫자를 가지는 크기 10000개의 리스트 생성**
2. **강연의 페이와 기한의 정보를 저장한 튜플을 담은 리스트를 페이가 높은 강의 순으로 정렬한다.**
3. **Union-find 알고리즘을 활용하여 높은 페이 순의 기한 값을 인덱스로 가지는 roots 배열을 훑어보며 disjoint set 값을 설정한다.**
4. **특정 인덱스 값이 자신의 인덱스를 가지는 인덱스에 도달한 경우에는 해당 인덱스 위치의 기한에는 아직 강연이 잡히지 않은 것이기 때문에 강연을 잡을 수 있다고 판단하고 해당 페이를 누적합해준 후 해당 roots 배열의 인덱스 값이 1을 감소시켜준다.**
5. **인덱스 값이 0에 도달한 경우에는 해당 강연을 잡을 시간이 존재하지 않다는 뜻.**

<br/>

<br/>

#### 코드 : 

```python
n = int(input())
roots = [x for x in range(10001)]
seminars = [tuple(map(int, input().split())) for _ in range(n)]
seminars.sort(reverse=True)


def findRoot(idx):
    if idx == 0:
        return 0
    if roots[idx] == idx:
        roots[idx] -= 1
        return idx

    roots[idx] = findRoot(roots[idx])
    return roots[idx]


income = 0
for seminar in seminars:
    price, duration = seminar
    if findRoot(duration) != 0:
        income += price
print(income)

```

