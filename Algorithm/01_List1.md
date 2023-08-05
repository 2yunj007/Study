# 버블 정렬 (Bubble Sort) 

: 인접한 두 개의 원소를 비교하며 자리를 계속 교환하는 방식

### 정렬 과정

1. 첫 번째 원소부터 인접한 원소끼리 계속 자리를 교환하면서 맨 마지막 자리까지 이동
2. 한 단계가 끝나면 가장 큰 원소가 마지막 자리로 정렬
3. 교환하며 자리를 이동하는 모습이 물 위에 올라오는 거품 모양과 같다고 하여 버블 정렬이라고 함

![img](https://github.com/GimunLee/tech-refrigerator/raw/master/Algorithm/resources/bubble-sort-001.gif)



### 시간 복잡도

**O(n^2)**



### 배열을 활용한 버블 정렬

```python
BubbleSort(a, N)				# 정렬할 배열과 배열의 크기
	for i : N-1 -> 1			# 정렬될 구간의 끝
		for j : 0 -> i-1		# 비교할 원소 중 왼쪽 원소의 인덱스
			if a[j] > a[j+1] 	# 왼쪽 원소가 더 크면
			a[j] <-> a[j+1]		# 오른쪽 원소와 교환
```

```python
def BubbleSort(a, N):			# 정렬할 List, N 원소 수
	for i in range(N-1, 0, -1):	# 범위의 끝 위치
    	for j in range(0, i):
            if a[j] > a[j+1]:
            	a[j], a[j+1] = a[j+1], a[j]
```



# 카운팅 정렬 (Countiong Sort)

: 항목들의 순서를 결정하기 위해 집합에 각 항목이 몇 개씩 있는지 세는 작업을 하여, 선형 시간에 정렬하는 효율적인 알고리즘



### 제한 사항

- 정수나 정수로 표현할 수 있는 자료에 대해서만 적용 가능
- 각 항목의 발생 횟수를 기록하기 위해, 정수 항목으로 인덱스 되는 카운트들의 배열을 사용하기 때문
- 카운트들을 위한 충분한 공간을 할당하려면 집합 내의 가장 큰 정수를 알아야 함



### 정렬 과정

1. 입력 배열의 최댓값, Counting Array 생성

   - 원소의 누적합을 구하기 위한 Counting Array 생성을 위해 배열의 최댓값이 필요
   - 이후 최댓값 + 1 크기의 Counting Array를 생성하여 입력 배열의 값을 기준으로 조회된 좌표에 입력 배열의 각 원소 개수를 count 함

2. 입력 배열 원소 개수의 누적합

   - 1번 과정에서 Counting Array 생성 및 원소 Count를 마쳤다면 이전 좌표의 원소 개수를 더해 나가 누적합 배열로 만들어 줌

3. 입력 배열과 누적합 배열을 이용한 정렬 수행

   - 입력 배열의 각 원소에 대해 Counting Array에 조회하여 어느 좌표에 들어가야 하는지 체크한 뒤 조회된 원소의 개수를 1 감소시켜 앞의 좌표로 입력받을 수 있게 함

![countiong sort](https://blog.kakaocdn.net/dn/QiWZZ/btq89vkmDh7/40myVsVLfxYVPs9fKtu7s0/img.png)

```python
def Counting_Sort(A, B, k)
# A []: 입력 배열 (0 to k)
# B []: 정렬된 배열
# C []: 카운트 배열

	C = [0] * (k+1)
    
    for i in range(0, len(A)):
        C[A[i]] += 1
    
    for i in range(1, len(C)):
        C[i] += C[i-1]
    
    for i in range(len(B)-1, -1, -1):
        C[A[i]] -= 1
        B[C[A[i]]] = A[i]
```



### 시간 복잡도

**O(n + k)**

n: 리스트 길이, k: 정수의 최댓값
