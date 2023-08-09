# 스택 (Stack)

## 스택의 특성

- 물건을 쌓아 올리듯 자료를 쌓아 올린 형태의 자료구조
- 스택에 저장된 자료는 **선형 구조**를 가짐
  - 선형 구조: 자료 간의 관계가 1대1의 관계를 가짐
  - 비선형 구조: 자료 간의 관계가 1대N 관계를 가짐 (ex. 트리)
- 스택에 자료를 삽입하거나 스택에서 자료를 꺼낼 수 있음
- **후입선출(LIFO, Last-In-First-Out)**: 마지막에 삽입한 자료를 가장 먼저 꺼냄

<img src="https://blog.kakaocdn.net/dn/bcgR9A/btqSX70PCTe/dMSMQoJcZhDpq4sRRpu3A0/img.png" alt="img" style="zoom: 33%;" />

## 자료구조

- 배열 사용 가능
- 저장소 자체를 스택이라 부르기도 함
- 스택에서 마지막 삽입된 원소의 위치를 top이라고 함



## 연산

- 삽입: 저장소에 자료를 저장 (push)
- 삭제: 저장소에서 자료를 꺼냄 (pop)

**.isEmpty**

-  스택이 공백인지 아닌지를 확인하는 연산

**.peek**

- 스택의 top에 있는 item(원소)을 반환하는 연산



## 스택의 push 알고리즘

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



## 스택의 pop 알고리즘

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



## 스택 구현 고려 사항

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

### Function call

- 프로그램의 함수 호출과 복귀에 따른 수행 순서를 관리

- 가장 마지막에 호출된 함수가 가장 먼저 실행을 완료하고 복귀하는 후입선출 구조이므로, 후입선출 구조의 스택을 이용하여 수행 순서 관리
- 함수 호출이 발생하면 호출한 함수 수행에 필요한 지역변수, 매개변수 및 수행 후 복귀할 주소 등의 정보를 스택 프레임(stack frame)에 저장하여 시스템 스택에 삽입
- 함수의 실행이 끝나면 시스템 스택의 top 원소(stack frame)를 삭제(pop)하면서 프레임에 저장되어 있던 복귀 주소를 확인하고 복귀
- 함수 호출과 복귀에 따라 이 과정을 반복하여 전체 프로그램 수행이 종료되면 시스템 스택은 공백 스택이 됨



### 함수 호출과 복귀에 따른 전체 프로그램 수행 순서

<img src="https://blog.kakaocdn.net/dn/ckZG8J/btqYgGK4Uug/4FoX6v2805UY4BUvkpmMi1/img.png" alt="img" style="zoom: 80%;" />



# 재귀 호출

- 자기 자신을 호출하여 순환 수행되는 것
- 함수에서 실행해야 하는 작업의 특성에 따라 일반적인 호출 방식보다 재귀 호출 방식을 사용하여 함수를 만들면 프로그램의 크기를 줄이고 간단하게 작성
- 재귀 호출의 예) factorial



### 피보나치 수열

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



# Memoization

- 메모이제이션(memoization): 컴퓨터 프로그램을 실행할 때 이전에 계산한 값을 메모리에 저장해서 매번 다시 계산하지 않도록 하여 전체적인 실행 속도를 빠르게 하는 기술
- 글자 그대로 해석하면 '메모리에 넣기'라는 의미
- 앞의 예에서 피보나치 수를 구하는 알고리즘에서 fibo(n)의 값을 계산하자마자 저장(memoization)하면 실행시간을 Ω(n)으로 줄일 수 있음

```python
# memo를 위한 배열을 할당하고, 모두 0으로 초기화
# memo[0]을 0으로 memo[1]는 1로 초기화

def fibo1(n):
    global memo
    if n >= 2 and memo[n] == 0:
        memo[n] = (fibo1(n-1) + fibo1(n-2))
    return memo[n]

memo = [0] * (n+1)
memo[0] = 0
memo[1] = 1
```