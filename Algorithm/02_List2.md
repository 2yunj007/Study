# 2차원 배열

## 2차원 배열의 선언

- 1차원 List를 묶어 놓은 List
- 2차원 이상의 다차원 List는 차원에 따라 Index 선언
- 2차원 List의 선언: 세로 길이 (행의 개수), 가로 길이(열의 개수)를 필요로 함
- Python에서는 데이터 초기화를 통해 변수 선언과 초기화 가능



## 배열 순회

n x m 배열의 n*m개의 모든 원소를 빠짐없이 조사하는 방법



### 행 우선 순회

```python
# i 행의 좌표
# j 열의 좌표
for i in range(n):
    for j in range(m):
        f(Array[i][j])	# 필요한 연산 수행
```

```python
# 각 행의 합의 최댓값 구하기
max_v = 0
for i in range(N):
    row_total = 0
    for j in range(M):
        row_total += arr[i][j]
    if max_v < row_total:
        max_v = row_total
```



#### 열 우선 순회

```python
# i 행의 좌표
# j 열의 좌표

for j in range(m):
    for i in range(n):
        f(Array[i][j])
```



#### 지그재그 순회

```python
# i 행의 좌표
# j 열의 좌표

for i in range(n):
    for j in range(m):
        f(Array[i][j + (m-1-2*j) * (i%2)])
        # (i%2) : 짝수 행에서는 좌 -> 우, 홀수 행에서는 우 -> 좌
        # j + (m-1-2*j) : m-1-j 를 변형 (역순으로 진행)
```



## 비어 있는 배열

```python
N = 2   # 행의 크기
M = 4   # 열의 크기
arr = [[0]*M for _ in range(N)] # [[0, 0, 0, 0], [0, 0, 0, 0]]

arr2 = [[0]*M]*N
# 이렇게 하면 안 되는 이유
# 하나의 열을 참조하는 두 개의 행이 생기는 것임

arr[0][0]= 1    # [[1, 0, 0, 0], [0, 0, 0, 0]]
arr[0][0] = 1   # [[1, 0, 0, 0], [1, 0, 0, 0]]
```



## 델타를 이용한 2차 배열 탐색

- 2차 배열의 한 좌표에서 4 방향의 인접 배열 요소를 탐색하는 방법

```python
arr[0...N-1][0...N-1] # NxN 배열

di[] <- [0, 1, 0, -1]	# y, x 방향
dj[] <- [1, 0, -1, 0]

for i : 0 -> N-1 :
	for j : 0 -> N-1:
		for k in range(4):
			nj <- i + di[k]
			nj <- j + dj[k]
			
			if 0 <= ni < N and 0 <= nj < N	# 유효한 인덱스면 (주어진 영역을 벗어나지 않을 경우)
				f(arr[ni][nj])
```



```python
# 4방향 주변 원소를 포함한 합의 최댓값
'''
3
1 2 3
4 5 6
7 8 9
'''

di = [0, 1, 0, -1]
dj = [1, 0, -1, 0]
N = int(input())
arr = [list(map(int, input().split())) for _ in range(N)]
m = 2

max_v = 0   # 모든 원소가 0 이상이라면
for i in range(N):  # 모든 원소 arr[i][j]에 대해
    for j in range(N):
        # arr[i][j] 중심으로
        s = arr[i][j]
        for k in range(4):  # 4방향에 대해서
            for p in range(m):  # 상하좌우로 m개씩 찾아야 한다면 추가
                ni , nj = i+di[k]*p, j+dj[k]*p
            if 0<=ni<N and 0<=nj<N: # 배열을 벗어나지 않으면
                s += arr[ni][nj]
        # 여기까지 주변 원소를 포함한 합
        if max_v < s:
            max_v = s          
```



## 전치 행렬

- 대각선 원소는 그대로 있고 나머지만 바꾸기

```python
# i : 행의 좌표, len(arr)
# j : 열의 좌표, len(arr[0])
arr = [[1, 2, 3], [4, 5, 6], [7, 8, 9]]	# 3*3 행렬

for i in range(3):
    for j in range(3):
        if i < j:
            arr[i][j], arr[j][i] = arr[j][i], arr[i][j]
```



## 배열 순회 정리

```python
# 행렬 탐색 연습
# 0으로 초기화된 N * M의 2차원 배열 생성하기
from pprint import pprint as pp
m = 5
n = 5

arr = []
for i in range(n):
    row = [0] * m
    arr.append(row)
# pp(arr)
# [[0, 0, 0, 0, 0],
#  [0, 0, 0, 0, 0],
#  [0, 0, 0, 0, 0],
#  [0, 0, 0, 0, 0],
#  [0, 0, 0, 0, 0]]

num_list = [[1, 2, 3],
            [4, 5, 6],
            [7, 8, 9]]


# 1. 행 우선 순회 정상적으로, 순서대로 진행되는지 확인
for r in range(len(num_list)):
    for c in range(len(num_list)):
        # print(f'{num_list[r][c]}', end = ' ')   # 1 2 3 4 5 6 7 8 9
        pass


# 2. 열 우선 순회
for c in range(len(num_list)):
    for r in range(len(num_list)):
        # print(f'{num_list[r][c]}', end = ' ')   # 1 4 7 2 5 8 3 6 9
        pass


# 3. 역-행 우선 순회
for i in range(len(num_list)):
    for j in range(len(num_list)-1, -1, -1):
        #print(num_list[i][j], end = ' ')    # 3 2 1 6 5 4 9 8 7
        pass


# 4. 역-열 우선 순회
for j in range(len(num_list)-1, -1, -1):
    for i in range(len(num_list)):
        # print(num_list[i][j], end = ' ')    # 3 6 9 2 5 8 1 4 7
        pass


# 가장자리의 합
def edge_sum(arr):
    # 순회를 하면서, 2차원 리스트의 가장자리에 있는 원소들을 합함
    edge_sum_result = 0
    for i in range(len(arr)):
        for j in range(len(arr[0])):
            if i == 0 or i == len(arr) - 1 or j == 0 or j == len(arr) - 1:
                print(arr[i][j], end = ' ') # 1 2 3 4 6 7 8 9
                edge_sum_result += arr[i][j]

    return edge_sum_result


result = edge_sum(num_list)
# print()
# print((result))   # 40


# 델타 탐색
# 문제에 제시된 제약 조건에 따라 탐색 순서는 달라질 수 있음
# ex) 대각선, 하우좌상, 1이면 상 / 2이면 하

        # 상 하 좌 우
dx = [-1, 1, 0, 0]
dy = [0, 0, -1, 1]
# 튜플로 하는 게 더 나을 수도 있음, 내가 편한 대로

# 대각선
    # 좌상단 대각선 / 좌하단 대각선 / 우하단 대각선 / 우상단 대각선
dxy = [(-1, -1), (-1, 1), (1, 1), (1, -1)]

for nx, ny in [[-1, 0], [1, 0], [0, -1], [0, 1]]:
    pass


def without_delta():
    print(num_list[r - 1][c])   # 상
    print(num_list[r + 1][c])   # 하
    print(num_list[r][c - 1])   # 좌
    print(num_list[r][c + 1])   # 우


# 벽을 세워야 함
# 주어진 2차원 리스트의 범위를 벗어나지 않도록 제한을 두는 행위

# 1. 벽의 제한을 두는데, 벽을 넘어가는 경우, 아무것도 하지 않음
# 2. 벽을 넘어가지 않는 경우에만 기능을 수행함

x = 0
y = 1
N = 0 # 임의의 수

for d in range(4):
    # 다음에 이동할(탐색할) 좌표 담기
    nx = x + dx[d]
    ny = y + dy[d]

    # map을 벗어나는 경우 아무것도 하지 않기
    if nx < 0 or nx >= N or ny < 0 or ny >= N:
        continue
        # print(결과 프린트)
        # 로직 수행

    # 벽을 넘어가지 않는 경우에만 수행하기
    if 0 <= nx < N and 0 <= ny < N:
        continue
        # 로직 수행


def isSafeArea(nx, ny):
    if 0 <= nx < N and 0 <= ny < N:
        return True
    return False
```



# 부분집합

- 집합의 원소가 n개일 때, 공집합을 포함한 부분집합의 수는 2^n개



### 부분집합 생성

- 각 원소가 부분집합에 포함되었는지를 loop를 이용하여 확인하고 부분집합을 생성하는 방법

```python
def print_subset(bit, arr, n):
    total = 0 # 부분집합의 합
    for i in range(n):
        if bit[i]:
            print(arr[i], end = ' ')
            total += arr[i]
    print(bit, total)

arr = [1, 2, 3, 4]
bit = [0, 0, 0, 0]
for i in range(2):
    bit[0] = i					# 0번 원소
    for j in range(2):
		bit[1] = j				# 1번 원소
		for k in range(2):
			bit[2] = k			# 2번 원소
            for l in range(2):
                bit[3] = l		# 3번 원소
                print_subset(bit, arr, 4)
```



# 비트 연산자

- '&' : 비트 단위로 AND 연산

- '|' : 비트 단위로 OR 연산

- '<<' : 피연산자의 비트 열을 왼쪽으로 이동

- '>>' : 피연산자의 비트 열을 오른쪽으로 이동

- **<< 연산자**
  - 1 << n : 2^n 즉, 원소가 n개일 경우 모든 부분집합의 수를 의미


- **& 연산자**
  - i & (1<<j) : i의 j 번째 비트가 1인지 아닌지 검사




### 비트 연산자를 활용해 부분집합 구하기

```python
arr = ['Fish', 'And', 'Chip']
N = len(arr)    # 배열의 길이

# 부분집합을 구하는 블록: arr에 대한 모든 경우의 수
for i in range(1 << N):
    for j in range(N):
        if i & (1 << j):
            print(arr[j], end = ' ')
    print()

'''
공집합
Fish
And
Fish And
Chips
Fish Chips
Fish And Chips
'''

# 1 << 2의 의미

N = 3
print(1 << N)

'''
2진수 | 10진수 | shift 횟수
---------------------------
0001 | 1      | 0
0010 | 2      | 1
0100 | 4      | 2
1000 | 8      | 3
---------------------------
'''
# 3개의 원소는 가지고, 모든 경우의 수는 2^3
```



- 정식 님 정리

```python
# arr 에 대한 모든 부분 집합의 경우의 수
for i in range(1 << N):  # 0 ~ 2^n 까지의 숫자, 즉 경우의 수의 모든 가지 수를 반복문을 통해서 구현
    for j in range(N):   # i 에 대응하는 숫자 중 arr 배열에서 index 값을 이용해 부분집합을 생성
        if i & (1 << j): 
            print(arr[j], end=' ')
0 1 2 3 4 5 6
0 01 10 11 100
# j 값은 binary 에서 2^n의 각각의 n 의 값이다.
# i 의 값(1 ~ N) : 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | ... | 2^n|
# binary(2^n)   : 1 | 10| 11|100|101|110|111|1000| ... | bin(2^n)|

# j 의 값(n)     : 0|1 | 2 |  3 |   4 |    5 |     6 |      7 |       8 |        9
# binary(n)     : 0| 1 |10 |100 |1000 |10000 |100000 |1000000 |10000000 |100000000
# 1 << j        : 1|10|100|1000|10000|100000|1000000|10000000|100000000|1000000000

# i 의 2진수 값에 들어있는 1 << j 값을 찾아서 부분집합의 요소로 추가한다. 
# i 의 값(1 ~ N), 1 << j
# 1 => 1           1 (0)
# 2 => 10        10 (1)
# 3 => 11        1 (0), 10(1)
# 4 => 100        100 (2)
# 5 => 101        1 (0), 100 (2)
# 6 => 110        10 (1), 100 (2)
# 7 => 111        1 (0), 10 (1), 100 (2)
# 8 => 1000        1000 (4)
```



# 순차 검색 (sequential search)

- 가장 간단하고 직관적인 검색 방법
- 배열이나 연결 리스트 등 순차구조로 구현된 자료 구조에서 원하는 항목을 찾을 때 유용함
- 알고리즘이 단순하여 구현이 쉽지만, 검색 대상의 수가 많은 경우에는 수행시간이 급격히 증가하여 비효율적



## 정렬되어 있지 않은 경우

### 검색 과정

1. 첫 번째 원소부터 순서대로 검색 대상과 키 값이 같은 원소가 있는지 비교하며 찾음

2. 키 값이 동일한 원소를 찾으려면 그 원소의 인덱스 반환

3. 지료구조의 마지막에 이를 때까지 검색 대상을 찾지 못하면 검색 실패

```python
def sequentialSearch(a, n, key)
	i <- 0
    while i<n and a[i] != key:
        i <- i+1
    if i<n : return i
	else : return -1
```



### 시간 복잡도

**O(n)**

- 찾고자 하는 원소의 순서에 따라 비교 회수가 결정



## 정렬되어 있는 경우

### 검색 과정

1. 자료가 오름차순으로 정렬되어 있는 경우, 자료를 순차적으로 검색하며 키 값 비교

2. 원소의 키 값이 검색 대상의 키 값보다 크면 찾는 원소가 없는 것이므로 검색 종료

```python
def sequentialSearch(a, n, key)
	i <- 0
    while i<n and a[i] != key:
        i <- i+1
    if i<n and a[i] == key :
        return i
	else :
        return -1
```



# 이진 검색 (binary search)

: 자료의 가운데에 있는 항목의 키 값과 비교하여 다음 검색의 위치를 결정하고 검색을 계속 진행하는 방법

- 목적 키를 찾을 때까지 이진 검색을 순환적으로 반복 수행함으로써 검색 범위를 반으로 줄여 가면서 보다 빠르게 검색 수행
- 이진 검색을 하기 위해서는 자료가 정렬된 상태여야 함



### 검색 과정

1. 자료의 중앙에 있는 원소를 고름

2. 중앙의 원소의 값과 찾고자 하는 목표 값을 비교

3. 목표 값이 중앙 원소의 값보다 작으면 자료의 왼쪽 반에 대해서 새로 검색을 수행하고, 크다면 오른쪽 반에 대해서 새로 검색 수행

4. 값을 찾을 때까지 반복



### 구현

- 검색 범위의 시작점과 종료점을 이용하여 검색을 반복 수행
- 자료에 삽입이나 삭제가 발생했을 때 배열의 상태를 항상 정렬 상태로 유지하는 추가 작업 필요

```python
def binarySearch(a, N, key)
	start = 0
    end = N - 1
    while start <= end:
        middle = (start + end) // 2
        if a[middle] == key:	# 검색 성공
            return True
        elif a[middle] > key:
            end = middle - 1
        else:
            start = middle + 1
    return False	# 검색 실패
```



### 재귀 함수 이용

* 참고

```python
def binarySearch(a, low, high, key):
    if low > high: # 검색 실패
        return False
    else:
        middle = (low + high) // 2
        if key == a[middle]:	# 검색 성공
            return True
        elif key < a[middle]:
            return binarySearch(a, low, middle-1, key)
        elif a[middle] < key
        return binarySearch2(a, middle+1, high, key)
```



### 시간 복잡도

**O(logN)**



# 선택 정렬 (Selection Sort)

: 주어진 자료들 중 가장 작은 값의 원소부터 차례대로 선택하여 위치를 교환하는 방식

- 셀렉션 알고리즘을 전체 자료에 적용한 것



### 정렬 과정

1. 주어진 리스트 중에서 최솟값을 찾음

2. 그 값을 리스트의 맨 앞에 위치한 값과 교환

3. 맨 처음 위치를 제외한 나머지 리스트 대상으로 위의 과정 반복

![selection-sort-001.gif](https://github.com/GimunLee/tech-refrigerator/blob/master/Algorithm/resources/selection-sort-001.gif?raw=true)

```python
def SelectionSort(a, N):
	for i in range(N-1):
        minIdx = i
        for j in range(i+1, N):
            if a[minIdx] > a[j]:
                minIdx = j
        a[i], a[minIdx] = a[minIdx], a[i]
```



### 시간 복잡도

**O(n2)**



## 셀렉션 알고리즘 (Selection Algorithm)

: 저장되어 있는 자료로부터 k번째로 큰 혹은 작은 원소를 찾는 방법

- 최솟값, 최댓값 혹은 중간값을 찾는 알고리즘



### 선택 과정

1. 정렬 알고리즘을 이용하여 자료 정렬하기
2. 원하는 순서에 있는 원소 가져오기



### k 번째로 작은 원소를 찾는 알고리즘

- 1번부터 k번째까지 작은 원소들을 찾아 배열의 앞쪽으로 이동시키고, 배열의 k번째를 반환 
- k가 비교적으로 작을 때 유용하며 O(kn)의 수행시간을 필요로 함

```python
def select(arr, k):
    for i in range(0, k):
        minIndex = i
        for j in range(i+1, len(arr)):
            if arr[minIndex] > arr[j]:
                minIndex = j
        arr[i], arr[minIndex] = arr[minIndex], arr[i]
    return arr[k-1]
```
