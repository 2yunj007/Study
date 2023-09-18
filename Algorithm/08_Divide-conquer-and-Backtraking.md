# 분할 정복 & 백트래킹

## 분할 정복(divide and conquer)

> 문제를 나눌 수 없을 때까지 나누어서 각각을 풀면서 다시 합병하여 문제의 답을 얻는 알고리즘

- 대표적인 예로는 퀵 정렬, 합병 정렬, 이진 탐색, 선택 문제, 고속 푸리에 변환(FFT) 문제들이 대표적

- **분할(Divide)** :문제가 분할이 가능한 경우, 2개 이상의 문제로 나눔
- **정복(Conquer)** : 나누어진 문제가 여전히 분할이 가능하면, 또 다시 Divide를 수행. 그렇지 않으면 문제를 품
- **통합(Combine)** : Conque한 문제들을 통합하여 원래 문제의 답을 얻음



### 거듭 제곱

- n 거듭 제곱은 자신을 n번 곱해야 하므로 O(n)의 시간이 소요됨
- C^8 = C * C * C * C * C * C * C * C
- 위 식을 다음과 같이 표현할 수도 있음 -> C^8 = C^4 * C^4 = (C^4)^2 = ((C^2)^2)^2

```
Recursive_Power(x, n)
	IF n == 1 : RETURN x
	IF n is even
		y <- Recursive_Power(x, n/2)
		RETURN y * y
	ELSE
		y <- Recursive_Power(x, (n-1)/2)
		RETURN y * y * x
```

- 지수가 짝수일 대는 지수를 반으로 나눠서 곱함
- 지수가 홀수일 대는 지수에서 1을 빼고 반으로 나누어서 곱하고 밑을 한 번 더 곱함
- 이렇게 지수를 반으로 나눠가는 거듭 제곱 알고리즘의 수행 시간은 **O(logn)**



### 병합 정렬(Merge Sort)

- 여러 개의 정렬된 자료의 집합을 병합하여 한 개의 정렬된 집합으로 만드는 방식
- 자료를 최소 단위의 문제까지 나눈 후에 차례대로 정렬하여 최종 결과를 얻어냄 (top-down 방식)
- 병합 정렬은 외부 정렬의 기본이 되는 정렬 알고리즘 
  - Multi-Core CPU나 다수의 프로세서엣 정렬 알고리즘을 병렬화하기 위해 병합 정렬 알고리즘 활용
- 시간 복잡도 : **O(nlogn)**



**동작 과정**

1. 입력 배열을 같은 크기의 2개의 부분 배열로 분할
2. 부분 배열을 정렬함. 부분 배열의 크기가 충분히 작지 않으면 (left<right) 순환 호출을 이용하여 다시 분할 정복 방법을 적용
3. 정렬된 부분 배열들을 하나의 배열에 합병

<img src="https://blog.kakaocdn.net/dn/pfWKt/btrhZNbm0OZ/6EkiqFde7OpgQjlp5e0kKk/img.png" alt="img" style="zoom:67%;" />

- 분할 과정

```
merge_sort(LIST m)
	IF length(m) == 1 : RETURN m
	
	LSIT left, right
	middle <- length(m) / 2
	FOR x in m before middle
		add x to left
	FOR x in m after or equal middle
		add x to right
		
	lefr <- merge_sort(left)
	right <- merge_sort(right)
	
	RETURN merge(left, right)
```

- 병합 과정

```
merge(LIST left, LIST right)
	LIST result
	
	WHILE legnth(left) > 0 OR length(right) > 0
		IF length(left) > 0 AND length(right) > 0
			IF first(left) <= first(right)
				append popfirst(left) to result
			ELSE
				append popfirst(right) to result
		ELIF length(left) > 0
			append popfirst(left) to result
		ELIF length(right) > 0
			append popfirst(right) to result
	RETURN result
```

- 구현

```python
arr = [69, 10, 30, 2, 16, 8, 31, 22]


def merge(left, right):
    result = []

    # 한 쪽이 빌 때까지 반복
    while len(left) > 0 or len(right) > 0:
        # 둘 다 남아 있으면, 두 리스트의 가장 앞에 있는 것 중 작은 것을 선택하여 result 에 추가
        if len(left) > 0 and len(right) > 0:
            if left[0] <= right[0]:
                result.append(left.pop(0))
            else:
                result.append(right.pop(0))
        # 한 쪽만 남아있으면, 남은것을 모두 result 에 추가
        elif len(left) > 0:
            result.append(left.pop(0))
        elif len(right) > 0:
            result.append(right.pop(0))

    return result

def merge_sort(unordered_list):
    # 길이가 1인 배열까지 나누면 stop
    if len(unordered_list) == 1:
        return unordered_list

    left = []
    right = []
    middle = len(unordered_list) // 2
    # 왼쪽을 따로 리스트에 저장
    for el in unordered_list[:middle]:
        left.append(el)

    # 오른쪽을 따로 리스트에 저장
    for el in unordered_list[middle:]:
        right.append(el)

    left = merge_sort(left)
    right = merge_sort(right)

    return merge(left, right)


arr = merge_sort(arr)
print(arr)

```

- 내가 전에 풀었던 코드

```python
def MergeSort(arr):
    global cnt
    # 길이가 1인 리스트 반환
    if len(arr) == 1:
        return arr

    mid = len(arr) // 2

    left_arr = MergeSort(arr[:mid])    # 분할된 리스트의 왼쪽
    right_arr = MergeSort(arr[mid:])   # 분할된 리스트의 오른쪽

    merge_arr = []
    i, j = 0, 0

    while i < len(left_arr) and j < len(right_arr):
        if left_arr[i] <= right_arr[j]:
            merge_arr.append(left_arr[i])
            i += 1
        elif left_arr[i] >= right_arr[j]:
            merge_arr.append(right_arr[j])
            j += 1

    if i >= len(left_arr):
        merge_arr += right_arr[j:]
    elif j >= len(right_arr):
        merge_arr += left_arr[i:]

    return merge_arr
```



### 퀵 정렬(Quick Sort)

- 병합 정렬은 그냥 두 부분으로 나누는 반면에, 퀵 정렬은 분할할 때, 기준 아이템(pivot item) 중심으로, 이보다 작은 것은 왼편, 큰 것은 오른편에 위치시킴
- 각 부분 정렬이 끝난 후, 병합 정렬은 병합 작업이 필요하나 퀵 정렬은 필요로 하지 않음
- 매우 큰 입력 데이터에 대해서 좋은 성능을 보임
- 시간 복잡도 : **O(nlogn)** 
  - 최악의 경우 (역순으로 정렬이 되어 있는 경우 등) :  **O(n^2)**



**동작 과정**

[그림으로 이해하기](https://ldgeao99.tistory.com/376)

1. 피봇 하나를 선택하여 피봇을 기준으로 2개의 부분 배열로 분할
2. 왼쪽부터 피봇보다 큰 값을 찾고 오른쪽부터는 피봇보다 작은 값을 찾아서 두 원소를 교환함. 분할된 부분 배열의 크기가 0이나 1이 될 때까지 반복

<img src="https://blog.kakaocdn.net/dn/bpzifb/btrhZnjD2mD/z4sh5giBUZ9Cjhe2WW5xUK/img.png" alt="img" style="zoom:67%;" />

```
quickSort(A[], l, r)
	IF l < r
		s <- partition(a, l, r)
		quickSort(A[], l, s - 1)
		quickSort(A[], s + 1, r)
```



#### Hoare-Partition 알고리즘

```
partition(A[], l, r)
	p <- A[l]	// p: 피봇 값
	i <- j, j <- r
	WHILE i <= j
		WHILE i <= j and A[i] <= p : i++
		WHILE i <= j and A[j] >= p : j--
		IF i < j : swap(A[i], A[j])
	
	swap(A[l], A[j])
	RETURN j
```

```python
# 배열, 왼쪽, 오른쪽
def quick_sort(arr, left, right):
    if left < right:
        mid = cal(arr, left, right)
        quick_sort(arr, left, mid - 1)
        quick_sort(arr, mid + 1, right)

def cal(arr, left, right):
    # 피봇보다 큰 구간의 왼쪽 경계
    i = left - 1
    # 피봇보다 큰 구간의 오른쪽 경계
    j = left
    # 호어 파티션
    pivot = arr[right]
    while j < right:
        if pivot > arr[j]:
            # 피봇이 j 위치 값보다 크면 i를 이동
            i += 1
            # i와 j가 서로 다른 위치에 있다면,
            # i와 j 사이 구간에 피봇보다 큰 값이 있다.
            if i < j:
                arr[i], arr[j] = arr[j], arr[i]
        j += 1
    arr[i + 1], arr[right] = arr[right], arr[i + 1]
    return i + 1

nums = [11, 45, 23, 81, 28, 34]
quick_sort(nums, 0, len(nums)-1)
print(nums)
```



#### Lomuto partition 알고리즘

```
partition(A[], p, r)
	x <- A[r]
	i <- p - 1
	
	FOR j in p -> r - 1
	IF A[j] <= x
		i ++, swap(A[i], A[j])
		
	swap(A[i+1], A[r])
	RETURN i + 1
```

```python
def quick_sort(arr):
    # 분할 정복
    if len(arr) <= 1:
        return arr
    else:
        # 분할 작업
        pivot = arr[0]
        left, right = [], []
        for i in range(1, len(arr)):
            if arr[i] > pivot:  # 로무토 (Lomuto)
                right.append(arr[i])
            else:
                left.append(arr[i])
        return [*quick_sort(left), pivot, *quick_sort(right)]
nums = [55, 45, 23, 81, 28, 60]
print(quick_sort(nums))
```



### 이진 검색(Binary Search)

- 자료의 가운데에 있는 항목의 키 값과 비교하여 다음 검색 위치를 결정하고 검색을 계속 진행하는 방법
- 목적 키를 찾을 때까지 이진 검색을 순환적으로 반복 수행함으로써 검색 범위를 반으로 줄여 가면서 보다 빠르게 검색을 수행함
- 이진 검색을 하기 위해서는 자료가 정렬된 상태여야 함



**검색 과정**

1. 자료의 중앙에 있는 원소를 고름
2. 중앙 원소의 값과 찾고자 하는 목표 값을 비교
3. 목표 값이 중앙 원소의 값보다 작으면 자료의 왼쪽 반에 대해서 새로 검색을 수행하고, 크다면 자료의 오른쪽 반에 대해서 새로 검색을 수행
4. 찾고자 하는 값을 찾을 때까지 위 과정을 반복

```
binarySearch(n, S[], key)
	low <- 0
	high <- n - 1
	
	WHILE low <= high
		mid <- low + (high - low) / 2
		
		IF S[mid] == key
			RETURN mid
		ELIF S[mid] > key
			high <- mid - 1
		ELSE
			low <- mid + 1
	RETURN -1
```

```py
# 이진 검색 - 반복문

def binarySearch(target):
    low = 0
    high = len(arr) - 1

    # low <= high라면 데이터를 못 찾은 경우
    while low <= high:
        mid = (low + high) // 2

        # 1. 가운데 값이 정답인 경우
        if arr[mid] == target:
            return mid
        
        # 2. 가운데 값이 정답보다 작은 경우
        elif arr[mid] < target:
            low = mid + 1
            
        # 3. 가운데 값이 정답보다 큰 경우
        else:
            high = mid - 1
            
    return -1


arr = [2, 4, 7, 9, 11, 19, 23]
arr.sort()
N = 9
print(f'{N} => {binarySearch(N)}')
```

```python
# 이진 검색 - 재귀 호출 활용

# 함수를 호출할 때마다 low. high 변수가 바뀌어서 사용됨
def binarySearch(low, high, target):
    # 기저 조건 : 언제까지 재귀 호출을 반복할 것인가?
    # low > high 라면 데이터를 못 찾음
    if low > high:
        return -1

    mid = (low + high) // 2

    # 1. 가운데 값이 정답인 경우
    if target == arr[mid]:
        return mid

    # 2. 가운데 값이 정답보다 작은 경우
    elif arr[mid] < target:
        return binarySearch(mid + 1, high, target)

    # 3. 가운데 값이 정답보다 큰 경우
    elif arr[mid] > target:
        return binarySearch(low, mid - 1, target)


arr = [2, 4, 7, 9, 11, 19, 23]
arr.sort()

N = 9
print(f'{N} => {binarySearch(0, len(arr)-1, N)}')
```



