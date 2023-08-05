# 함수 (Function) 

: 특정 작업을 수행하기 위한 재사용 가능한 코드 묶음

- 두 수의 합을 구하는 함수를 정의하고 사용함으로써 코드의 중복을 방지
- 재사용성이 높아지고, 코드의 가독성과 유지보수성 향상

```python
# 두 수의 합을 구하는 함수
def get_sum(num1, num2):
    return num1 + num2

# 함수를 사용하여 결과 출력
num1 = 5
num2 = 3
sum_result = get_sum(num1, num2)
print(sum_result)
```



# 내장 함수 (Built-in function)

: 파이썬이 기본적으로 제공하는 함수 (별도의 import 없이 바로 사용 가능)

```python
# abs 함수 호출의 반환 값을 result에 할당

result = abs(-1)
print(result)	# 1
```

- 함수 호출 (function call): 함수를 실행하기 위해 함수의 이름을 사용하여 해당 함수의 코드 블록을 실행하는 것



# 함수의 구조

## 함수 구조

```python
def make_sum(pram1, pram2):	# parameter
    # fuction body
    """이것은 두 수를 받아
    두 수의 합을 반환하는 함수입니다.
    
    >>> make_sum(1, 2)
    3
    """
    return pram1 + pram2	# return value
```



## 함수의 정의와 호출

### 함수 정의

- 함수 정의는 def 키워드로 시작
- def 키워드 이후 함수 이름 작성
- 괄호 안에 매개변수를 정의할 수 있음
- 매개변수(parameter)는 함수에 전달되는 값을 나타냄



### 함수 body

- 콜론(:) 다음에 들여쓰기 된 코드 블록
- 함수가 실행될 때 수행되는 코드를 정의
- Docstring은 함수 body 앞에 선택적으로 작성 가능한 함수 설명서



### 함수 반환 값

- 함수는 필요한 경우 결과를 반환할 수 있음
- return 키워드 이후에 반환할 값을 명시
- return 문은 함수의 실행을 종료하고, 결과를 호출 부분으로 반환
- return 문이 없으면 자동으로 None이 반환됨



### 함수 호출

- 함수를 호출하기 위해서는 함수의 이름과 필요한 인자(argument)를 전달해야 함
- 호출 부분에서 전달된 인자는 함수 정의 시 작성한 매개변수에 대입됨



# 매개변수와 인자

## 매개변수 (parameter)

: 함수를 정의할 때, 함수가 받을 값을 나타내는 변수



## 인자 (argument)

: 함수를 호출할 때, 실제로 전달되는 값

```python
def add_numbers(x, y): # x와 y는 매개변수
    result = x + y
    return result

a = 2
b = 3
sum_result = add_numbers(a, b)	# a와 b는 인자
print(sum_result)
```



# 인자의 종류

## Positional Arguments (위치 인자)

- 함수 호출 시 인자의 위치에 따라 전달되는 인자
- 위치인자는 함수 호출 시 반드시 값을 전달해야 함 (누락하면 동작하지 않음)

```python
def greet(name, age):
    print(f'안녕하세요, {name}님! {age}살이시군요.')
    
greet('Alice', 25)
```



## Default Argument Values (기본 인자 값)

- 함수 정의에서 매개변수에 기본 값을 할당하는 것
- 함수 호출 시 인자를 전달하지 않으면, 기본값이 매개변수에 할당됨

```python
def greet(name, age=30):
    print(f'안녕하세요, {name}님! {age}살이시군요.')
    
greet('Alice')	# 안녕하세요, Alice님! 30살이시군요.
greet('Alice', 25)	# 안녕하세요, Alice님! 25살이시군요.
```



## Keyword Arguments (키워드 인자)

- 함수 호출 시 인자의 이름과 함께 값을 전달하는 인자
- 매개 변수와 인자를 일치시키지 않고, 특정 매개변수에 값을 할당할 수 있음
- 인자의 순서는 중요하지 않으며, 인자의 이름을 명시하여 전달
- 단, 호출 시 **키워드 인자는 위치 인자 뒤에** 위치해야 함

```python
def greet(name, age):
    print(f'안녕하세요, {name}님! {age}살이시군요.')
    
greet(name='Alice', age=35)	# 안녕하세요, Alice님! 35살이시군요.
greet(age=35, name='Alice')	# 안녕하세요, Alice님! 35살이시군요.

greet(age=35, 'Alice')	# positional argument follows keyword argument
```



## Arbitrary Argument Lists (임의의 인자 목록)

- 정해지지 않은 개수의 인자를 처리하는 인자

- 함수 정의 시 매개변수 앞에 *를 붙여 사용하며, 여러 개의 인자를 tuple로 처리

- 대표적으로 print 함수가 있음 

  print(*objects, sep=' ', end='\n', file=sys.stdout, flush=False)

```python
def calculate_sum(*args):
    print(args)
    total = sum(args)
    print(f'합계: {total}')
    
"""
(1, 2, 3)	# tuple
합계: 6
"""
calculate_sum(1, 2, 3)
```



## Arbitray Keyword Argument Lists (임의의 키워드 인자 목록)

- 정해지지 않은 개수의 키워드 인자를 처리하는 인자
- 함수 정의 시 매개변수 앞에 **를 붙여 사용하며, 여러 개의 인자를 dictionary로 묶어 처리

```python
def print_info(**kwargs):
    print(kwargs)
    
print_info(name='Eve', age=30)	# {'name': 'Eve', 'age': 30}
```



## 함수 인자 권장 작성 순서

- 위치 -> 기본 -> 가변 -> 키워드 -> 가변 키워드
- 호출 시 인자를 전달하는 과정에서 혼란을 줄일 수 있도록 함
- 단, 모든 상황에 적용되는 절대적인 규칙은 아니며, 상황에 따라 유연하게 조정될 수 있음

```python
def func(pos1, pos2, default_arg='default', *args, kwd, **kwargs):
```



# 함수와 Scope

## Python의 범위 (Scope)

- 함수는 코드 내부에 local scope를 생성하며, 그 외 공간인 global scope로 구분
- scope
  - global scope: 코드 어디에서든 참조할 수 있는 공간
  - local scope: 함수가 만든 scope ( 함수 내부에서만 참조 가능)
- variable
  - global variable: global scope에 정의된 변수
  - local variable: local scope에 정의된 변수



## 변수의 수명주기 (lifecycle)

- 변수의 수명주기는 변수가 선언되는 위치와 스코프에 따라 결정됨

1. built-in scope

   파이썬이 실행된 이후부터 영원히 유지 ex) print()

2. global scope

   모듈이 호출된 시점 이후 혹은 인터프리터가 끝날 때가지 유지

3. local scope

   함수가 호출될 때 생성되고, 함수가 종료될 때까지 유지

   

## 이름 검색 규칙 (Name Resolution)

- 파이썬에서 사용되는 이름(식별자)들은 특정한 이름공간(namescope)에 저장되어 있음
- 아래와 같은 순서로 이름을 찾아 나가며, LEGB Rule이라고 부름
  1. Local scope: 지역 범위 (현재 작업 중인 범위)
  2. Enclosed scope: 지역 범위 한 단계 위 범위
  3. Global scope: 최상단에 위치한 범위
  4. Buil-in scope: 모든 것을 담고 있는 범위 (정의하지 않고 사용할 수 있는 모든 것)
- 함수 내에서는 바깥 Scope의 변수에 접근 가능하나 수정은 할 수 없음 (웬만하면 사용하지 않음)
- 이름을 찾을 때 하위 scope로 가지 않음



### LEGB Rule 예시

- sum이라는 이름을 global scope에서 사용하게 되면서 기존에 buil-in scope에 있던 내장함수 sum을 사용하지 못함
- sum을 참조 시 LEGB Rule에 따라 global에서 먼저 찾기 때문
- sum 변수 객체 삭제를 위해 del sum 입력 후 진행

```python
print(sum)	# <built-in function sum>
print(sum(range(3)))	# 3

sum = 5

print(sum)	# 5
print(sum(range(3)))	# TypeError: 'int' object is not callable
```

```python
a = 1
b = 2

def enclosed():
    a = 10
    c = 3
    
    def local(c):
        print(a, b, c)	# 10, 2, 500
        # c에 500이 호출되고 500이 호출된 시점에서의 a, b 값이 호출됨
    local(500)
    print(a, b, c)	# 10, 2, 3
    
enclosed()
print(a, b)	# 1, 2
# global에서 찾음
```



## 'global' 키워드

- 변수의 스코프를 전역 범위로 지정하기 위해 사용
- 일반적으로 함수 내에서 전역 변수를 수정하려는 경우에 사용

```python
num = 0	# 전역 변수

def increment():
    global num	# num을 전역 변수로 사용
    num += 1
    
print(num)	# 0
increment()
print(num)	# 1
```



### 'global'  키워드 주의사항

- global 키워드 선언 전에 접근 시

```python
num = 0

def increment():
   # SyntaxError: name 'num' is used prior to global declaration
    print(num)
    global num
    num += 1
```

- 매개변수에 global 사용 불가

```python
num = 0

def increment(num):
   # "num" is assigned before global declaration
    global num
    num += 1
```

- global 키워드는 가급적 사용하지 않는 것을 권장
- 함수로 값을 바꾸고자 한다면 항상 인자로 넘기고 함수의 반환 값을 사용하는 것을 권장



# 재귀 함수

: 함수 내부에서 자기 자신을 호출하는 함수

- 특정 알고리즘 식을 표현할 때 변수의 사용이 줄어들며, 코드의 가독성이 높아짐
- 1개 이상의 base case(종료되는 상황)가 존재하고, 수렴하도록 작성 (반복되는 호출이 종료 조건을 향하도록)



## 재귀 함수 예시 - 팩토리얼

- faxtorial 함수는 자기 자신을 재귀적으로 호출하여 입력된 숫자 n의 팩토리얼을 계산
- 재귀 호출은 n이 0이 될 때까지 반복되며, 종료 조건을 설정하여 재귀 호출이 멈추도록 함 (무한 호출 주의)
- 재귀 호출의 결과를 이용하여 문제를 작은 단위의 문제로 분할하고, 분할된 문제들의 결과를 조합하여 최종 결과 도출

```python
def factorial(n):
    # 종료 조건: n이 0이면 1을 반환
    if n == 0:
        return 1
    # 재귀 호출: n과 n-1의 팩토리얼을 곱한 결과를 반환
    return n * factorial(n - 1)
```



# 유용한 함수

## map(function, iterable)

: 순회 가능한 데이터 구조(iterable)의 모든 요소에 함수를 적용하고, 그 결과를 map object로  반환

- 함수를 인자로 받는다는 점에서 매우 강력한 함수
- iterable은 리스트와 문자열 같은 반복 가능한 시퀀스 구조

```python
numbers = [1, 2, 3]
result = map(str, numbers)

print(result)	# <map object at 0x00000239C915D760>
print(list(result))	# ['1', '2', '3']

# map을 사용하지 않는 방법
result = []
for number in numbers:
    result.append(str(number))
    
print(result)
```

- map() 사용 예시 (swea 문제 풀이 시)

```python
import sys

sys.stdin = open('input.txt')

T = int(input())

K, N, M = map(int, input().split())
```



## zip(*iterables)

: 임의의 iterable을 모아 튜플을 원소로 하는 zip object를 반환

```python
girls = ['jane', 'ashley']
boys = ['peter', 'jay']
pair = zip(girls, boys)

print(pair)	# <zip object at 0x000001C76DE58700>
print(list(pair))	# [('jane', 'peter'), ('ashley', 'jay')]
```

```python
names = ['Alice', 'Bob', 'Charlie']
ages = [30, 25, 35]
cities = ['New York', 'London', 'Paris']

for name, age, city in zip(names, ages, cities):
    print(name, age, city)
'''
Alice 30 New York
Bob 25 London
Charlie 35 Paris
'''
    
# 인자 수 안 맞으면 어떻게 출력하는지 확인하기
names = ['Alice', 'Bob', 'Charlie']
ages = [30, 25]
cities = ['New York', 'London', 'Paris']

for name, age, city in zip(names, ages, cities):
    print(name, age, city)	
'''
Alice 30 New York
Bob 25 London
'''
```

```python
# 두 개의 리스트를 딕셔너리로 변환하기
keys = ['a', 'b', 'c']
values = [1, 2, 3]
my_dict = dict(zip(keys, values))
print(my_dict)	# {'a': 1, 'b': 2, 'c': 3}
```



## lambda 매개변수: 표현식

: 이름 없이 정의되고 사용되는 익명 함수

- 간단한 연산이나 함수를 한 줄로 표현할 때 사용
- 함수를 매개변수로 전달하는 경우에도 유용하게 활용

```python
def addition(x, y):
    return x + y
=> addition = lambda x, y: x + y
```

```python
# map + lambda
numbers = [1, 2, 3, 4, 5]
result = list(map(lambda x: x * 2, numbers))
print(result)	# [2, 4, 6, 8, 10]

# lambda를 사용하지 않는 방법
numbers = [1, 2, 3, 4, 5]

def double_number(x):
    return x * 2

result = list(map(double_number, numbers))
print(result)	# [2, 4, 6, 8, 10]
```



# Packing

: 여러 개의 값을 하나의 변수에 묶어서 담는 것



## 패킹 예시

- 변수에 담긴 값들은 **튜플(tuple)** 형태로 묶임

```python
packed_values = 1, 2, 3, 4, 5
print(packed_values)	# (1, 2, 3, 4, 5)
```



## * 을 활용한 패킹

- *는 남은 요소들을 **리스트**로 패킹하여 할당

```python
numbers = [1, 2, 3, 4, 5]
a, *b, c = numbers

print(a)	# 1
print(b)	# [2, 3, 4]
print(c)	# 5
```



# Unpacking

: 패킹된 변수의 값을 개별적인 변수로 분리하여 할당하는 것



## 언패킹 예시

- 튜플이나 리스트 등의 객체의 요소들을 개별 변수에 할당

```python
packed_values = 1, 2, 3, 4, 5
a, b, c, d, e = packed_values

print(a, b, c, d, e)	# 1 2 3 4 5
```

```python
# 변수가 적은 경우
packed_values = 1, 2, 3, 4, 5
a, b, c = packed_values

print(a, b, c)	# ValueError: too many values to unpack
```

```python
# 변수가 많은 경우
packed_values = 1, 2, 3, 4, 5
a, b, c, d, e, f = packed_values

print(a, b, c, d, e, f)	# ValueError: not enough values to unpack
```



## * 을 활용한 언패킹

- *는 리스트의 요소를 언패킹

```python
names = ['alice', 'jane', 'peter']
print(*names)	# alcie jane peter
```



## ** 을 활용한 언패킹

- **는 딕셔너리의 키-값 쌍을 함수의 키워드 인자로 언패킹

```python
def my_function(x, y, z):
    print(x, y, z)
    
my_dict = {'x': 1, 'y': 2, 'z': 3}
my_function(**my_dict)	# 1 2 3
# **my_dict는 x = 1, y = 2, z = 3을 의미
```



# Module

: 한 파일로 묶인 변수와 함수의 모음으로, 특정한 기능을 하는 코드가 작성된 파일(.py)

- 이미 다른 프로그래머가 이미 작성해 놓은 수천, 수백만 줄의 코드를 사용하는 것은 생산성에서 매우 중요



## 모듈 예시

- 파이썬의 math 모듈
- 파이썬이 미리 작성해 둔 수학 관련 변수와 함수가 작성된 모듈

```python
import math

print(math.pi)	# 3.141592...

print(math.sqrt(4))	# 2.0
```



## 모듈 import

### 모듈 가져오기

- 모듈 내 변수와 함수에 접근하려면 import문이 필요
- 내장 함수 help를 사용해 모듈에 무엇이 들어 있는지 확인 가능



### 모듈 사용하기

- '. (dot)'은 "점의 왼쪽 객체에서 점의 오른쪽 이름을 찾아라"라는 의미의 연산자



### 모듈을 import하는 다른 방법

- from 절을 활용해 특정 모듈을 미리 참조하고 어떤 요소를 import 할지 명시
- 하지만 '. (dot)'를 사용하여 import하는 것이 더 명시적임

```python
from math import pi, sqrt

print(pi)
print(sqrt(4))
```



### 모듈 주의사항

- 만약 서로 다른 모듈이 같은 이름의 함수를 제공할 경우 문제 발생
- 마지막에 import된 이름으로 대체됨

```python
from math import pi, sqrt
from my_math import sqrt
```



### 직접 정의한 모듈 사용하기

```python
# my_math.py
pi = 3.141592

def add(x, y):
    return x + y

# test.py
import my_math

print(my_math.add(1, 2))	# 3

print(my_math.pi)	# 3.141592
```



## 파이썬 표준 라이브러리

: 파이썬 언어와 함께 제공되는 다양한 모듈과 패키지의 모음



## 패키지

: 관련된 모듈들을 하나의 디렉토리에 모아 놓은 것

- '. (dot)'를 사용해 한 번 더 패키지에 접근

```python
from my_package.math import my_math	# my_package 패키지 안에 있는 math 패키지 안에 있는 my_math 모듈
```

- PSL 내부 패키지: 설치 없이 바로 import하여 사용
- 외부 패키지: pip를 사용하여 설치 후 import 필요



## pip

: 외부 패키지들을 설치하도록 도와주는 파이썬의 패키지 관리 시스템

- requests 외부 패키지 설치 및 사용 예시

```
$ pip install requests
```

```python
import requests

response = requests.get('https://dog.ceo/api/breeds/image/random')

print(response.json())	# {'message': 'https://images.dog.ceo/breeds/pomeranian/n02112018_5317.jpg', 'status': 'success'}
```



## 패키지 사용 목적

- 모듈들의 이름 공간을 구분하여 충돌을 방지
- 모듈들을 효율적으로 관리하고 재사용할 수 있도록 돕는 역할



# return & print 주의사항

```python
def add(a, b):
    return a + b

a = print('hello')
print(a)	# None

result = add(1, 2) 
print(result)	 # 3

"""
print의 역할은 터미널의 값을 출력할 뿐
코드에서 쓸 수 있는 값을 반환하는 것은 아님 (return이 없는 함수)
"""
```

```python
def add(a, b):
    print(a + b)	# 3

result = add(1, 2)
print(result)	# None

"""
add를 호출했기 때문에 3이 출력되지만 반환 값은 None
return이 없으면 자동으로 return None을 반환하기 때문임
"""
```

