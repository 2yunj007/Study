# 트리

- 비선형 구조
- 원소들 간에 1:n 관계를 가지는 자료구조
- 원소들 간에 계층 관계를 가지는 계층형 자료구조
- 상위 원소에서 하위 원소로 내려가며 확장되는 트리(나무) 모양의 구조



## 정의

- 한 개 이상의 노드로 이루어진 유한 집합이며 다음 조건을 만족
  - 노드 중 최상위 노드를 **루트(root)**라고 함
  - 나머지 노드들은 n(>=0)개의 분리 집합 T1,...,TN으로 분리 가능
  - 이들 T1,...,TN은 각각 하나의 트리가 되며 (재귀적 정의) 루트의 **부 트리(subtree)**라 함

![img](https://blog.kakaocdn.net/dn/M2NhQ/btq10SfYUFc/3q760ablTaC0HF5iTEQ8KK/img.png)

## 용어 정리

- **노드(node)** : 트리의 원소
- **간선(edge)** : 노드를 연결하는 선. 부모 노드와 자식 노드를 연결
- **루트 노드(root node)** : 트리의 시작 노드
- **형제 노드(sibling node)** : 같은 부모 노드의 자식 노드들

- **조상 노드** : 간선을 따라 루트 노드까지 이르는 경로에 있는 모든 노드들
- **서브 트리(subtree)** : 부모 노드와 연결된 간선을 끊었을 때 생성되는 트리
- **자손 노드** : 서브 트리에 있는 하위 레벨의 노드들

- **차수(degree)**
  - 노드의 차수 : 노드에 연결된 자식 노드의 수 (B의 차수 = 2, C의 차수 = 1)
  - 트리의 차수 : 트리에 있는 노드의 차수 중에서 가장 큰 값 (트리 T의 차수 = 3)
  - 단말 노드(리프 노드) : 차수가 0인 노드. 자식 노드가 없는 노드

- **높이**
  - 노드의 높이 : 루트에서 노드에 이르는 간선의 수. 노드의 레벨
  - 트리의 높이 : 트리에 있는 노드의 높이 중에서 가장 큰 값. 최대 레벨

![img](https://blog.kakaocdn.net/dn/bNYLuw/btq16iLacXK/d4qRxkXVcpOt8i5U0UntmK/img.png)



## 그래프 vs. 트리

|                    | 그래프                            | 트리                                            |
| :----------------- | :-------------------------------- | :---------------------------------------------- |
| **정의**           | 노드와 간선으로 구성되는 자료구조 | 그래프의 한 종류로, 방향성이 있는 비순환 그래프 |
| **방향성**         | 방향 그래프 O, 무방향 그래프 O    | 방향 그래프 O, 무방향 그래프 X                  |
| **루트 노드**      | X                                 | O                                               |
| **부모/자식 관계** | X                                 | O                                               |



# 이진 트리

- 모든 노드들이 2 개의 서브 트리를 갖는 특별한 형태의 트리
- 각 노드가 자식 노드를 최대한 2 개까지만 가질 수 있는 트리
  - 왼쪽 자식 노드(left child node)
  - 오른쪽 자식 노드(right child node)

- 이진 트리의 예

![img](https://blog.kakaocdn.net/dn/bmEzxA/btq103PhIiy/0Xy3K3YICPUMe2NgPfSr30/img.png)

## 특성

- 레벨 i에서 노드의 최대 개수는 2^i개
- 높이가 h인 이진 트리가 가질 수 있는 노드의 최소 개수는 h+1 개
- 최대 개수는 2^(h+1)-1 개



## 종류

### 포화 이진 트리(Full Binary Tree)

- 모든 레벨에 노드가 포화 상태로 차 있는 이진 트리
- 높이가 h일 때, 최대의 노드 개수인 2^(h+1)-1의 노드를 가진 이진 트리
- 루트를 1 번으로 하여 2^(h+1)-1 까지 정해진 위치에 대한 노드 번호를 가짐

![img](https://blog.kakaocdn.net/dn/PdtZY/btq12l9I8J8/09gfh8ZrrD5jvCEuqkIUqK/img.png)



### 완전 이진 트리(Complete Binary Tree)

- 높이가 h이고 노드 수가 n개일 때 (단, 2^h <= n <=  2^(h+1)-1), 포화 이진 트리의 노드 번호 1번부터 n번까지 빈 자리가 없는 이진 트리
- ex) 노드가 10개인 완전 이진 트리

![img](https://blog.kakaocdn.net/dn/9nkCv/btq10ROWdJ9/rLgWDfvMc7kF8XIXYmpuc1/img.png)



### 편향 이진 트리(Skewed Binary Tree)

- 높이 h에 대해 최소 개수의 노드를 가지면서 한쪽 방향의 자식 노드만을 가진 이진 트리
  - 왼쪽 편향 이진 트리
  - 오른쪽 편향 이진 트리

![img](https://blog.kakaocdn.net/dn/cujNp2/btq16ij6xTI/s0p5i06lEAIpD4yGFqq0vk/img.png)



## 순회(traversal)

- 트리의 각 노드를 중복되지 않게 전부 방문하는 것을 의미
- 트리는 비선형 구조이기 때문에 선형 구조에서와 같이 선후 연결 관계를 알 수 없음
- 따라서 특별한 접근 방법이 필요
  - 전위순회(preorder traversal) : VLR
  - 중위순회(inorder traversal) : LVR
  - 후위순회(postorder traversal) : LRV

![img](https://blog.kakaocdn.net/dn/db8seM/btq11MsOolO/kD0Ks0vy5Nu1llpaA8uhH1/img.png)



### 전위순회(preorder traversal)

#### 수행 방법

1. 현재 노드 n을 방문하여 처리 -> V
2. 현재 노드 n의 왼쪽 서브 트리로 이동 -> L
3. 현재 노드 n의 오른쪽 서브 트리로 이동 -> R

![img](https://blog.kakaocdn.net/dn/pX28w/btq12v5q3R0/6KKyZi7a938QqpBgwVWnn1/img.png)



#### 전위 순회 알고리즘

```python
def preorder_traversal(T):
    if T:	# T is not None
        visit(T)	# print(T.item)
        preorder_traversal(T.left)
        preorder_traversal(T.right)
```



### 중위 순회(inorder traversal)

#### 수행 방법

1. 현재 노드 n의 왼쪽 서브 트리로 이동 -> L
2. 현재 노드 n을 방문하여 처리 -> V
3. 현재 노드 n의 오른쪽 서브 트리로 이동 -> R

![img](https://blog.kakaocdn.net/dn/DXMlk/btq150jyQhK/98J5IfHULTzkdwQ2g9OVY1/img.png)



#### 중위 순회 알고리즘

```python
def inorder_traversal(T):
    if T:
        inorder_traversal(T.left)
        visit(T)
        inorder_traversal(T.right)
```



### 후위 순회(postorder traversal)

#### 수행 방법

1. 현재 노드 n의 왼쪽 서브 트리로 이동 -> L
2. 현재 노드 n의 오른쪽 서브 트리로 이동 -> R
3. 현재 노드 n을 방문하여 처리 -> V

![img](https://blog.kakaocdn.net/dn/b3rRr5/btq1WOr7GRP/5kvk2kaw1yyJnji7Hp9yHK/img.png)



#### 후위 순회 알고리즘

```python
def postorder_traversal(T):
    if T:
        postorder_traversal(T.left)
        postorder_traversal(T.right)
        visit(T)
```



## 이진 트리의 표현

### 배열

- 이진 트리에 각 노드 번호를 다음과 같이 부여
- 루트의 번호를 1로 함
- 레벨 n에 있는 노드에 대하여 왼쪽부터 오른쪽으로 2^n부터 2^(n+1)-1까지 번호를 차례로 부여

![img](https://blog.kakaocdn.net/dn/cMrZjo/btq15ZybGcn/Ybfs6rIGoTfRco3pJyvw3k/img.png)



#### 노드 번호를 배열의 인덱스로 사용

- 노드 번호가 **i 인 노드의 부모 노드 번호** : floor(i / 2) (i / 2와 같거나 그보다 작은 정수 중 가장 큰 값)
- 노드 번호가 **i 인 노드의 왼쪽 자식 노드 번호** : 2*i
- 노드 번호가 **i 인 노드의 오른쪽 자식 노드 번호** : 2*i+1
- 레벨 n의 노드 번호 시작 번호 : 2^n노드 번호의 성질

![img](https://blog.kakaocdn.net/dn/IJ4Mj/btq15ZybXzK/HA4KikQRrruJhjW8rHF0z1/img.png)



#### 높이가 h인 이진 트리를 위한 배열 크기

- 레벨 i의 최대 노드 수 : 2^i
- 따라서 1 + 2 + 4 + 8 + ... + 2^i = Σ2^i = 2^(h+1) - 1



![img](https://blog.kakaocdn.net/dn/bbxQhm/btq11LAJH7W/pXRU4B6Nou8VWtg0B5PhO1/img.png)



#### 배열을 이용한 이진 트리 표현의 단점

-  편향 이진 트리의 경우 메모리 공간 낭비 발생
- 트리의 중간에 새로운 노드를 삽입하거나 기존 노드를 삭제할 경우 배열의 크기 변경이 어려워 비효율적



### 연습 문제

```python
# 루트가 1인 경우
'''
V
부모 자식 ...
13
1 2 1 3 2 4 3 5 3 6 4 7 5 8 5 9 6 10 6 11 7 12 11 13
'''
def preorder(n):
    if n:   # 존재하는 정점이면
        print(n, end=' ')    # visit(n)
        preorder(ch1[n])     # 왼쪽 서브 트리로 이동
        preorder(ch2[n])     # 오른쪽 서브 트리로 이동


V = int(input())    # 정점 수 = 마지막 정점 번호
E = V - 1   # tree의 간선 수 = 정점 수 - 1
arr = list(map(int, input().split()))
# 부모를 인덱스로 자식을 저장
ch1 = [0] * (V+1)
ch2 = [0] * (V+1)
for i in range(E):
    p, c = arr[i*2], arr[i*2+1]
    if ch1[p] == 0: # 자식1이 아직 없으면
        ch1[p] = c
    else:
        ch2[p] = c

preorder(1)     # 1 2 4 7 12 3 5 8 9 6 10 11 13 
```

```python
# 루트가 정해지지 않았을 때 (루트부터 찾음)
'''
V
부모 자식 ...
5
3 1 3 2 2 5 2 4
'''
def preorder(n):
    if n:   # 존재하는 정점이면
        print(n, end=' ')    # visit(n)
        preorder(ch1[n])     # 왼쪽 서브 트리로 이동
        preorder(ch2[n])     # 오른쪽 서브 트리로 이동


V = int(input())    # 정점 수 = 마지막 정점 번호
E = V - 1   # tree의 간선 수 = 정점 수 - 1
arr = list(map(int, input().split()))
# 부모를 인덱스로 자식을 저장
# 자식을 인덱스로 부모를 저장
ch1 = [0] * (V+1)
ch2 = [0] * (V+1)
par = [0] * (V+1)
for i in range(E):
    p, c = arr[i*2], arr[i*2+1]
    if ch1[p] == 0:  # 자식1이 아직 없으면
        ch1[p] = c
    else:
        ch2[p] = c
    par[c] = p       # 자식을 인덱스로 부모 저장

# 실제 루트 찾기
root = 1
while par[root] != 0:
    root += 1

preorder(root)     # 3 1 2 5 4 
```



## 수식 트리

- 수식을 표현하는 이진 트리
- 수식 이진 트리(Expression Binary Tree)라고 부르기도 함
- 연산자는 루트 노드이거나 가지 노드
- 피연산자는 모두 잎 노드



### 수식 트리의 순회

- 중위 순회 : 6 + 2 * 2 - 1 (식의 중위 표기법)
- 후위 순회 : 6 2 2 * + 1 - (식의 후위 표기법)
- 전위 순회 : \- + 6 * 2 2 1 (식의 전위 표기법)

![img](https://blog.kakaocdn.net/dn/yCf8T/btqxbBCJ15a/6RqCzR9Qw1KYmauTXF23d1/img.png)



# 이진 탐색 트리

- 탐색 작업을 효율적으로 하기 위한 자료 구조
- 모든 원소는 서로 다른 유일한 키를 가짐
- key(왼쪽 서브 트리) < key(루트 노드) < key(오른쪽 서브 트리)
- 왼쪽 서브 트리와 오른쪽 서브 트리도 이진 탐색 트리임
- 중위 순회하면 오름차순으로 정렬된 값을 얻을 수 있음

![img](https://blog.kakaocdn.net/dn/lv0I0/btq1XLozR8D/CVxDQJhPKg8Sut5oq2UwB0/img.png)



## 탐색 연산

1. 루트에서 시작

2. 탐색할 키 값 x를 루트 노드의 키 값과 비교

   - `x = 루트노드의 키 값`인 경우 : 원하는 원소를 찾았으므로 탐색 연산 성공

   - `x < 루트노드의 키 값`인 경우 : 루트 노드의 왼쪽 서브 트리에 대해 탐색 연산 수행
   - `x > 루트노드의 키 값`인 경우 : 루트 노드의 오른쪽 서브 트리에 대해 탐색 연산 수행

3. 서브 트리에 대해서 순환적으로 탐색 연산 반복

- ex) 13 탐색

![img](https://blog.kakaocdn.net/dn/bSYYA5/btq1Xk5qlrJ/29VHIfNvUcP7HDxu9XM5O0/img.png)



## 삽입 연산

1. 탐색 연산을 수행

   - 삽입할 원소와 같은 원소가 있다면 삽입 불가능

   - 탐색에 실패할 경우 마지막까지 탐색한 위치가 삽입 위치

2. 탐색 실패한 위치에 원소를 삽입

- ex) 5 삽입



![img](https://blog.kakaocdn.net/dn/laXaH/btq1YkEgtKn/VqFKF6vjpOXQvS72mHKxUk/img.png)



## 삭제 연산

1. 삭제할 노드가 단말 노드인 경우
   - 부모 노드가 삭제할 노드를 가리키지 않도록 함
2. 하나의 서브 트리만 가지고 있는 경우
   - 삭제할 노드의 부모 노드가 삭제할 노드의 자식 노드를 가리키도록 함

3. 두 개의 서브 트리를 가지고 있는 경우
   - 삭제할 노드의 오른쪽 자식 중 가장 작은 값을 부모 노드가 가리키도록 함
   - 또는, 삭제할 노드의 왼쪽 자식 중 가장 큰 값을 부모 노드가 가리키도록 함



## 성능

- 탐색, 삽입, 삭제 시간은 트리의 높이 만큼 시간 소요
  - O(h), h : BST의 깊이
- 평균의 경우
  - 이진 트리가 균형적으로 생성되어 있는 경우
  - O(log n)
- 최악의 경우
  - 한쪽으로 치우친 경사 이진 트리 경우
  - O(n)
  - 순차 탐색과 시간 복잡도 동일



### 검색 알고리즘 비교

- 배열에서의 순차 검색 : O(N)
- 정렬된 배열에서의 순차 검색 : O(N)
- 정렬된 배열에서의 이진 탐색 : O(logN)
  - 고정 배열 크기와 삽입, 삭제 시 추가 연산 필요
- 이진 탐색 트리에서의 평균 : O(logN)
  - 최악의 경우 : O(N)
  - 완전 이진 트리 또는 균형 트리로 바꿀 수 있다면 최악의 경우 방지
    - 새로운 원소를 삽입할 때 소요 시간 단축
    - 평균과 최악의 시간이 동일 O(logN)
- 해쉬 검색 : O(1)
  - 추가 저장 공간 필요



# 힙(heap)

- 완전 이진 트리에 있는 노드 중에서 키 값이 가장 큰 노드나 가장 작은 노드를 찾기 위해서 만든 자료구조

- **최대 힙(max heap)**
  - 키값이 가장 큰 노드를 찾기 위한 완전 이진 트리
  - 부모 노드 키 값 > 자식 노드 키 값
  - 루트 노드 : 키 값이 가장 큰 노드

- **최소 힙(min heap)**
  - 키 값이 가장 작은 노드를 찾기 위한 완전 이진 트리
  - 부모 노드 키 값 < 자식 노드 키 값
  - 루트 노드 : 키 값이 가장 작은 노드

![img](https://blog.kakaocdn.net/dn/Kmz95/btq106ZoYH1/S3BTXMWRd1gAuQk9RzcGj0/img.png)

 

## 힙의 연산

### 삽입

1. 삽입할 자리 확장 (완전 이진 트리 규칙에 따라서 마지막 자리 확장)
2. 확장한 자리에 삽입할 원소 저장
3. 부모 노드와 크기를 비교하여 자리 바꾸기를 반복
4. 바꿀 필요가 없다면 자리 확정

 

### 삭제

1. 루트 원소 삭제
   - 힙에서는 루트 노드의 원소만 삭제 가능
2. 마지막 노드를 루트 노드로 이동
3. 자식 노드와 루트 노드의 크기 비교하여 자리 바꾸기를 반복
4. 바꿀 필요가 없다면 자리 확정 (자식이 없거나 부모가 크면 비교 중지)

```python
def deq():
    global last
    tmp = heap[1]           # 루트 백업
    heap[1] = heap[last]    # 삭제할 노드의 키를 루트에 복사
    last -= 1               # 마지막 노드 삭제
    p = 1   # 루트에 옮긴 값을 자식과 비교
    c = p * 2   # 왼쪽 자식 (비교할 자식 노드 번호)
    while c <= last:    # 자식이 하나라도 있으면
        if c + 1 <= last and heap[c] < heap[c + 1]:  # 오른쪽 자식도 있고, 오른쪽 자식이 더 크면
            c += 1  # 비교 대상이 오른쪽 자식 노드
        if heap[p] < heap[c]:   # 자식이 더 크면 최대힙 규칙에 어긋나므로
            heap[p], heap[c] = heap[c], heap[p]
            p = c   # 자식을 새로운 부모로
            c = p * 2   # 왼쪽 자식 번호를 계산
        else:   # 부모가 더 크면
            break
    return tmp  # 비교 중단

heap = [0] * 100
last = 0
```

