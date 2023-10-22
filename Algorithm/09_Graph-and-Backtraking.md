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



## 최소 신장 트리(MST)

> 무방향 가중치 그래프에서 신장 트리를 구성하는 간선들의 가중치의 합이 최소인 신장 트리

- **그래프에서 최소 비용 문제**

  - 모든 정점을 연결하는 간선들의 가중치의 합이 최소가 되는 트리 -> 최소 신장 트리

  - 두 정점 사이의 최소 비용의 경로 찾기 -> 다익스트라

- **신장 트리**
  -  n개의 정점으로 이루어진 무방향 그래프에서 n개의 정점과 n-1개의 간선으로 이루어진 트리
  - 모든 정점이 연결되어 있음
  - 사이클이 존재하지 않는 부분 그래프
  - 한 그래프에서 여러 개의 신장 트리가 나올 수 있음



### Prim 알고리즘

- 하나의 정점에서 연결된 간선들 중에 하나씩 선택하면서 MST를 만들어 가는 방식
  - 임의 정점 하나 선택해서 시작
  - 선택한 정점과 인접하는 정점들 중의 최소 비용의 간선이 존재하는 정점을 선택
  - 모든 정점이 선택될 때까지 앞의 과정을 반복
- 서로소 2개의 집합(2 disjoint - sets) 정보를 유지
  - 트리 정점들(tree vertices) - MST를 만들기 위해 선택된 정점들
  - 비트리 정점들(nontree vertices) - 선택되지 않은 정점들

```py
'''
7 11
0 1 32
0 2 31
0 5 60
0 6 51
1 2 21
2 4 46
2 6 25
3 4 34
3 5 18
4 5 40
4 6 51
'''

import heapq


def prim(start):
    heap = []
    # MST 에 포함되었는지 여부
    MST = [0] * V

    # 가중치, 정점 정보
    heapq.heappush(heap, (0, start))
    # 누적합 저장
    sum_weight = 0

    while heap:
        # 가장 적은 가중치를 가진 정점을 꺼냄
        weight, v = heapq.heappop(heap)

        # 이미 방문한 노드라면 pass
        if MST[v]:
            continue

        # 방문 체크
        MST[v] = 1
        
        # 누적합 추가
        sum_weight += weight

        # 갈 수 있는 노드들을 체크
        for next in range(V):
            # 갈 수 없거나 이미 방문했다면 pass
            if graph[v][next] == 0 or MST[next]:
                continue

            heapq.heappush(heap, (graph[v][next], next))

    return sum_weight


V, E = map(int, input().split())
# 인접행렬
graph = [[0] * V for _ in range(V)]

for _ in range(E):
    f, t, w = map(int, input().split())
    graph[f][t] = w
    graph[t][f] = w     # 무방향 그래프

result = prim(0)
print(f'최소 비용 = {result}')
```



### KRUSKAL 알고리즘

- 간선을 하나씩 선택해서 MST를 찾는 알고리즘
- 최초, 모든 간선을 가중치에 따라 오름차순으로 정렬
- 가중치가 가장 낮은 간선부터 선택하면서 트리를 증가시킴
  - 사이클이 존재하면 다음으로 가준치가 낮은 간선 선택
- n-1개의 간선이 선택될 때까지 앞의 과정을 반복

```py
'''
7 11
0 1 32
0 2 31
0 5 60
0 6 51
1 2 21
2 4 46
2 6 25
3 4 34
3 5 18
4 5 40
4 6 51
'''

# 모든 간선들 중 비용이 가장 적은 걸 우선으로 고르자
V, E = map(int, input().split())
edge = []
for _ in range(E):
    f, t, w = map(int, input().split())
    edge.append([f, t, w])
# w 를 기준으로 정렬
edge.sort(key=lambda x: x[2])

# 사이클 발생 여부를 union find 로 해결
parents = [i for i in range(V)]

def find_set(x):
    if parents[x] == x:
        return x

    parents[x] = find_set(parents[x])
    return parents[x]


def union(x, y):
    x = find_set(x)
    y = find_set(y)

    if x == y:
        return

    if x < y:
        parents[y] = x
    else:
        parents[x] = y

# 현재 방문한 정점 수
cnt = 0
sum_weight = 0
for f, t, w in edge:
    # 싸이클이 발생하지 않는다면
    if find_set(f) != find_set(t):
        cnt += 1
        sum_weight += w
        union(f, t)
        # MST 구성이 끝나면
        if cnt == V:
            break
print(f'최소 비용 = {sum_weight}')
```



## 최단 경로

> 간선의 가중치가 있는 그래프에서 두 정점 사이의 경로들 중에 간선의 가중치의 합이 최소인 경로

- 하나의 시작 정점에서 끝 정점까지의 최단 경로
  - **다익스트라(dijkstra) 알고리즘**
    - 음의 가중치를 허용하지 않음
  - **벨만-포드(Bellman-Ford) 알고리즘**
    - 음의 가중치 허용
- 모든 정점들에 대한 최단 경로
  - **플로이드-워샬(Floyd-Warshall) 알고리즘**



### Dijkstra 알고리즘

> 시작 정점에서 거리(누적 거리)가 최소인 정점을 선택해 나가면서 최단 경로를 구하는 방식

- 시작 정점(s)에서 끝 정점(t)까지의 최단 경로에 정점 x가 존재
- 이때, 최단 경로는 s에서 x까지의 최단 경로와 x에서 t까지의 최단 경로로 구성됨
- 탐욕 기법을 사용한 알고리즘으로 MST의 프림 알고리즘과 유사

![img](https://upload.wikimedia.org/wikipedia/commons/5/57/Dijkstra_Animation.gif)

```python
'''
6 8
0 1 2
0 2 4
1 2 1
1 3 7
2 4 3
3 4 2
3 5 1
4 5 5
'''

# 최단 거리 문제 유형
# 1. 특정 지점 -> 도착 지점까지의 최단 거리 : 다익스트라
# 2. 가중치에 음수가 포함되어 있네 ? : 밸만포드
# 3. 여러 지점 -> 여러 지점까지의 최단 거리
#       - 여러 지점 모두 다익스트라 돌리기 -> 시간 복잡도 계산 잘해야함
#       - 플로이드-워샬


# 내가 갈 수 있는 경로 중 누적거리가 제일 짧은 것부터 고르자!
import heapq

# 입력
n, m = map(int, input().split())
# 인접리스트
graph = [[] for i in range(n)]
for _ in range(m):
    f, t, w = map(int, input().split())
    graph[f].append([t, w])

# 1. 누적 거리를 계속 저장
INF = int(1e9) # 최대값으로 1억 - 대충 엄청 큰 수
distance = [INF] * n

def dijkstra(start):
    # 2. 우선순위 큐
    pq = []
    # 출발점 초기화
    heapq.heappush(pq, (0, start))
    distance[start] = 0

    while pq:
        # 현재 시점에서
        # 누적 거리가 가장 짧은 노드에 대한 정보 꺼내기
        dist, now = heapq.heappop(pq)

        # 이미 방문한 지점 + 누적 거리가 더 짧게 방문한 적이 있다면 pass
        if distance[now] < dist:
            continue

        # 인접 노드를 확인
        for next in graph[now]:
            next_node = next[0]
            cost = next[1]

            # next_node 로 가기 위한 누적 거리
            new_cost = dist + cost

            # 누적 거리가 기존보다 크네?
            if distance[next_node] <= new_cost:
                continue

            distance[next_node] = new_cost
            heapq.heappush(pq, (new_cost, next_node))

dijkstra(0)
print(distance)
```

```py
import sys
sys.stdin = open('input.txt', 'r')


import heapq

def dijkstra(graph, start):
    # 시작 노드에서 다른 노드까지의 최단 거리를 저장하는 딕셔너리
    distances = {node: float('infinity') for node in graph}
    distances[start] = 0  # 시작 노드의 거리는 0

    # 우선순위 큐 (거리, 노드)를 사용하여 노드를 방문 순서대로 관리
    priority_queue = [(0, start)]

    while priority_queue:
        # 현재 노드와 현재까지의 최단 거리를 가져옴
        # heapq.heappop()은 우선순위 큐에서 가장 작은 값을 가져옴
        current_distance, current_node = heapq.heappop(priority_queue)

        # 현재 노드가 이미 처리된 노드라면 무시
        # 이를 위해 distances 딕셔너리에는 각 노드에 대한 최단 거리를 저장
        # 현재 노드를 방문하기 전에 이미 해당 노드에 대해 더 짧은 거리로 방문한 경우에는 무시
        if current_distance > distances[current_node]:
            continue

        # 현재 노드와 연결된 노드들을 순회
        for neighbor, weight in graph[current_node].items():
            distance = current_distance + weight

            # 만약 이 경로를 통해 더 짧은 거리를 찾았다면 업데이트
            if distance < distances[neighbor]:
                distances[neighbor] = distance
                heapq.heappush(priority_queue, (distance, neighbor))

    return distances

# 그래프를 딕셔너리로 표현
graph = {
    'A': {'B': 1, 'C': 4},
    'B': {'A': 1, 'C': 2, 'D': 5},
    'C': {'A': 4, 'B': 2, 'D': 1},
    'D': {'B': 5, 'C': 1}
}

start_node = 'A'
result = dijkstra(graph, start_node)
print("시작 노드로부터의 최단 거리:")
for node, distance in result.items():
    print(f"{node}: {distance}")
```

```py
import sys


# 우선순위 큐를 사용하지 않는 코드

def prim(graph):
    V = len(graph)  # 그래프의 노드 수
    visited = [False] * V  # 노드 방문 여부를 저장하는 리스트

    # 시작 노드를 0번 노드로 선택
    start_node = 2
    visited[start_node] = True

    # 시작 노드와 연결된 간선을 저장할 리스트
    edges = []

    # 시작 노드와 연결된 간선을 edges 리스트에 추가
    for neighbor, weight in graph[start_node]:
        edges.append((start_node, neighbor, weight))

    # 최소 신장 트리를 저장하는 리스트
    mst = []

    while len(mst) < V - 1:  # 모든 노드를 방문할 때까지 반복
        # 리스트에서 최소 가중치 간선을 찾음
        min_edge = min(edges, key=lambda x: x[2])

        u, v, weight = min_edge

        # 이미 방문한 노드인 경우 무시
        if visited[v]:
            edges.remove(min_edge)  # 이미 방문한 노드와 연결된 간선을 edges 리스트에서 제거
            continue

        # 간선 (u, v)를 최소 신장 트리에 추가
        mst.append(min_edge)
        visited[v] = True  # 노드 v를 방문했음을 표시

        # 새로 추가된 노드 v와 연결된 간선을 edges 리스트에 추가
        for neighbor, weight in graph[v]:
            if not visited[neighbor]:  # 방문하지 않은 노드만을 고려
                edges.append((v, neighbor, weight))

        # 현재 간선을 edges 리스트에서 제거
        edges.remove(min_edge)  # 현재 처리한 간선은 다음 루프에서 중복으로 처리하지 않도록 제거

    return mst


# 그래프 정보 입력
V, E = map(int, input().split())
graph = [[] for _ in range(V)]

for _ in range(E):
    u, v, w = map(int, input().split())
    # 무방향 그래프이므로 u에서 v로 가는 간선과 v에서 u로 가는 간선을 추가
    graph[u - 1].append((v - 1, w))
    graph[v - 1].append((u - 1, w))

# MST 계산
minimum_spanning_tree = prim(graph)

# 결과 출력
total_weight = sum(weight for _, _, weight in minimum_spanning_tree)
print("Minimum Spanning Tree:")
for u, v, weight in minimum_spanning_tree:
    print(f"Edge ({u + 1}-{v + 1}), Weight: {weight}")
print(f"Total Weight of MST: {total_weight}")
```

