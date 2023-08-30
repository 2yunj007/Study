# 재귀 함수(Recursion function)

- 함수 내부에서 직접 혹은 간접적으로 자기 자신을 호출하는 함수
- 일반적으로 재귀적 정의를 이용해서 재귀 함수를 구현함
- 따라서, 기본 부분(basis part)와 유도 부분(inductive part)로 구성됨
- 재귀적 프로그램을 작성하는 것은 반복 구조에 비해 간결하고 이해하기 쉬움
- 함수 호출은 프로그램 메모리 구조에서 스택을 사용함
- 따라서, 재귀 호출은 반복적인 스택의 사용을 의미하며 메모리 및 속도에서 성능 저하가 발생함

```python
def f(i, N):	# i: 현재 상태, N: 목표, key
    if i == N:
        print(B)	# [1, 2, 3, 4, 5]
        return
    else:
        B[i] = A[i]
        f(i+1, N)
        
N = 5
A = [1, 2, 3, 4, 5]
B = [0]*N
f(0, N)
```

```python
# key가 있으면 1, 없으면 0을 리턴하는 함수
def f(i, N, key, arr):	# i: 현재 상태, N: 목표, key: 찾고자 하는 원소
    if i == N:
        return 0 	# key가 없는 경우
    elif arr[i] == key:
        return 1
    else:
        f(i+1, N, key, arr)
        
N = 5
A = [1, 2, 3, 4, 5]
key = 3
print(f(0, N, key, A))
```



## 반복과 재귀의 비교

|                    | 재귀                                                     | 반복                  |
| ------------------ | -------------------------------------------------------- | --------------------- |
| **종료**           | 재귀 함수 호출이 종료되는 <br />베이스 케이스(base case) | 반복문의 종료 조건    |
| **수행 시간**      | 상대적 느림                                              | 빠름                  |
| **메모리 공간**    | 상대적 많이 사용                                         | 적게 사용             |
| **소스 코드 길이** | 짧고 간결                                                | 긺                    |
| **소스 코드 형태** | 선택 구조(if...else)                                     | 반복 구조(for, while) |
| **무한 반복 시**   | 스택 오버플로우                                          | CPU를 반복해서 점유   |



## 예시

### 팩토리얼 재귀 함수

#### 재귀적 정의

**Basis rule**

N <= 1 경우, n=1

**Inductive rule**

N > 1, n! = n * (n - 1)!



### 선택 정렬(Selection Sort)의 재귀적 알고리즘

```python
def SelectionSort(n, N, arr):
    # try
```



# 완전 검색 기법

- 완전 검색 방법은 문제의 해법으로 **생각할 수 있는 모든 경우의 수를 나열해보고 확인**하는 기법
- `Brute-force` 혹은 `generate-and-text` 기법이라고도 불림
- 모든 경우의 수를 테스트한 후, 최종 해법을 도출
- 일반적으로 경우의 수가 상대적으로 작을 때 유용

- 모든 경우의 수를 생성하고 테스트하기 때문에 **수행 속도는 느리지만, 해답을 찾아내지 못할 확률은 적음**
- 평가 등에서 주어진 문제를 풀 때, 우선 완전 검색으로 접근하여 해답을 도출
- 성능 개선을 위해 다른 알고리즘을 사용하고 해답을 확인하는 것이 바람직한 접근



# 순열(Permutation)

- 서로 다른 것들 중 몇 개를 뽑아서 한 줄로 나열하는 것
- nPr : 서로 다른 n개 중 r개를 택하는 순열
  - nPr = n * (n - 1) x (n - 2) x ... x (n - r + 1)
  - nPn = n!

- 다수의 알고리즘 문제들은 순서화된 요소들의 집합에서 최선의 방법을 찾는 것과 관련 있음
  - ex) TSP(Traveling Salesman Problem)
- N개의 요소들에 대해서 n! 개의 순열들이 존재



## 재귀 호출을 통한 순열 생성

```python
# p[]: 데이터가 저장된 배열
# k: 원소의 개수, n: 선택된 원소의 수
perm(i, k)
	if i == k
    	print array		# 원하는 작업 수행
    else
    	for j : i -> k-1
        	p[i] <-> p[j]
            perm(i+1, k)
            p[i] <-> p[j]
```

```python
def f(i, N):	# i: 이전에 고른 개수
    if i == N:  # 순열 완성
        print(p)
        return
    else:   # p[i]에 들어갈 숫자를 결정
        for j in range(N):
            if used[j] == 0:    # 아직 사용되기 전이면
                p[i] = card[j]
                used[j] = 1
                f(i+1, N)
                used[j] = 0


card = [1, 2, 3]
used = [0] * 3  # 이미 사용한 카드인지 표시
p = [0] * 3
f(0, 3)
'''
[1, 2, 3]
[1, 3, 2]
[2, 1, 3]
[2, 3, 1]
[3, 1, 2]
[3, 2, 1]
'''
```

```python
# N 개에서 K 개를 고르는 순열
def f(i, N, K):
    global cnt
    if i == K:  # 순열 완성: K 개를 모두 고른 경우
        cnt += 1
        print(cnt, p)
        return
    else:   # p[i]에 들어갈 숫자를 결정
        for j in range(N):
            if used[j] == 0:    # 아직 사용되기 전이면
                p[i] = card[j]
                used[j] = 1
                f(i+1, N, K)
                used[j] = 0


card = [1, 2, 3, 4, 5]
N = 5   # N 개의 숫자에서
K = 3   # K 개를 골라 만드는 순열
used = [0] * N  # 이미 사용한 카드인지 표시
p = [0] * N
cnt = 0
f(0, N, K)
```



# 부분 집합

- 집합에 포함된 원소들을 선택하는 것
- 다수의 중요 알고리즘들이 원소들의 그룹에서 최적의 부분 집합을 찾는 것
  - ex) 배낭 짐 싸기(knapsack)
- N개의 원소를 포함한 집합
  - 자기 자신과 공집합 포함한 모든 부분 집합(power set)의 개수는 2^n 개
  - 원소의 수가 증가하면 부분 집합의 개수는 지수적으로 증가



## 부분 집합 생성 방법

### 바이너리 카운팅을 통한 사전적 순서(Lexicographic Order)

- 부분 집합을 생성하기 위한 가장 자연스러운 방법
- 바이너리 카운팅(Bibary Counting)은 사전적 순서로 생성하기 위한 가장 간단한 방법



### 바이너리 카운팅(Binary Counting)

- 원소 수에 해당하는 N개의 비트열을 이용
- n 번째 비트 값이 1이면 n 번째 원소가 포함되었음을 의미

```python
arr = [3, 6, 7, 1, 5, 40]
n = len(arr)

for i in range(1, 1<<(n-1)):	# 1<<n 부분 집합의 개수, 1<<(n-1) == (1<<n)//2
    subset1 = []
    subset2 = []
	for j in range(n):	# 원소의 수만큼 비트를 비교
        if i & (1<<j):	# i의 j 번째 비트가 1이면 j 번째 원소 출력
            subset1.append(arr[j])
        else:
            subset2.append(arr[j])
    print(subset1, subset2)
```

