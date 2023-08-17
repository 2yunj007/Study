# 큐 (Queue)

- 스택과 마찬가지로 삽입과 삭제의 위치가 제한적인 자료구조
- 큐의 뒤에서는 삽입만 하고, 큐의 앞에서는 삭제만 이루어지는 구조
- **선입선출(FIFO, First-In-First-Out)** : 큐에 삽입한 순서대로 원소가 저장되어, 가장 먼저 삽입된 원소는 가장 먼저 삭제됨
- **Front** : 저장된 원소 중 첫 번째 원소 (또는 삭제된 위치)
- **Rear** : 저장된 원소 중 마지막 원소

<img src="https://blog.kakaocdn.net/dn/5NOv1/btqSTINnoq8/4f8bjzzf6W4POewlq8At31/img.png" alt="img" style="zoom: 50%;" />



## 선형 큐

- 1차원 배열을 이용한 큐
- 큐의 크기 = 배열의 크기
- 초기 상태 : front = rear = -1
- 공백 상태 : front == rear
- 포화 상태 : rear == n-1 (n : 배열의 크기, n-1 : 배열의 마지막 인덱스)



### 선형 큐 연산 과정

- 공백 큐 생성 : **createQueue()**

<img src="https://velog.velcdn.com/images%2Fsuitepotato%2Fpost%2F0f51ac3c-3647-447f-a301-a659c9477d16%2Fqueue_calc_01.svg" alt="img" style="zoom: 67%;" />



- 원소 A, B 삽입 : **enQueue(A), enQueue(B)**
  - 큐의 뒤쪽(rear 다음)에 원소를 삽입하는 연산

<img src="https://velog.velcdn.com/images%2Fsuitepotato%2Fpost%2F6e281fb1-4b9f-4ce4-a6a9-c9cca1584f40%2Fqueue_calc_02.svg" alt="img" style="zoom: 67%;" />

<img src="https://velog.velcdn.com/images%2Fsuitepotato%2Fpost%2F16913c0e-3b0a-48f1-8d20-cd741be0f6f6%2Fqueue_calc_03.svg" alt="img" style="zoom: 67%;" />

```python
def enQueue(data):
    global rear
    if rear == Qsize-1:	# 가득 차면
        print('Q is Full')
    else:
        rear += 1
        Q[rear] = data
```



- 원소 반환/삭제 : **deQueue()**
  - 큐의 앞쪽(front)에서 원소를  삭제하고 반환하는 연산

<img src="https://velog.velcdn.com/images%2Fsuitepotato%2Fpost%2F9e5f098a-b7f8-471e-81ed-ff886643c18b%2Fqueue_calc_04.svg" alt="img" style="zoom: 67%;" />

```python
def deQueue():
    global front
    if front == rear:
        print('Q is Empty')
else:
    front += 1:
    return Q[front]
```



- 공백 상태 및 포화 상태 검사 : **isEmpty(), isFull()**

```python
def isEmpty():
    return fornt == rear

def isFull():
    return rear == len(Q) - 1
```



- 검색 : **Qpeek()**
  - 큐의 앞쪽(front)에서 원소를 삭제 없이 반환하는 연산

```python
def Qpeek():
    if fornt == rear:
        print('Q is Empty')
    else:
        return Q[front+1]
```



### 선형 큐 이용 시의 문제점

**잘못된 포화 상태 인식**

- 선형 큐를 이용하여 원소의 삽입과 삭제를 계속할 경우 배열의 앞부분에 활용할 수 있는 공간이 있음에도 불구하고, rear=n-1인 상태 즉, 포화 상태로 인식하여 더 이상의 삽입을 수행하지 않게 됨

**해결 방법 1**

- 매 연산이 이루어질 때마다 저장된 원소들을 배열의 앞부분으로 모두 이동시킴
- 원소 이동에 많은 시간이 소요되어 큐의 효율성이 급격히 떨어짐

**해결 방법 2**

- 1차원 배열을 사용하되, 논리적으로 배열의 처음과 끝이 연결되어 원형의 큐를 이룬다고 가정하고 사용



## 원형 큐

- front와 rear의 위치가 배열의 마지막 인덱스인 n-1를 가리킨 후, 그 다음에는 논리적 순환을 이루어 배열의 처음 인덱스인 0으로 이동해야 함
- 이를 위해 나머지 연산자 mod를 사용함

- 공백 상태와 포화 상태 구분을 쉽게 하기 위해 front가 있는 자리는 사용하지 않고 항상 빈자리로 둠
- 초기 상태 : front = rear = 0
- 공백 상태 : front == rear
- 포화 상태 : 삽입할 rear의 다음 위치 == 현재 front
  - (rear + 1) mod n == front

|         | 삽입 위치               | 삭제 위치                 |
| ------- | ----------------------- | ------------------------- |
| 선형 큐 | rear = rear + 1         | front = front + 1         |
| 원형 큐 | rear = (rear + 1) mod n | front = (front + 1) mod n |



### 원형 큐 연산 과정

- 공백 큐 생성 : **createQueue()**
  -  front는 항상 비워 두므로 크기는 3이지만 실제 배열에서는 크기가 4인 배열 생성

<img src="https://velog.velcdn.com/images%2Fsuitepotato%2Fpost%2F86424bc1-be48-421e-a07c-238d303fdae1%2Fcircle_queue_calc_01.svg" alt="img" style="zoom: 67%;" />



- 원소 A, B 삽입 : **enQueue(A), enQueue(B)**
  - rear 값을 조정하여 새로운 원소를 삽입한 자리를 마련함
    - rear <- (rear + 1) mod n;
  - 그 인덱스에 해당하는 배열원소 cQ[rear]에 item을 저장

<img src="https://velog.velcdn.com/images%2Fsuitepotato%2Fpost%2F97e7a160-1915-4678-bbf3-f1cb7b2ccfa5%2Fcircle_queue_calc_02.svg" alt="img" style="zoom:67%;" />

<img src="https://velog.velcdn.com/images%2Fsuitepotato%2Fpost%2F3741a4dd-9db3-43f2-9a96-09b775fd46c5%2Fcircle_queue_calc_03.svg" alt="img" style="zoom:67%;" />

```python
def enQueue(item):
    global rear
    if is Full():
        print('Queue is Full')
    else:
        rear = (rear + 1) % len(cQ)
        cQ[rear] = item
```



- 원소 반환/삭제 : **deQueue()**
  - front 값을 조정하여 삭제할 자리를 준비
  - 새로운 front 원소를 리턴 함으로써 삭제와 동일한 기능함

<img src="https://velog.velcdn.com/images%2Fsuitepotato%2Fpost%2Fa88f0f74-2627-4947-b74b-3afb66087813%2Fcircle_queue_calc_04.svg" alt="img" style="zoom:67%;" />

```python
def deQueue():
    global front
    if is Empty():
        print('Queue is Empty')
    else:
        front = (front + 1) % len(cQ)
        return cQ[front]
```



- 공백 상태 및 포화 상태 검사 : **isEmpty(), isFull()**

```python
def isEmpty():
    return fornt == rear

def isFull():
    return (rear+1) % len(cQ) == front
```



## 우선순위 큐 (Priority Queue)

- 우선순위를 가진 항목들을 저장하는 큐
- FIFO 순서가 아니라 우선순위가 높은 순서대로 먼저 나가게 됨
- 적용 분야 : 시뮬레이션 시스템, 네트워크 트래픽 제어, 운영체제의 테스크 스케줄링 등

- 배열이나 리스트를  이용하여 구현

- 배열을 이용한 우선순위 큐
  - 원소를 삽입하는 과정에서 우선순위를 비교하여 적절한 위치에 삽입하는 구조
  - 가장 앞에 최고 우선순위의 원소가 위치하게 됨
  - 그러나 삽입/삭제 연산이 일어날 때 원소의 재배치로 인해 시간/메모리 낭비 발생



## 버퍼 (Buffer) 

- 데이터를 한 곳에서 다른 한 곳으로 전송하는 동안 일시적으로 그 데이터를 보관하는 메모리의 영역
- 버퍼링 : 버퍼를 활용하는 방식 또는 버퍼를 채우는 동작을 의미
- 버퍼는 일반적으로 입출력 및 네트워크와 관련된 기능에서 이용
- 순서대로 입력/출력/전달이 되어야 하므로 FIFO 방식인 큐가 활용됨
