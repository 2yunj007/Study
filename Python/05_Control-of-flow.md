# 제어문 (Control Statement) 

: 코드의 실행 흐름을 제어하는 데 사용되는 구문

- 조건에 따라 코드 블록을 실행하거나 반복적으로 코드를 실행



# 조건문 (Conditional Statement)

: 주어진 조건식을 평가하여 해당 조건이 참(True)인 경우에만 코드 블록을 실행하거나 건너뜀

- 조건식을 동시에 검사하는 것이 아니라 순차적으로 비교
- 중첩 가능



## if statement

```python
if 표현식:
    코드 블록
elif 표현식:
    코드 블록
else 표현식:
    코드 블록
```

```python
num = int(input("숫자를 입력하세요: "))

# if statement
# num이 홀수라면 (2로 나눈 나머지가 1이라면)
if num % 2 == 1:	# if num % 2: (1이면 True이기 때문에 같은 동작 가능)
    print('홀수입니다.')
# num이 홀수가 아니라면 (짝수면)
else:
    print('짝수입니다')
```



# 반복문 (Loop Statement)

: 주어진 코드 블록을 여러 번 반복해서 실행하는 구문

- for : iterable의 요소를 하나씩 순회하며 반복
- while : 주어진 조건식이 참인 동안 반복



## 'for' statement

: 임의의 시퀀스 항목들을 그 시퀀스에 들어 있는 순서대로 반복

- 길이가 정해져 있기 때문에 종료 조건이 필요 없음
- 반복 가능한 객체 (iterable): 반복문에서 순회할 수 있는 객체 (시퀀스 객체뿐만 아니라 dict, set 등 포함)

```python
for 변수 in 반복 가능한 객체:
    코드 블록
```

### for 문 원리

- 리스트 내 첫 항목이 반복 변수에 할당되고 코드 블록이 실행
- 다음으로 반복 변수에 리스트의 2번째 항목이 할당되고 코드 블록이 다시 실행
- 마지막으로  반복 변수에 리스트의 마지막 요소가 할당되고 코드 블록이 실행

```python
# 리스트 순회
items = ['apple', 'banana', 'coconut']

for item in items:
    print(item)
    
"""
apple
banana
coconut
"""
```

```python
# 문자열 순회
country = 'Korea'
for char in country:
    print(char)

"""
K
o
r
e
a
"""
```

```python
# range 순회
for i in range(5):
    print(i)

"""
0
1
2
3
4
"""
```

### 인덱스로 리스트 순회

- 리스트의 요소가 아닌 인덱스로 접근하여 해당 요소들을 변경하기
- range(5)를 사용하면 리스트의 길이가 변했을 때 대응할 수 없기 때문에 사용

```python
numbers = [4, 6, 10 , -8, 5]

for i in range(len(numbers)):
    numbers[i] = number[i] * 2
    
print(numbers)	# [8, 12, 20, 16, 5]
```

### 중첩된 반복문

```python
outers = ['A', 'B']
inners = ['c', 'd']

for outer in outers:
    for inner in inners:
        print(outer, inner)

"""
A c
A d
B c
B d
"""
```

### 중첩 리스트 순회

- 안쪽 리스트 요소에 접근하려면 바깥 리스트를 순회하면서 중첩 반복문을 사용해 각 안쪽 반복을 순회

```python
elements = [['A', 'B'], ['c', 'd']]

for elem in elements:
    for item in elem:
        print(item)
        
"""
A
B
c
d
"""
```



## 'while' statement 

: 주어진 조건식이 참(True)인 동안만 코드를 반복해서 실행, == 조건식이 거짓(False)가 될 때까지 반복

- 반드시 종료 조건이 필요

```python
while 조건식:
    코드 블록
```

### 사용자의 입력에 따른 반복

- while문을 사용한 특정 입력 값에 대한 종료 조건 활용하기

```python
number = int(input('양의 정수를 입력해주세요: '))

while number <= 0:
    if nuber < 0:
        print('음의 정수를 입력했습니다.')
    else:
        print('0은 양의 정수가 아닙니다.')
        
    number = int(input('양의 정수를 입력해주세요: '))    

print('잘했습니다!')
```



## 적정한 반복문 활용하기

### for

- 반복 횟수가 명확하게 정해져 있는 경우에 유용
- 예를 들어 리스트, 튜플, 문자열 등과 같은 시퀀스 형식의 데이터를 처리할 때

### while

- 반복 횟수가 불명확하거나 조건에 따라 반복을 종료해야 할 때 사용
- 예를 들어 사용자의 입력을 받아서 특정 조건이 충족될 때까지 반복하는 경우



# 반복 제어

: for문과 while은 매 반복마다 본문 내 모든 코드를 실행하지만 때때로 일부만 실행하는 것이 필요할 때가 있음 



## break

: 반복을 즉시 중지

```python
# 프로그램 종료 조건 만들기
number = int(input('양의 정수를 입력해주세요: '))

while number <= 0:
    if number == -9999:
        print('프로그램을 종료합니다.')
        break
        
    if nuber < 0:
        print('음의 정수를 입력했습니다.')
    else:
        print('0은 양의 정수가 아닙니다.')
        
    number = int(input('양의 정수를 입력해주세요: '))    

print('잘했습니다!')

"""
양의 정수를 입력해주세요: -999
프로그램을 종료합니다.
잘했습니다!
"""
```

```python
# 첫 번째 짝수 출력하기
numbers = [1, 3, 5, 6, 7, 9, 10, 11]
found_even = False

for num in numbers:
    if num % 2 == 0:
        print('첫 번째 짝수를 찾았습니다:', num)
        found_even = True
        break

if not found_even:
    print('짝수를 찾지 못했습니다')
    
"""
첫 번째 짝수를 찾았습니다: 6
"""
```



## continue

: 다음 반복으로 건너뜀

- 현재 반복문의 남은 코드를 건너뛰고 다음 반복으로 넘어감

```py
# 리스트에서 홀수만 출력하기
numbers = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]

for num in numbers:
    if num % 2 == 0:
        continue
    print(num)
    
"""
1
3
5
7
9
"""
```



## break와 continue 주의사항

- break와 continue를 남용하는 것은 코드의 가독성을 저하시킬 수 있음
- 특정한 종료 조건을 만들어 break를 대신하거나, if 문을 사용해 continue처럼 코드를 건너뛸 수도 있음
- 약간의 시간이 들더라도 가능한 코드의 가독성을 유지하고 코드의 의도를 명확하게 작성하도록 노력하는 것이 중요



# List Comprehension

: 간결하고 효율적인 리스트 생성 방법

- 코드를 봤을 때 이해하기 어렵기 때문에 남용하지는 말 것

``` python
[expression for 변수 in iterable]

lsit(expression for 변수 in iterable)
```

### 활용

```python
# 1. 일반적인 방법
number = [1, 2, 3, 4, 5]
squared_numbers = []

for num in numbers:
    squared_numbers.append(num**2)
    
print(squared_numbers)	# [1, 4, 9, 16, 25]

# 2. list comprehension
number = [1, 2, 3, 4, 5]
squared_numbers = [num**2 for num in numbers]

print(squared_numbers)	# [1, 4, 9, 16, 25]
```



 ## List Comprehension과 if 조건문

```python
[expression for 변수 in iterable if 조건식]

lsit(expression for 변수 in iterable if 조건식)
```

### 활용

```python
# 1. 일반적인 방법
new_list = []
for i in range(10):
    if i % 2 == 1:
        new_list.append(i)
print(new_list)		# [1, 3, 5, 7, 9]

# 2. list comprehension
new_list_2 = [i for i in range(10) if i % 2 == 1]
print(new_list_2)	# [1, 3, 5, 7, 9]

new_list_3 = [i if i % 2 == 1 else str(i) for i in range(10)]	
print(new_list_3)	# ['0', 1, '2', 3, '4', 5, '6', 7, '8', 9]

# else도 사용 가능하나, 가독성이 떨어짐 (elif는 사용 불가능)
```



## 리스트를 생성하는 3가지 방법 비교

```python
# 정수 1, 2, 3을 가지는 새로운 리스트 만들기
numbers = ['1', '2', '3']

# 1. for loop
new_numbers = []
for number in numbers:
    new_numbers.append(int(number))
print(new_numbers)  # [1, 2, 3]

# 2. map
new_numbers_2 = list(map(int, numbers))
print(new_numbers_2)    # [1, 2, 3]

# 3. list comprehension
new_numbers_3 = [int(number) for number in numbers]
print(new_numbers_3)    # [1, 2, 3]
```

### 성능 => 일반화가 불가능 (외부요인, 상황)

#### loop & map & comprehension

- 대부분의 상황에서는 comprehension이 빠르다.
- 하지만 다른 함수, 내장함수에 따라 map이 더 빠른 경우도 많았다.
- 파이썬이 3점대 후반에 for loop 성능에 비약적인 향상이 있었기 때문에 극단적인 차이는 존재하지 않는다.
- "작은 효율성에 대해서는, 말하자면 97% 정도에 대해서는 잊어버려라. 섣부른 최적화는 모든 악의 근원이다."

- 코드의 가독성 > 간결함
- 프로그래밍은 우리 프로그램이 어떻게 그 목적을 명확하게 전달하는지에 대한 것



# pass

: 아무런 동작도 수행하지 않고 넘어가는 역할

- 문법적으로 문장이 필요하지만 프로그램 실행에는 영향을 주지 않아야 할 때 사용

### 활용

### 1. 코드 작성 중 미완성 부분

- 구현해야 할 부분이 나중에 추가될 수 있고, 코드 컴파일하는 동안 오류가 발생하지 않음

```python
def my_function():
    pass
```

### 2. 조건문에서 아무런 동작을 수행하지 않아야 할 때

```python
if condition:
    pass	# 아무런 동작도 수행하지 않음
else:
    # 다른 동작 수행
```

### 3. 무한 루프에서 조건이 충족되지 않을 때 pass를 사용하여 루프를 계속 진행하는 방법

```python
while True:
    if condition:
        break
    elif condition:
        pass	# 루프 계속 진행
    else:
        print('..')
```



# enumerate

: iterable 객체의 각 요소에 대해 인덱스와 함께 반환하는 내장함수

- 인덱스와 요소의 튜플이 생성됨

```python
enumerate(iterable, start=0)
```

```python
result = ['a', 'b', 'c']
print(enumerate(result))    # <enumerate object at 0x00000190F6C7AD80>
print(list(enumerate(result)))  # [(0, 'a'), (1, 'b'), (2, 'c')]
```

```python
fruits = ['apple', 'banana', 'cherry']

for index, fruit in enumerate(fruits):
    print(f'인덱스 {index}: {fruit}')
    
"""
인덱스 0: apple
인덱스 1: banana
인덱스 2: cherry
"""
```