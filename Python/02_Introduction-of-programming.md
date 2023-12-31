# Introduction of programming 

## 프로그램 (program)

: 명령어들의 집합

- 새 연산을 정의하고 조합해 유용한 작업을 수행하는 것

- 문제를 해결하는 매우 강력한 방법

- 프로그래밍 언어: 컴퓨터에게 작업을 지시하고 문제룰 해결하는 도구



# 파이썬 소개

## 파이썬을 사용하는 이유

- 간결하고 읽기 쉬운 문법

- 다양한 응용 분야: 데이터 분석, 인공지능, 웹 개발, 자동화 등

- 파이썬 커뮤니티의 지원: 세계적인 규모의 풍부한 온라인 포럼 및 커뮤니티 생태계



## 파이썬 실행

- 컴퓨터는 기계어로 소통하기 때문에 사람이 기계어를 직접 작성하기 어려움
- So, 인터프리터가 사용자의 명령어를 운영체제가 이해하는 언어로 바꿈

![image](https://github.com/ragu6963/TIL/assets/32388270/6318332e-9c76-4c2b-b183-21280b338b8a)



## 인터프리터

### 장점

- 훨씬 더 사용하기 쉽고 운영체제간 이식도 가능
- 확장성 높음 (운영체제가 바뀌더라도 쓰는 언어 자체는 유지 )

### 파이썬 인터프리터 사용 방법

1. shell이라는 프로그램으로 한 번에 한 명령어씩 입력해서 실행

- 실행: python-i
- 종료: exit(), ctrl + c

2. 확장자가 .py인 파일에 작성된 파이썬 프로그램을 실행



# 표현식과 값

## 표현식 (Expression)

- 값, 변수, 연산자 등을 조합하여 계산되고 결과를 내는 코드 구조
- 표현식이 **평가**되어 값이 반환됨



## 평가 (Evaluate)

- 표현식이나 문장을 실행하여 그 결과를 계산하고 값을 결정하는 과정
- 표현식이나 문장을 순차적으로 평가하여 프로그램의 동작을 결정



## 문장 (Statement)

-  실행 가능한 동작을 기술하는 코드 (조건문, 반복문, 함수 정의 등)

- 문장은 보통 여러 개의 표현식을 포함



# 타입

: 값이 어떤 종류의 데이터인지, 어떻게 해석하고 처리되어야 하는지를 정의

- 타입은 두 가지 요소로 이루어짐: '값'과 '값에 적용할 수 있는 연산'



## 데이터 타입

- **Numeric Type**: int(정수), float(실수), complex(복소수)
- **Sequence Types**: list, tuple, range
- **Text Sequence Type**: str(문자열)
- **Set Type**: set
- **Mapping Type**: dict
- 기타: None,Boolean, Function



## 산술 연산자

| 기호 | 연산자           |
| ---- | ---------------- |
| -    | 음수 부호        |
| +    | 덧셈             |
| -    | 뺄셈             |
| *    | 곱셈             |
| /    | 나눗셈           |
| //   | 정수 나눗셈 (몫) |
| %    | 나머지           |
| **   | 지수 (거듭제곱)  |



## 연산자 우선순위

| 우선순위 | 연산자      | 연산                              |
| -------- | ----------- | --------------------------------- |
| 높음     | **          | 지수                              |
|          | -           | 음수 부호                         |
|          | *, /, //, % | 곱셈, 나눗셈, 정수 나눗셈, 나머지 |
| 낮음     | +, -        | 덧셈, 뺄셈                        |

```python
-2 ** 4		# -16
-(2 ** 4)	# -16
(-2) ** 4	# 16
```



# 변수와 메모리

## 변수 (Varianble)

: 값을 참조하는 이름

- "변수 degrees에 값 36.5를 할당했다" / "변수 degrees는 값 36.5를 참조"

```python
degrees = 36.5
```



## 변수명 규칙

- 영문, 알파벳, 언더스코어(_), 숫자로 구성

- 숫자로 시작할 수 없음

- 대소문자를 구분

- 아래 키워드는 파이썬의 내부 예약어로 사용할 수 없음

```python
['False', 'None', 'True', '__peg_parser__', 'and', 'as', 'assert', 
'async', 'await', 'break', 'class', 'continue', 'def', 'del', 
'elif', 'else', 'except', 'finally', 'for', 'from', 'global', 
'if', 'import', 'in', 'is', 'lambda', 'nonlocal', 'not', 
'or', 'pass', 'raise', 'return', 'try', 'while', 'with', 'yield']
```



## 변수, 값 그리고 메모리

- 메모리의 모든 위치에는 그 위치를 고유하게 식별하는 메모리 주소가 존재
- 주소를 쓰게 되면 다른 두 변수가 같은 주소의 값을 사용 가능하기 때문에 데이터 관리 측면에서 좋음

### 객체 (Object)

: 타입을 갖는 메모리 주소 내 값

- "값이 들어 있는 상자"
- 변수는 그 변수가 참조하는 객체의 메모리 주소를 가짐

![image](https://github.com/ragu6963/TIL/assets/32388270/b5dfec1e-9f57-4dba-bf47-d333c988a1ab)

## 할당문

- 할당 연산자(=) 오른쪽에 있는 표현식을 평가해서 값(메모리 주소)을 생성
- 값의 메모리 주소를 '=' 왼쪽에 있는 변수에 저장
- 존재하지 않는 변수라면 새 변수 생성 
- 기존에 존재했던 변수라면 기존 변수를 재사용해서 변수에 들어 있는 메모리 주소를 변경
- 아래와 같이 변수에 재할당해도 변수 double에는 값 20의 주소가 들어 있으니 여전히 20을 참조

```python
number = 10
double = 2 * number
print(double)	# 20

number = 5
print(double)	# 20
```



# 읽기 좋은 코드

## Style Guide

: 코드의 일관성과 가독성을 향상하기 위한 규칙과 권장 사항의 모음 (pep 8)

- 변수명은 무엇을 위한 변수인지 직관적인 이름을 가져야 함

- 공백(spaces) 4칸을 사용하여 코드 블록을 들여쓰기

- 한 줄의 길이는 79자로 제한하며, 길어질 경우 줄 바꿈을 사용

- 문자와 밑줄(_)을 사용하여 함수, 변수, 속성의 이름을 작성

- 함수 정의나 클래스 정의 등의 블록 사이에는 빈 줄을 추가 (2줄)

- 여러 값이 할당되어 있는 시퀀스의 변수명은 복수형으로 작성

  ex) NUMBERS = [1, 2, 3, 4, 5]

- 고정된 변수(상수 값)는 대문자 사용 

  ex) SECONDS_PER_MINUTE



# 주석

: 프로그램 코드 내에 작성되는 설명이나 메모 (ctrl + /)

- 인터프리터에 의해 실행되지 않음
- 여러 줄 주석은 보통 함수에 대해 설명할 때 사용



## 주석의 목적

- 코드의 특정 부분을 설명하거나 임시로 코드를 비활성화할 때
- 코드를 이해하거나 문서화하기 위해
- 다른 개발자나 자신에게 코드의 의도나 동작을 설명하는 데 도움