## 계산기

### 문자열 수식 계산의 일반적 방법

step1. 중위 표기법의 수식을 후위 표기법으로 변경 (스택 이용)

step2. 후위 표기법의 수식을 스택을 이용하여 계산

**중위 표기법 (infix nitation)**: 연산자를 피연산자의 가운데 표기하는 방법 (ex. A+B)

**후위 표기법 (postfic notation)**: 연산자를 피연산자 뒤에 표기하는 방법 (ex. AB+)



**step1. 중위 표기식의 후위 표기식 변환 방법 1**

1. 수식의 각 연산자에 대해서 우선순위에 따라 괄호를 사용하여 다시 표현

2. 각 연산자를 그에 대응하는 오른쪽 괄호의 뒤로 이동시킴

3. 괄호 제거



**step1. 중위 표기식의 후위 표기식 변환 방법 2 (스택)**

1. 입력받은 중위 표기식에서 토큰을 읽음

2. 토큰이 피연산자이면 토큰을 출력

3. 토큰이 연산자(괄호 포함)일 때,

   - 이 토큰이 스택의 top에 저장되어 있는 연산자보다 우선 순위가 높으면 스택에 push

   - 그렇지 않다면 스택 top의 연산자의 우선순위가 토큰의 우선 순위보다 작을 때까지 스택에서 pop한 후 토큰의 연산자를 push

   - 만약 top에 연산자가 없으면 push

4. 토큰이 오른쪽 괄호 ')'이면 스택 top에 왼쪽 괄호 '('가 올 때까지 스택에 pop 연산을 수행하고 pop한 연산자를 출력

5. 왼쪽 괄호를 만나면 pop만 하고 출력하지는 않음

6. 중위 표기식에 더 읽을 것이 없다면 중지하고, 더 읽을 것이 있다면 1부터 다시 반복

7. 스택에 남아 있는 연산자를 모두 pop하여 출력

   - 스택 밖의 왼쪽 괄호는 우선 순위가 가장 높으며, 스택 안의 왼쪽 괄호는 우선 순위가 가장 낮음



**step2. 후위 표기법의 수식을 스택을 이용하여 계산**

1. 피연산자를 만나면 스택에 push
2. 연산자를 만나면 필요한 만큼의 피연산자를 스택에서 pop하여 연산하고, 연산 결과를 다시 스택에 push
3. 수식이 끝나면, 마지막으로 스택을 pop하여 출력



```python
# (6+5*(2-8)/2)
# 6528-*2/+

stack = [0] * 100 
top = -1
icp = {'(':3, '*':2, '/':2, '+':1. '-':1}
isp = {'(':0, '*':2, '/':2, '+':1. '-':1}

fx = (6+5*(2-8)/2)
susik = ''
for x in fx:
    if x not in '(+-*/)': # 피연산자
        susik += x
    elif x == ')': # '('까지 pop()
        while stack[top] != '(': # peek
            susik += stack[top]
            top -= 1
        top -= 1 # '('버림. pop
    else: # '(+-*/'
        if top == -1 or isp[stack[top]] <icp[x]: # stack이 비어있거나 
                                                 # 들어오는 애의 우선순위가 더 높으면
            top += 1 # push
            stack[top] = x
        elif isp[stack[top]] >= icp[x]:
            while top > -1 and isp[stack[top]] >= icp[x]:
                susik += stack[top]
                top -= 1
            top +=1 # push
            satack[top] = x
print(susik)           
    

susik = '6528-*2/+'
for tk in susik:
    if x not in '+-/*': # 피연산자면
        top += 1 # push(x)
        stack[top] = int(x)
    else:
        op1 = stack[top] # pop()
        top -= 1
        op2 = stack[top] # pop
        top -= 1
        if x == '+': # op2 + op1(먼저 꺼낸 애가 오른쪽에)
            top += 1
            stack[top] = op2 + op1
        elif x == '-':
            top += 1
            stack[top] = op2 - op1
        elif x == '*':
            top += 1
            stack[top] = op2 * op1
        else:
            top += 1
            stack[top] = op2 / op1
print(stak[top])
```



# 백트래킹 (Backtracking)

- 백트래킹 기법은 해를 찾는 도중에 막히면 (즉, 해가 아니면) 되돌아가서 다시 해를 찾는 기법

- 백트래킹 기법은 최적화(optimozation) 문제와 결정(decision) 문제를 해결할 수 ㅇㅆ음

- 결정 문제: 문제의 조건을 만족하는 해의 존재 여부를 yes/no로 답하는 문제

  (ex. 미로 찾기, n-Queen 문제, Map coloring, 부분 집합의 합 문제 등)



### 백트래킹과 깊이 우선 탐색의 차이

- 어떤 노드에서 출발하는 경로가 해결책으로 이어질 것 같지 않으면 더 이상 그 경로를 따라가지 않음으로써 시도의 횟수를 줄임 (Prunning 가지치기)
- 깊이 우선 탐색이 모든 경로를 추적하는데 비해 백트래킹은 불필요한 경로를 조기 차단
- 깊이 우선 탐색을 가하기에는 경우의 수가 너무 많음
- 즉, N! 가지의 경우의 수를 가진 문제에 대해 깊이 우선 탐색을 가하면 당연히 처리 불가능한 문제
- 백트래킹 알고리즘을 적용하면 일반적으로 경우의 수가 줄어들지만 이 역시 최악의 경우에는 여전히 ecponential time을 요하므로 처리 불가능



### 백트래킹 기법

- 어떤 노드의 유망성을 점검한 후에 유망하지 않다고 결정되면 그 노드의 부모로 되돌아가 다음 자식 노드로 감
- 어떤 노드를 방문하였을 때 그 노드를 포함한 경로가 해답이 될 수 없으면 그 노드는 유망하지 않다고 하며, 반대로 해답의 가능성이 있으면 유망하다고 함
- 가지치기: 유망하지 않은 노드가 포함되는 경로는 더 이상 고려하지 않음



## 미로 찾기

- 아래 그림처럼 입구와 출구가 주어진 미로에서 입구부터 출구까지의 경로를 찾는 문제
- 이동할 수 있는 방향은 4방향으로 제한

<img src="https://blog.kakaocdn.net/dn/Rvi0C/btqYxpUDCDQ/FkdKOZjtvpSP7mYj9OzgB1/img.png" alt="img" style="zoom:67%;" />

### 미로 찾기 알고리즘

- 스택을 이용하여 지나온 경로를 역으로 되돌아 감

![img](https://blog.kakaocdn.net/dn/mpFlY/btqYusq7ypb/a0FhMc1vODMLeZhqb79PuK/img.png)

## n-Queen

- N*N 인 체스판 위에 퀸 N 개가 서로를 공격 할 수 없게 놓는 경우의 수를 구하는 문제

```python
# 일반 백트래킹 알고리즘
def checknode(v): # node
if promising(v)L
	if there is a solution at v:
        write the solution
    else:
        for u in each child of v:
            checknode(u)
```

<img src="https://blog.kakaocdn.net/dn/cyJ4YP/btqYtPNw0dL/e3r4XLoFIHGyDWFY7Y4eck/img.png" alt="img" style="zoom: 80%;" />


## 부분집합 구하기

- n개의 원소가 들어 있는 집합의 2^n개의 부분집합을 만들 때는, true 또는 false 값을 가지는 항목들로 구성된 n개의 배열을 만드는 방법을 이용
- 여기서 배열의 i번째 항목은 i번째의 원소가 부분집합의 값인지 아닌지를 나타내는 값

<img src="https://blog.kakaocdn.net/dn/IpbU4/btqYurFGoka/bESaPKqvddqr61kXIE2UL1/img.png" alt="img" style="zoom:67%;" />



### powerset을 구하는 백트래킹 알고리즘

```python
N = 3
arr = [1, 2, 3] # 우리가 활용할 데이터
sel = [0] * N # a리스트 (내가 해당 원소를 뽑았는지 체크)

def powerset(idx):
    if idx == N:
        print(sel, ":", end = ' ')
        for i in range(N):
            if sel[i]:
                print(arr[i], end=' ')
        print()
        
        return
    
    # idx 자리의 원소를 뽑고 감
    sel[idx] = 1
    powerset(idx+1)
    # idx 자리를 안 뽑고 감
    sel[idx] = 0
    powerset(idx + 1)


powerset(0)
# =>
# [1, 1, 1] : 1 2 3 
# [1, 1, 0] : 1 2 
# [1, 0, 1] : 1 3 
# [1, 0, 0] : 1 
# [0, 1, 1] : 2 3 
# [0, 1, 0] : 2 
# [0, 0, 1] : 3 
# [0, 0, 0] : 
```



### 순열 구하기

```python
arr = [1, 2, 3]
n = 3
sel = [0] * n
check = [0] * n

# 재귀방식
def perm(idx):
    
    # 다 뽑아서 정리했다면
    if idx == n:
        print(sel)
    
    else:
        for i in range(n):
            if check[i] == 0:
                sel[idx] = arr[i]   # 값을 사용해라
                check[i] = 1        # 사용을 했다는 표시
                perm(idx+1)
                check[i] = 0        # 다음 반복문을 위한 원상복구

perm(0)
# =>
# [1, 2, 3]
# [1, 3, 2]
# [2, 1, 3]
# [2, 3, 1]
# [3, 1, 2]
# [3, 2, 1]
```

```python
# 비트연산 방식
# check 10진수 정수
def perm(idx, check):
    if idx == n:
        print(sel)
        return

    for i in range(n):
        # i 번째 원소를 활용했으면 안 쓰고 넘어감
        if check & (1<<i): 
            continue

        sel[idx] = arr[i]
        perm(idx+1, check | (1<<i)) # 원상복구가 필요 없음

perm(0,0)
# => 결과는 위와 동일
```

```python
# 스왑 방식
def perm(idx):
    if idx == n:
        print(arr)

    else:
        for i in range(idx, n):
            # 순서를 바꾸고
            arr[idx], arr[i] = arr[i], arr[idx]
            perm(idx + 1)
            
            # 원상복구
            arr[idx], arr[i] = arr[i], arr[idx]

perm(0)
# => 결과는 위와 동일
```



# [참고] 부분 집합의 합

```python
# 부분 집합의 합 (재귀) 1
def f(i, N):
    if i == N:
        print(bit, end=' ')
        s = 0
        for j in range(N):
            if bit[j]:
                s += A[j]
                print(A[j], end=' ')
        print(f': {s}')
        return
    else:
        bit[i] = 1
        f(i+1, N)
        bit[i] = 0
        f(i+1, N)
        return

A = [1, 2, 3]
bit = [0] * 3
f(0, 3)
```

```python
# 부분 집합의 합 (재귀) 2
def f(i, N, s): # s: i-1원소까지 부분 집합의 합 (포함된 원소의 합)
    if i == N:
        print(bit, end=' ')
        print(f': {s}')
        return
    else:
        bit[i] = 1  # 부분 집합에 A[i] 포함
        f(i+1, N, s+A[i])
        bit[i] = 0  # 부분 집합에 A[i] 빠짐
        f(i+1, N, s)
        return

A = [1, 2, 3]
bit = [0] * 3
f(0, 3, 0)
```

```python
def f(i, N, s): # s: i-1원소까지 부분 집합의 합 (포함된 원소의 합)
    global cnt
    cnt += 1
    if i == N:
        if s == 10:
            print(bit)
        return
    else:
        bit[i] = 1  # 부분 집합에 A[i] 포함
        f(i+1, N, s+A[i])
        bit[i] = 0  # 부분 집합에 A[i] 빠짐
        f(i+1, N, s)
        return

# 1부터 10까지 원소인 집합 중, 부분 집합의 합이 10인 경우는?
N = 10
A = [i for i in range(1, N+1)]
bit = [0] * N
cnt = 0
f(0, N, 0)
print(cnt)	# 2047
```

```python
# 백트래킹
def f(i, N, s, t): # s: i-1원소까지 부분 집합의 합 (포함된 원소의 합), t: 찾으려는 합
    global cnt
    cnt += 1
    if s == t:		# i-1 원소까지의 합이 찾는 값인 경우
        print(bit)
        return
    elif i == N:	# 모든 원소에 대한 고려가 끝난 경우
        return
    elif s > t:		# 남은 원소를 고려할 필요가 없는 경우
        return
    else:			# 남은 원소가 있고 s < t인 경우
        bit[i] = 1
        f(i+1, N, s+A[i], t)	# i 원소 포함
        bit[i] = 0
        f(i+1, N, s, t)	 	# i 원소 미포함
        return

# 1부터 10까지 원소인 집합 중, 부분 집합의 합이 10인 경우는?
N = 10
A = [i for i in range(1, N+1)]
bit = [0] * N
cnt = 0
f(0, N, 0, 10)
print(cnt)	# 349 (호출 횟수 줄임)
```

- i원소의 포함 여부를 결정하면 i까지의 부분 집합의 합 s_i를 결정할 수 있음
- s_i-1이 찾고자 하는 부분 집합의 합보다 크면 남은 원소를 고려할 필요가 없음



# [참고] 순열

```python
def f(i, N):
    if i == N:  # 순열 완성
        print(A)
    else:
        for j in range(i, N):   # 자신부터 오른쪽 끝까지
            A[i], A[j] = A[j], A[i]
            f(i+1, N)
            A[i], A[j] = A[j], A[i]     # 원상복구

A = [0, 1, 2]
f(0, 3)
'''
[0, 1, 2]
[0, 2, 1]
[1, 0, 2]
[1, 2, 0]
[2, 1, 0]
[2, 0, 1]
'''
```

```
# swea 4881
total = 0

for k: 0 -> 2 # 모든 행에서
total += arr[k][A[k]]	# A[k]에 가면 열 번호가 들어가 있음
```



# 분할 정복 알고리즘

- 분할(Divide): 해결할 문제를 여러 개의 작은 부분으로 나눔
- 정복(Conquer): 나눈 작은 문제를 각각 해결함
- 통합(Combine): (필요하다면) 해결된 해답을 모음



## 거듭 제곱(Ecponentiation)

- 분할 정복 기반의 알고리즘을 사용했을 때 시간 복잡도 : O(n) -> **O(log2n)**

```python
def f1(b, e):
    global cnt1
    if b == 0:
        return 1
    r = 1
    for i in range(e):
        r *= b
        cnt1 += 1
    return r

def f2(b, e):
    global cnt2
    if b == 0 or e == 0:
        return 1
    if e % 2:  # 홀수면
        r = f2(b, (e-1)//2)
        return r*r*b
    else:   # 짝수면
        r = f2(b, e//2)
        cnt2 += 1
        return r*r

cnt1 = 0
cnt2= 0
print(f1(2, 20), cnt1)
print(f2(2, 20), cnt2)
```
