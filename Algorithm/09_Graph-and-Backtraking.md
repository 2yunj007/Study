# Graph & Backtraking

## DFS

```py
# 인접 행렬
# 장점 : 구현이 쉬움
# 단점 : 메모리 낭비 (0도 표시를 하기 때문)
graph = [
    [0, 1, 0, 1, 0],
    [1, 0, 1, 1, 1],
    [0, 1, 0, 0, 0],
    [1, 1, 0, 0, 1],
    [0, 1, 0, 1, 0]
]


# DFS - Stack
def dfs_stack(start):
    visited = []
    stack = [start]

    while stack:
        now = stack.pop()
        # 이미 방문한 지점이라면 continue
        if now in visited:
            continue

        # 방문하지 않은 지점이라면 방문 표시
        visited.append(now)

        # 갈 수 있는 곳들을 stack 에 추가
        # 작은 번호부터 조회하고 싶으면 역순으로 순회
        for next in range(5):
            # 연결이 안되어 있다면 continue
            if graph[now][next] == 0:
                continue

            # 방문한 지점이라면 stack 에 추가하지 않음
            if next in visited:
                continue

            stack.append(next)
    # 출력을 위한 반환
    return visited


print("dfs stack = ", end='')
print(*dfs_stack(0))


# DFS - 재귀
# MAP 크기(길이)를 알 때 append 형식 말고 아래와 같이 사용하면 훨씬 빠름
visited = [0] * 5


def dfs(now):
    visited[now] = 1    # 현재 지점 방문 표시
    print(now, end=' ')

    # 인접한 노드들을 방문
    for next in range(5):
        if graph[now][next] == 0:
            continue

        if visited[next]:
            continue

        dfs(next)


print('dfs 재귀 = ', end=' ')
dfs(0)

'''
dfs stack = 0 3 4 1 2
dfs 재귀 =  0 1 2 3 4 
'''
```



## BFS

```py
def bfs(start):
    visited = [0] * 5

    # 먼저 방문했던 것을 먼저 처리해야 함 = queue
    queue = [start]
    visited[start] = 1

    while queue:
        # queue 의 맨 앞 요소를 꺼냄
        now = queue.pop(0)
        print(now, end=' ')

        # 인접한 노드들을 queue 에 추가
        for next in range(5):
            # 연결이 안되어 있다면 continue
            if graph[now][next] == 0:
                continue

            # 방문한 지점이라면 stack 에 추가하지 않음
            if visited[next]:
                continue

            queue.append(next)
            # bfs 이므로 여기서 방문 체크를 해도 상관없음
            visited[next] = 1

bfs(0)  # 0 1 3 2 4 
```



## 서로소 집합(Disjoint-sets)

- 서로소 또는 상호배타 집합들은 서로 중복 포함된 원소가 없는 집합들
  - 다시 말해 교집합이 없음
- **집합에 속한 하나의 특정 멤버를 통해 각 집합들을 구분**하는데, 이를 대표자(representatine)라고 함



**상호배타 집합을 표현하는 방법**

- 연결 리스트 
- 트리



**상호배타 집합 연산**

- Make-Set(X)
- Find-Set(x) : 각 요소가 내가 속한 그룹의 대표자를 어떻게 찾을지
- Union(x, y) : 대표자 저장 (같은 그룹으로 묶기)



### 상호배타 집합 표현

#### 연결 리스트

- 같은 집합의 원소들은 하나의 연결 리스트로 관리
- 연결 리스트의 맨 앞 원소를 집합의 대표 원소로 삼음
- 각 원소는 집합의 대표 원소를 가리키는 링크를 가짐

<img src="https://velog.velcdn.com/images%2Faszxvcb%2Fpost%2F6d82bfaf-e16e-4062-8aa4-c45ed214b298%2Fimage.png" alt="img" style="zoom:67%;" />



#### 트리

- 하나의 집합(a disjoint set)을 하나의 트리로 표현
- 자식 노드가 부모 노드를 가리키며 루트 노드가 대표자가 됨

<img src="https://velog.velcdn.com/images%2Faszxvcb%2Fpost%2Ff0294819-c3d0-4d9d-b5f1-0bc2ef1d6696%2Fimage.png" alt="img" style="zoom:67%;" />



#### 구현

```py
# 0 ~ 9
# make set - 집합을 만들어 주는 과정
# 각 요소가 가리키는 값이 부모
parent = [i for i in range(10)]

# find-set (대표자 검색)
def find_set(x):
    if parent[x] == x:
        return x

    # return find_set(parent[x])

    # 경로 압축
    parent[x] = find_set(parent[x])
    return parent[x]

# union
def union(x, y):
    # 1. 이미 같은 집합인 지 체크
    x = find_set(x)
    y = find_set(y)

    # 대표자가 같으니, 같은 집합이다.
    if x == y:
        print("싸이클 발생")
        return

    # 2. 다른 집합이라면, 같은 대표자로 수정
    if x < y:
        parent[y] = x
    else:
        parent[x] = y

union(0, 1)

union(2, 3)

union(1, 3)

# 이미 같은 집합에 속해 있는 원소를 한 번 더 union
# 싸이클이 발생
union(0, 2)

# 대표자 검색
print(find_set(2))
print(find_set(3))

# 같은 그룹인 지 판별
t_x = 0
t_y = 2

if find_set(t_x) == find_set(t_y):
    print(f"{t_x} 와 {t_y} 는 같은 집합에 속해 있습니다.")
else:
    print(f"{t_x} 와 {t_y} 는 다른 집합에 속해 있습니다.")

'''
싸이클 발생
0
0
0 와 2 는 같은 집합에 속해 있습니다.
'''
```

```py
# 트리의 rank 도 따로 저장하여 효율적으로 알고리즘을 구현한 방법
class UnionFind:
    def __init__(self, n):
        self.parent = [i for i in range(n)]  # 각 원소의 부모를 자신으로 초기화
        self.rank = [0] * n  # 트리의 깊이(랭크)를 저장

    def find_set(self, x):
        if self.parent[x] == x:
            return x

        return self.find_set(self.parent[x])

        # 경로 압축 (path compression)을 통해 부모를 루트로 설정
        # self.parent[x] = self.find_set(self.parent[x])
        # return self.parent[x]

    def union(self, x, y):
        root_x = self.find_set(x)
        root_y = self.find_set(y)

        if root_x != root_y:
            # 랭크를 비교하여 더 높은 트리의 루트를 부모로 설정
            if self.rank[root_x] < self.rank[root_y]:
                self.parent[root_x] = root_y
            elif self.rank[root_x] > self.rank[root_y]:
                self.parent[root_y] = root_x
            else:
                self.parent[root_y] = root_x
                self.rank[root_x] += 1

# 예제 사용법
n = 5  # 원소의 개수
uf = UnionFind(n)

# 원소 0과 원소 1을 합침
uf.union(0, 1)

# 원소 2와 원소 3을 합침
uf.union(2, 3)

# uf.union(3, 4)

target_x = 2
target_y = 4

# 원소 1과 원소 2가 같은 집합에 속해 있는지 확인
if uf.find_set(target_x) == uf.find_set(target_y):
    print(f"원소 {target_x}과 원소 {target_y}는 같은 집합에 속해 있습니다.")
else:
    print(f"원소 {target_x}과 원소 {target_y}는 다른 집합에 속해 있습니다.")

'''
원소 2과 원소 4는 다른 집합에 속해 있습니다.
'''
```

