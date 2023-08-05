# Data Types 

: 값의 종류와 그 값에 적용 가능한 연산과 동작을 결정하는 속성

- 값들을 구분하고 어떻게 다뤄야 하는지 알 수 있음
- 각 데이터 타입 값들도 각자에게 적합한 도구를 가짐
- 타입을 명시적으로 지정하면 코드를 읽는 사람이 변수의 의도를 더 쉽게 이해할 수 있고, 잘못된 데이터 타입으로 인한 오류를 미리 예방



# Numeric Types

## int 

: 정수 자료형



### 진수 표현

- 2진수(binary): 0b
- 8진수(octal): 0o
- 16진수(hexadecimal): 0x

```python
print(0b10)	# 2
print(0o30)	# 24
print(0x10)	# 16
```



## float 

: 실수 자료형, 프로그래밍 언어에서 float는 실수에 대한 근삿값



### 실수 연산 시 주의사항

- 컴퓨터는 2진수를 사용, 사람은 10진수를 사용
- 이때 10진수, 01은 2진수로 표현하면 0.000110011001100110011... 같이 무한대로 반복
- 무한대 숫자를 그대로 저장할 수 없어서 사람이 사용하는 10진법의 근삿값만 표시
- 0.1의 경우 360287971896397 / 2 ** 55 이며 0.1에 가깝지만 정확히 동일하지는 않음
- 이런 과정에서 예상치못한 결과가 나타남
- 이런 증상을 Floating point rounding error라고 함



### 실수 연산 시 해결책

- 두 수의 차이가 매우 작은 수보다 작은지를 확인하거나 math 모듈 활용

```python
a = 3.2 - 3.1	# 0.10000000000000009
b = 1.2 - 1.1	# 0.09999999999999987

# 1. 임의의 작은 수 활용
print(abs(a - b) <= 1e - 10)	# True

# 2. math 모듈 활용
import math
print(math.isclose(a, b))	# True
```



### 지수 표현 방식

- e 또는 E를 사용한 지수 표현

```python
# 314 * 0.01
number = 314e-2

# float
print(type(number))

# 3.14
print(number)
```



# Sequence Types 

: 여러 개의 값들을 순서대로 나열하여 저장하는 자료형 (str, list, tuple. range)

- 순서(Sequence): 값들이 순서대로 저장 (정렬 X)
- 인덱싱(Indexing): 각 값에 고유한 인덱스(번호)를 가지고 있으며, 인덱스를 사용하여 특정 위치의 값을 선택하거나 수정할 수 있음
- 슬라이싱(Slicing): 인덱스 범위를 조절해 부분적인 값을 추출할 수 있음
- 길이(Length): len() 함수를 사용하여 저장된 값의 개수(길이)를 구할 수 있음
- 반복(Iteration): 반복문을 사용하여 저장된 값들을 반복적으로 처리할 수 있음



## Str

: 문자열, 문자들의 순서가 있는 변경 불가능한 시퀀스 자료형



### 문자열 표현

- 문자열은 단일 문자나 여러 문자의 조합으로 이루어짐
- 작은따옴표(') 또는 큰따옴표(") 감싸서 표현



### 중첩 따옴표

- 따옴표 안에 따옴표를 표현할 경우
  - 작은따옴표가 들어 있는 경우는 큰따옴표로 문자열 생성
  - 큰따옴표가 들어 있는 경우는 작은따옴표로 문자열 생성



### Escape sequence

- 역슬래시(backslash) 뒤에 특정 문자가 와서 특수한 기능을 하는 문자 조합
- 파이썬의 일반적인 문법 규칙을 잠시 탈출한다는 의미
- \n (줄 바꿈), \t (탭), \\ (백슬래시), \\' (작은따옴표), \\\\" (큰따옴표) 

```python
# 철수야 '안녕'
print('철수야 \'안녕\'')
print('이 다음은 엔터\n입니다.')
```



### String Interpolation

: 문자열 내에 변수나 표현식을 삽입하는 방법

- **f-string**: 문자열에 f 또는 F를 붙이고 표현식을 {expression}로 작성하여 문자열에 표현식의 값을 삽입할 수 있음

```python
# f-string
bugs = 'roaches'
counts = 13
area = 'living room'
print(f'Debigging {bugs} {counts} {area}')	# Debigging roaches 13 living room

# f-string 응용
greeting = 'hi'
print(f'{greeting:>10}')    # 오른쪽 끝에 출력
print(f'{greeting:^10}')    # 가운데에 출력
print(f'{3.141592:.4f}')    # 소수점 4자리까지 출력
```



### 문자열의 시퀀스 특징

```python
my_str = 'hello'

# 인덱싱
print(my_str[1])	# e

# 슬라이싱
print(my_str[2:4])	# ll

# 길이
print(len(my_str))	# 5
```



## 인덱스

: 시퀀스 내의 값들에 대한 고유한 번호로, 각 값의 위치를 식별하는 데 사용 되는 숫자

|  H   |  E   |  L   |  L   |  O   |
| :--: | :--: | :--: | :--: | :--: |
|  0   |  1   |  2   |  3   |  4   |
|  -5  |  -4  |  -3  |  -2  |  -1  |



## 슬라이싱

: 시퀀스의 일부분을 선택하여 추출하는 작업

- 시작 인덱스와 끝 인덱스를 지정하여 해당 범위의 값을 포함하는 새로운 시퀀스 생성

```python
my_str[0:6]	# 슬라이싱 a[start:stop:step] stop - 1까지 추출
my_str[:7]	# start를 생략하면 맨앞에서부터, stot을 생략하면 맨뒤까지
my_str[::2]	# step을 지정하여 추출
my_str[::-1]	# 문자열 뒤집기
```

- 문자열은 **불변**

```python
# TypeError: 'str' object does not support item assignmaent
my_str[1] = 'z'
```



## list

: 여러 개의 값을 순서대로 저장하는 변경 가능한 시퀀스 자료형



### 리스트 표현

- 0개 이상의 객체를 포함하며 데이터 목록을 저장
- 대괄호 ([])로 표시
- 데이터는 어떤 자료형도 저장할 수 있음

```python
my_list = [1, 2, 3, 'Python', ['hello', 'world', '!!!']]

print(len(my_list))	# 5
print(my_list[4][-1])	# !!!
print(my_list[-1][1][0])	# w
```

- 리스트는 **가변**

```python
my_list = [1, 2, 3]
my_list[0] = 100

print(my_list)	# [100, 2, 3]
```



## tuple

: 여러 개의 값을 순서대로 저장하는 변경 불가능한 시퀀스 자료형



### 튜플 표현

- 0개 이상의 객체를 포함하여 데이터 목록을 저장
- 소괄호 (())로 표시
- 데이터는 어떤 자료형도 저장할 수 있음

```python
my_tuple_1 = ()
my_tuple_2 = (1,)	# (,)가 없으면 정수형 자료로 인식
my_tuple_3 = (1, 'a', 3, 'b', 5)
```

- 튜플은 **불변**

```python
my_tuple = (1, 'a', 3, 'b', 5)

# TypeError: 'tuple' object does not support item assignmaent
my_tuple[1] = 'z'
```



### 튜플의 쓰임

- 튜플의 불변 특성을 사용하여 안전하게 여러 개의 값을 전달, 그룹화, 다중 할당 등 개발자가 직접 사용하기보다 '파이썬 내부 동작'에서 주로 사용됨

```python
x, y = (10, 20)

print(x)	# 10
print(y)	# 20

# 파이썬은 쉼표를 튜플 생성자로 사용하니 괄호는 생략 가능
x, y = 10, 20
```



## range

: 연속된 정수 시퀀스를 생성하는 변경 불가능한 자료형



### range 표현

- range(n): 0부터 n-1까지의 숫자 시퀀스
- range(n, m): n부터 m-1까지의 숫자 시퀀스

```python
my_range_1 = range(5)
my_range_2 = range(1, 10)

print(my_range_1)
print(my_range_2)

# 리스트로 형 변환 시 데이터 확인 가능
print(list(my_range_1))	# [0, 1, 2, 3, 4]
print(list(my_range_2))	# [1, 2, 3, 4, 5, 6, 7, 8, 9]
```



# Non-sequence Types

## dict

: key - value 쌍으로 이루어진 순서와 중복이 없는 변경 가능한 자료형



### 딕셔너리 표현

- **key는 변경 불가능**한 자료형만 사용 가능 (str, int, float, tuple, range ...)
- **value는 모든 자료형** 사용 가능
- 중괄호({})로 표기
- key를 통해 value에 접근

```python
my_dict = {'apple': 12, 'list': [1, 2, 3]}

print(my_dict['apple'])	# 12
print(my_dict['list'])	# [1, 2, 3]

# 값 변경
my_dict['apple'] = 100
print(my_dict)	# {'apple': 100, 'list': [1, 2, 3]}
```



## set

: 순서와 중복이 없는 변경 가능한 자료형



### 세트 표현

- 수학에서의 집합과 동일한 연산 처리 가능
- 중괄호 ({})로 표기
- 순서가 없기 때문에 인덱스가 존재하지 않음
- 중복되지 않음

```python
my_set_1 = set()
my_set_2 = {1, 2, 3}
my_set_3 = {1, 1, 1}

print(my_set_1)	# set()
print(my_set_2)	# {1, 2, 3}
print(my_set_3)	# {1}
```



### 세트의 집합 연산

```python
my_set_1 = {1, 2, 3}
my_set_2 = {3, 6, 9}

# 합집합
print(my_set_1 | my_set_2)	# {1, 2, 3, 6, 9}

# 차집합
print(my_set_1 - my_set_2) # {1, 2}

# 교집합
print(my_set_1 & my_set_2)	# {3}
```



# Other Types

## None

: 파이썬에서 '값이 없음'을 표현하는 자료형

```python
variable = None
print(variable)	# None
```



## Boolean

: 참(True)과 거짓(False)을 표현하는 자료형



### 불리언 표현

- 비교 / 논리 연산의 평가 결과로 사용됨
- 주로 조건 / 반복문과 함께 사용

```python
bool_1 = True
bool_2 = False

print(bool_1)	# True
print(bool_2)	# False
print(3 > 1)	# True
print('3' != 3)	# True
```



# Collection

: 여러 개의 항목 또는 요소를 담는 자료 구조 (str, list, tuple, set, dict)

| 컬렉션 | 변경 가능 여부 |  나열 여부   |
| :----: | :------------: | :----------: |
|  str   |       X        |  O (시퀀스)  |
|  list  |       O        |  O (시퀀스)  |
| tuple  |       X        |  O (시퀀스)  |
|  set   |       O        | X (비시퀀스) |
|  dict  |    O (key)     | X (비시퀀스) |



## 가변과 불변의 차이

```python
# 리스트는 주소를 할당
list_1 = [1, 2, 3]
lsit_2 = list_1

list_1[0] = 100
print(list_2)	# [100, 2, 3]
```

```python
# 문자열은 값을 할당
x = 10
y = x 

x = 20
print(y)	# 10
```

![image](https://github.com/ragu6963/TIL/assets/32388270/b6dca7db-4a13-4e75-843b-cbc8badf3691)



# Type Conversion

## 암시적 형변환

: 파이썬이 자동으로 형변환을 하는 것

- Boolean과 Numeric Type에서만 가능

```python
print(3 + 5.0)	# 8.0
print(True + 3)	# 4
print(True + False)	# 1
```



## 명시적 형변환

: 개발자가 직접 형변환을 하는 것, 암시적 형변환이 아닌 경우를 모두 포함

- str -> integer: 형식에 맞는 숫자만 가능
- integer -> str: 모두 가능

```python
print(int('1'))	# 1
print(str(1) + '등')	# 1등
print(float('3.5'))	 # 3.5
print(int(3.5))	# 3

# ValueError: invalid literal for int() with base 10: '3.5'
print(int('3.5'))
```



## 컬렉션 간 형변환 정리

|           | str  |  list   |  tuple  | range |   set   | dict |
| :-------: | :--: | :-----: | :-----: | :---: | :-----: | :--: |
|  **str**  |      |    O    |    O    |   X   |    O    |  X   |
| **list**  |  O   |         |    O    |   X   |    O    |  X   |
| **tuple** |  O   |    O    |         |   X   |    O    |  X   |
| **range** |  O   |    O    |    O    |       |    O    |  X   |
|  **set**  |  O   |    O    |    O    |   X   |         |  X   |
| **dict**  |  O   | O (key) | O (key) |   X   | O (key) |      |



# 비교 연산자

## is 비교 연산자

- 메모리 내에서 같은 객체를 참조하는지 확인
- ==는 동등성(equality), is는 식별성(identity)
- 값을 비교하는 ==와 다름
- is : 같음 / is not : 같지 않음

```python
print(3 > 6)	# False
print(2.0 == 2)	# True
print(2 != 2)	# False
print('HI' == 'hi')	# Fasle

# SyntaxWarning
# ==은 값(데이터)을 비교하는 것이지만 is는 레퍼런스(주소)를 비교하기 때문
# is 연산자는 되도록이면 None, True, False 등을 비교할 때 사용
print(2.0 is 2)	# False
```



## 논리 연산자

```python
print(True and False)	# False
print(True or False)	# True
print(not True)	# False
print(not 0)	# True
```



## 단축평가

: 논리 연산에서 두 번째 피연산자를 평가하지 않고 결과를 결정하는 동작

- 코드 실행을 최적화하고, 불필요한 연산을 피할 수 있도록 함

```python
vowels = 'aeiou'

print(('a' and 'b') in vowels)	# False
# ('a' and 'b')에서 끝까지 평가 -> 이 문장은 'b'가 vowels에 있는지를 평가
print(('b' and 'a') in vowels)	# True

print(3 and 5)	# 5
print(3 and 0)	# 0
print(0 and 3)	# 0
print(0 and 0)	# 0 (왼쪽 0)

print(5 or 3)	# 5
print(3 or 0)	# 3
print(0 or 3)	# 3
print(0 or 0)	# 0 (오른쪽 0)
```

- and
  - 첫 번째 피연산자가 False인 경우, 전체 표현식은 False로 결정 / 두 번째 피연산자는 평가되지 않고 그 값이 무시
  - 첫 번째 피연산자가 True인 경우, 전체 표현식의 결과는 두 번째 피연산자에 의해 결정 / 두 번째 피연산자가 평가되고 그 결과가 전체 표현식의 결과로 반환

- or
  - 첫 번째 피연산자가 True인 경우, 전체 표현식은 True로 결정 / 두 번째 피연산자는 평가되지 않고 그 값이 무시
  - 첫 번째 피연산자가 False인 경우, 전체 표현식의 결과는 두 번째 피연산자에 의해 결정 / 두 번째 피연산자가 평가되고 그 결과가 전체 표현식의 결과로 반환



## 멤버십 연산자

: 특정 값이 시퀀스나 다른 컬렉션에 속하는지 여부를 확인 (결과는 True/False)

```python
word = 'hello'
numbers = [1, 2, 3, 4, 5]

print('h' in word)	# True
print('z' in word)	# False

print('4' not in numbers)	# False
print('6' not in numbers)	# True
```



## 시퀀스형 연산자

- +(결합 연산자)와 *(반복 연산자)는 시퀀스 간 연산에서 산술 연산자일 때와 다른 역할을 가짐

```python
# Gildong Hong
print('Gildong' + 'Hong')

# hihihihihi
print('hi' * 5)

# [1, 2, 'a', 'b']
print ([1, 2] + ['a', 'b'])

#[1, 2, 1, 2]
print([1, 2] * 2)
```



## 연산자 우선순위

| 우선순위 |        연산자         |         내용          |
| :------: | :-------------------: | :-------------------: |
|   높음   |          ()           |    소괄호 grouping    |
|          |          []           |   인덱싱,  슬라이싱   |
|          |          **           |       거듭제곱        |
|          |         +, -          | 단항 연산자 양수/음수 |
|          |      *, /, //, %      |      산술 연산자      |
|          |         +, -          |      산술 연산자      |
|          | <,  <=, >, >=, ==, != |      비교 연산자      |
|          |      is,  is not      |       객체 비교       |
|          |      in,  not in      |     멤버십 연산자     |
|          |          not          |       논리 부정       |
|          |          and          |       논리 AND        |
|   낮음   |          or           |        논리 OR        |
