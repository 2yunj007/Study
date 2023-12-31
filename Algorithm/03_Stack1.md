# 스택 (Stack)

## 스택의 특성

- 물건을 쌓아 올리듯 자료를 쌓아 올린 형태의 자료구조
- **후입선출(LIFO, Last-In-First-Out)**: 마지막에 삽입한 자료를 가장 먼저 꺼냄
- 스택에 저장된 자료는 **선형 구조**를 가짐
  - 선형 구조: 자료 간의 관계가 1대1의 관계를 가짐
  - 비선형 구조: 자료 간의 관계가 1대N 관계를 가짐 (ex. 트리)
- 스택에 자료를 삽입하거나 스택에서 자료를 꺼낼 수 있음
- 스택에서 마지막 삽입된 원소의 위치를 top이라고 함

<img src="https://blog.kakaocdn.net/dn/bcgR9A/btqSX70PCTe/dMSMQoJcZhDpq4sRRpu3A0/img.png" alt="img" style="zoom: 33%;" />



## 스택의 연산 과정

- 원소 삽입 : **push()**

```python
def push(item, size):
    global top
    top += 1
    if top==size:
        print('overflow!')
    else:
        stack[top] = item

size = 10
stack = [0] * size
top = -1

push(10, size)
top += 1	# push(20)
stack[top] = 20
```



- 원소 반환/삭제 : **pop()**

```python
def pop():
    global top
    if top == -1:
        print('underflow')
        return 0
    else:
        top -= 1
        return stack[top+1]

print(pop())

if top > -1:	# pop()
    top -= 1
    print(stack[top+1])
```



- 공백 상태 검사 : **isEmpty(**)

- 원소 반환 : **peek()**



## 고려 사항

- 1차원 배열을 사용하여 구현할 경우 구현이 용이하다는 장점이 있지만 스택의 크기를 변경하기 어렵다는 점이 있음
- 저장소를 동적으로 할당하여 스택을 구현하는 방법으로 해결 가능
  - 동적 연결 리스트를 이용하여 구현
  - 구현이 복잡하지만 메모리 효율성 향상 가능



## 스택의 응용 1: 괄호 검사

### 조건

1. 왼쪽 괄호의 개수와 오른쪽 괄호의 개수가 같아야 함
2. 같은 괄호에서 왼쪽 괄호는 오른쪽 괄호보다 먼저 나와야 함
3. 괄호 사이에는 포함 관계만 존재함



### 알고리즘 개요

1. 문자열에 있는 괄호를 차례대로 조사하면서 왼쪽 괄호를 만나면 스택에 삽입
2. 오른쪽 괄호를 만나면 스택에서 top 괄호를 삭제한 후 오른쪽 괄호와 짝이 맞는지 검사
3. 이때, 스택이 비어 있으면 조건 1 또는 조건 2에 위배되고 괄호의 짝이 맞지 않으면 3에 위배됨
4. 마지막 괄호까지 조사한 후에도 스택에 괄호가 남아 있으면 조건 1에 위배



## 스택의 응용 2: Function call

- 프로그램의 함수 호출과 복귀에 따른 수행 순서를 관리

- 가장 마지막에 호출된 함수가 가장 먼저 실행을 완료하고 복귀하는 후입선출 구조이므로, 후입선출 구조의 스택을 이용하여 수행 순서 관리
- 함수 호출이 발생하면 호출한 함수 수행에 필요한 지역변수, 매개변수 및 수행 후 복귀할 주소 등의 정보를 스택 프레임(stack frame)에 저장하여 시스템 스택에 삽입
- 함수의 실행이 끝나면 시스템 스택의 top 원소(stack frame)를 삭제(pop)하면서 프레임에 저장되어 있던 복귀 주소를 확인하고 복귀
- 함수 호출과 복귀에 따라 이 과정을 반복하여 전체 프로그램 수행이 종료되면 시스템 스택은 공백 스택이 됨

<img src="https://blog.kakaocdn.net/dn/ckZG8J/btqYgGK4Uug/4FoX6v2805UY4BUvkpmMi1/img.png" alt="img" style="zoom: 80%;" />



# 재귀 호출

- 자기 자신을 호출하여 순환 수행되는 것
- 함수에서 실행해야 하는 작업의 특성에 따라 일반적인 호출 방식보다 재귀 호출 방식을 사용하여 함수를 만들면 프로그램의 크기를 줄이고 간단하게 작성

```python
# 예시
def f(i, N):
    if i == N:
        return
    else:
        B[i] = A[i]
        f(i+1, N)
        return  # 형식상 쓰는 게 맞는데 편의상 안 써도 됨

N = 3
A = [1, 2, 3]
B = [0] * N
f(0, N)
print(B)
```



## 피보나치 수

- 0과 1로 시작하고 이전의 두 수의 합을 다음 항으로 하는 수열을 피보나치라 함
  - 0, 1, 1, 2, 3, 5, 8, 13, ...
- 피보나치 수열의 i 번째 값을 계산하는 함수 F를 정의하면 다음과 같음
  - F_0 = 0, F_1 = 1
  - F_i = F_i-1 + F_i-2 for i >= 2

- 위의 정의로부터 피보나치 수열의 i 번째 항을 반환하는 함수를 재귀 함수로 구현 가능

```python
def fibo(n):
    if n < 2:
        return n
    else:
        return fibo(n-1) + fibo(n-2)
```

- 하지만 다음과 같은 엄청난 중복 호출이 존재한다는 문제 발생

![img](https://velog.velcdn.com/images/skarb4788/post/315c7b88-0671-4f75-a0c4-8bf821b5134e/image.png)



# 메모이제이션 (Memoization)

- 컴퓨터 프로그램을 실행할 때 이전에 계산한 값을 메모리에 저장해서 매번 다시 계산하지 않도록 하여 전체적인 실행 속도를 빠르게 하는 기술
- 재귀를 이용해 피보나치 수를 구하는 알고리즘에서 fibo(n)의 값을 계산하자마자 저장하면 실행시간을 Ω(n)으로 줄일 수 있음

```python
# memo를 위한 배열을 할당하고, 모두 0으로 초기화
# memo[0]을 0으로 memo[1]는 1로 초기화

def fibo1(n):
    global memo
    if n >= 2 and memo[n] == 0:
        memo[n] = fibo1(n-1) + fibo1(n-2)
    return memo[n]

memo = [0] * (n+1)
memo[0] = 0
memo[1] = 1
```



# 동적 계획법 (Dynamic Programming, DP)

- 그리디 알고리즘과 같이 최적화 문제를 해결하는 알고리즘
- 먼저 입력 크기가 작은 부분 문제들을 모두 해결한 후에 그 해들을 이용하여 보다 큰 크기의 부분 문제들을 해결하여, 최종적으로 원래 주어진 입력의 문제를 해결하는 알고리즘



## 피보나치 수 DP 적용

- 부분 문제로 나누는 일을 끝냈으면 가장 작은 부분 문제부터 해를 구함
- 그 결과를 테이블에 저장하고, 테이블에 저장된 부분 문제의 해를 이용하여 상위 문제의 해를 구함
  - 테이블 인덱스: [0], [1], [2], ... [n]
  - 저장되어 있는 값: 0, 1, 1, ... fibo(n)

```python
def fibo2(n):
    dp = [0]*(n+1)
    dp[0] = 0
    dp[1] = 1
    for i in range(2, n+1):
        dp[i] = dp[i-1] + dp[i-2]
    return dp[n]
```



## DP의 구현 방식

- recursive 방식 / iterative 방식

- memoization을 재귀적 구조에 사용하는 것보다 반복적 구조로 DP를 구현한 것이 성능 면에서 보다 효율적
- 재귀적 구조는 내부에 시스템 호출 스택을 사용하는 오버헤드가 발생하기 때문



# 깊이 우선 탐색 (Depth First Search, DFS)

- 모든 경로를 방문해야 할 경우에 사용에 적합
- 시작 정점의 한 방향으로 갈 수 있는 경로가 있는 곳까지 깊이 탐색해 가다가 더 이상 갈 곳이 없게 되면, 가장 마지막에 만났던 갈림길 간선이 있는 정점으로 되돌아와서 다른 방향의 정점으로 탐색을 계속 반복하여 결국 모든 정점을 방문하는 순회 방법
- 가장 마지막에 만났던 갈림길의 정점으로 되돌아가서 다시 깊이 우선 탐색을 반복해야 하므로 후입선출 구조의 스택 사용

<img src="https://blog.kakaocdn.net/dn/bEpXQk/btrdSd6mdIY/5jnQjr8l5uEZPtrNGPOjfK/img.gif" alt="img" style="zoom:67%;" />



## DFS 알고리즘

1. 시작 정점 v를 결정하여 방문
2. 정점 v에 인접한 정점 중에서 
   1. 방문하지 않은 정점 w가 있으면, 정점 v를 스택에 push하고 정점 w를 방문, w를 v로 하여 반복
   2. 방문하지 않은 정점이 없으면, 탐색의 방향을 바꾸기 위해서 스택을 pop하여 받은 가장 마지막 방문 정점을 v로 하여 반복
3. 스택이 공백이 될 때까지 반복

```python
# visited[], stack[] 초기화
DFS(v)
	visited <- true;
    while {
        if (v의 인접 정점 중 방문 안 한 정점 w가 있으면)
        	push(v);
        	v <- w; (w에 방문)
        	visited[w] <- true;
        else
        	if (스택이 비어 있지 않으면)
        		v <- pop(stack);
        	else
        		break
    }
end DFS()
```

```python
# 스택에서 빼면서 방문
def dfs(V, S):
    visited = [0]*(V+1)
    s = [S]
    cnt = 0
    while s:
        t = s.pop()
        if visited[t] == 0:
            cnt += 1
            visited[t] = cnt
            for w in adj_l[t][::-1]:	# 오름차순 방문
                if visited[w] == 0:
                    s.append(w)
```



### 연습 문제

```python
'''
V E
v1 w1 v2 w2 ...
7 8
1 2 1 3 2 4 2 5 4 6 5 6 6 7 3 7
'''
def dfs(n, V, adj_m):
    stack = []      		# stack 생성
    visited = [0] * (V+1)	# visited 생성
    visited[n] = 1  		# 시작점 방문 표시
    print(n)        		# do[n] (n: 정점)
    
    while True:
        for w in range(1, V+1):
            # 현재 정점 n에 인접하고 미방문 w 찾기
            if adj_m[n][w] == 1 and visited[w] == 0:
                stack.append(n) # push(w), v = w
                n = w
                print(n)    # do(w)
                visited[n] = 1  # w 방문 표시
                break   # n에 인접인 w 찾은 경우 for w 끝
        else:
            if stack:   # 스택에 지나온 정점이 남아 있으면
                n = stack.pop() # pop()해서 다시 다른 w 찾기
            else:   # 스택이 비어 있으면
                break   # while True 끝


V, E = map(int, input().split())    # 1번부터 V번 정점, E개의 간선
arr = list(map(int, input().split()))
adj_m = [[0]*(V+1) for _ in range(V+1)]
for i in range(E):
    v1, v2 = arr[i*2], arr[i*2+1]
    adj_m[v1][v2] = 1
    adj_m[v2][v1] = 1

dfs(1, V, adj_m)

# adj_m:
# [[0, 0, 0, 0, 0, 0, 0, 0],
#  [0, 0, 1, 1, 0, 0, 0, 0],
#  [0, 1, 0, 0, 1, 1, 0, 0],
#  [0, 1, 0, 0, 0, 0, 0, 1],
#  [0, 0, 1, 0, 0, 0, 1, 0],
#  [0, 0, 1, 0, 0, 0, 1, 0],
#  [0, 0, 0, 0, 1, 1, 0, 1],
#  [0, 0, 0, 1, 0, 0, 1, 0]]
```



# 그래프

- 아이템들간의 연결 관계
- 정점들의 집합과 이들을 연결해주는 간선들의 집합으로 구성됨

- 선형 자료 구조에서 나타내기 힘든 N;M 관계 표현이 가능
- 인접 행렬 : 특정 노드에서 연결된 노드의 정보



## stack을 활용하는 이유

- "내가 돌아갈 곳을 저장해 놓는 것" ==  Stack 활용 (후입선출)

- 그래프를 표현하는 방법 (인접 행렬)

1. 딕셔너리의 활용

   ```python
   graph = {}
   graph['A'] = ['B','C']
   graph['B'] = ['D', 'E']
   ```

2. 2차원 배열의 활용

   ```
   #   A B C D E F
   # A 0 1 1 0 0 0 
   # B 0 0 0 1 1 0
   # C
   # D
   # E
   # F
   ```



```python
# 인접 행렬 생성
V, E = map(int, inpt().split()) # 노드, 간선
data = list(map(int, input().split())) # 간선 정보

arr = [[0] * (v+1) for _ in range(V+1)]
visited = [0] * (V + 1) # 노드의 방문 여부 체크 리스트
#visited = [False] * (V + 1)

for i in range(E):
    n1, n2 = data[i * 2], data[i * 2 + 1]
    arr[n1][n2] = 1 # n1과 n2는 인접해 있음
    arr[n2][n1] = 1
    
# 재귀
def dfs(v):
    visited[v] = 1
    print(v, end = ' ') # 특정 로직을 수행하는 곳
    # v 현재 시작 정점, 인접한 정점 중에서 방문을 하지 않은 곳
    for w in range(1, V+1):
        if arr[v][w] == 1 and visited[w] == 0:
        	dfs(w)
            
# 반복문
def dfs(v):
    stack = [v]
    # 스택이 빌 때까지 반복
    while len(stack):
        v = stack.pop()
        visited[v] = 1
        for w in range(1, V+1):
            id arr[v][w] == 1 and vistied[w] == 0:
                stack.append(w)
                
dfs(1)
```

