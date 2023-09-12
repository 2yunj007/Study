# 2. 타입

## 2.1. 변수

- C++에서 숫자 표현에 관련된 변수는 정수형 변수와 실수형 변수로 구분
-  정수형 변수 : char형, int형, long형, long long형
- 실수형 변수 : float형, double형
- 사용자 정의 구조체 변수 : 관련된 데이터를 한 번에 묶어서 처리



**변수와 메모리 주소**

- 변수는 기본적으로 메모리의 주소(address)를 기억하는 역할을 함
  - 메모리 주소란 메모리 공간에서의 정확한 위치를 식별하기 위한 고유 주소
- 변수를 참조할 때는 메모리의 주소를 참조하는 것이 아닌, 해당 주소에 저장된 데이터를 참조
- 변수는 데이터가 저장된 메모리의 주소뿐만 아니라, 저장된 데이터의 길이와 형태에 관한 정보도 같이 기억해야 함

![C 변수](https://www.tcpschool.com/lectures/img_c_variable.png)

**변수의 선언**

- C++에서는 변수를 사용하기 전에 반드시 먼저 해당 변수를 저장하기 위한 메모리 공간을 할당받아야 함
- **변수의 선언만 하는 방법**
  - 먼저 변수를 선언하여 메모리 공간을 할당받고, 나중에 변수를 초기화하는 방법
  - 선언만 된 변수는 초기화하지 않았기 때문에 해당 메모리 공간에는 알 수 없는 쓰레기 값만이 들어가 있음 (초기화하지 않은 변수는 사용하지 않도록 주의)
  -  반드시 해당 타입의 데이터만을 저장

```c++
// 타입 변수이름;
int num;
num = 20;
```

- **변수의 선언과 동시에 초기화하는 방법**
  - C++에서는 변수의 선언과 동시에 그 값을 초기화할 수 있음
  - 또한, 선언하고자 하는 변수들의 타입이 같다면 이를 동시에 선언할 수 있음

```c++
// 타입 변수이름[, 변수이름];
// 타입 변수이름 = 초기값[, 변수이름 = 초기값];
int num1, num2;
double num3 = 1.23, num4 = 4.56;
```



## 2.2. 상수

- 상수(constant)란 변수와 마찬가지로 데이터를 저장할 수 있는 메모리 공간을 의미

- 하지만 상수가 변수와 다른 점은 프로그램이 실행되는 동안 메모리에 저장된 데이터를 변경할 수 없다는 점

- C++에서 상수는 표현 방식에 따라 다음과 같이 나눌 수 있음

  - 리터럴 상수(literal constant)

  - 심볼릭 상수(symbolic constant)



**리터럴 상수(literal constant)**

- 변수와는 달리 데이터가 저장된 메모리 공간을 가리키는 이름을 가지고 있지 않음
- C++에서 상수는 타입에 따라 정수형 리터럴 상수, 실수형 리터럴 상수, 문자형 리터럴 상수 등으로 구분 가능



**정수형 리터럴 상수**

- 123, -456과 같이 아라비아 숫자와 부호로 직접 표현
- C++에서는 정수형 상수를 10진수뿐만 아니라 8진수(0으로 시작)나 16진수(0x로 시작)로도 표현
-  여러 가지 진법으로 표현된 정수형 상수의 출력을 위해 cout 객체는 dec, hex, oct 조정자를 제공
  - 사용자가 다시 변경하기 전까지 출력되는 진법의 형태를 계속 유지

```c++
int a = 10;

cout << "숫자 10을 10진수로 표현하면 " << a << "이며, " << endl;
cout << oct;
cout << "숫자 10을 8진수로 표현하면 " << a << "이며, " << endl;
cout << hex;
cout << "숫자 10을 16진수로 표현하면 " << a << " 입니다.";

/*
숫자 10을 10진수로 표현하면 10이며, 
숫자 10을  8진수로 표현하면 12이며, 
숫자 10을 16진수로 표현하면 a 입니다.
*/
```

- C++에서 정수형 리터럴 상수는 다음과 같은 경우를 제외하면 모두 int형으로 저장
  - 데이터의 값이 너무 커서 int형으로 저장할 수 없는 경우
  - 정수형 상수에 접미사를 사용하여, 해당 상수의 타입을 직접 명시하는 경우
- C++에서는 다음과 같은 접미사를 상수의 끝에 추가하여, 해당 상수의 타입을 직접 명시 가능

![img](https://mblogthumb-phinf.pstatic.net/MjAxODExMDlfMTAz/MDAxNTQxNzY0MTYyNjc1.GEww4OGVwN__cS9-AvyCFvGjamiW1k5y3IahooK9RJIg.5zzJCba_jR_weRFhHVria5khRYTraYKcVOE1w3FaPlsg.PNG.je1206/Screen_Shot_2018-11-09_at_11.49.08.png?type=w800)



**실수형 리터럴 상수**

- 3.14, -45.6과 같이 소수 부분을 가지는 아라비아 숫자로 표현
- C++에서 실수형 리터럴 상수는 모두 부동 소수점 방식으로 저장
- 실수형 리터럴 상수는 모두 double형으로 저장되며, 접미사를 추가하여 저장되는 타입을 직접 명시 가능

![img](https://mblogthumb-phinf.pstatic.net/MjAxODExMDlfMjk3/MDAxNTQxNzY0MzE5MTYy.NA4raErXyrO-tCANKChpshix2q-srC4nQfoqndShdbIg.IFowRgIBotEA05oRZ8-xd9C2a9ylTUAHPPfjbfO0gjog.PNG.je1206/Screen_Shot_2018-11-09_at_11.51.47.png?type=w800)



**문자형 리터럴 상수**

- 'a', 'Z'와 같이 따옴표('')로 감싸진 문자로 표현



**포인터 리터럴 상수**

- 널 포인터(null pointer)란 아무것도 가리키고 있지 않은 포인터를 의미
- nullptr 키워드를 사용한 리터럴 상수의 타입은 포인터 타입이며, 정수형으로 변환할 수 없음
- 아직도 C++에서는 0을 사용해 널 포인터를 명시할 수 있으며, 따라서 nullptr == 0은 참(true)을 반환
  - 하지만 nullptr 리터럴 상수를 사용하는 것이 좀 더 안전



**이진 리터럴 상수**

- C++14부터는 0B 또는 0b의 접두사와 0과 1의 시퀀스를 가지고 이진 리터럴 상수를 표현
- `auto a = 0B010111;`



**심볼릭 상수(symbolic constant)**

- 심볼릭 상수는 변수와 마찬가지로 이름을 가지고 있는 상수
- 심볼릭 상수는 선언과 동시에 반드시 초기화해야 함
- 이러한 심볼릭 상수는 매크로를 이용하거나, const 키워드를 사용하여 선언할 수 있음
- C++에서는 가급적 const 키워드를 사용하여 선언
  - 상수의 타입을 명시적으로 지정 가능
  - 구조체와 같은 복잡한 사용자 정의 타입에도 사용 가능
  - 해당 심볼릭 상수를 특정 함수나 파일에서만 사용할 수 있도록 제한할 수 있음
- `const int ages = 30;`



## 2.3. 기본 타입

**정수형 타입**

- 정수형 데이터에 unsigned 키워드를 추가하면, 부호를 나타내는 최상위 비트(MSB, Most Significant Bit)까지도 크기를 나타내는 데 사용할 수 있음
- unsigned 정수로는 음의 정수를 표현할 수는 없지만, 0을 포함한 양의 정수는 두 배 더 많이 표현할 수 있음
- 음의 정수까지도 표현할 수 있는 signed 키워드는 모든 타입에서 기본적으로 생략 가능

![img](https://mblogthumb-phinf.pstatic.net/MjAxODExMDlfODMg/MDAxNTQxNzY1MDE2MDg2.05MPTF2OuYnIYBOx9_En9Wxo6mBIhMQ41wojz_pfZpUg.whW4i6dSLcii3d4vbl6XbeBOw9UvGcIKa3gSStPi48Ag.PNG.je1206/Screen_Shot_2018-11-09_at_12.03.18.png?type=w800)

- 정수형 데이터의 타입을 결정할 때에는 반드시 자신이 사용하고자 하는 데이터의 최대 크기를 고려해야 함
  - 범위를 벗어나면 오버플로우가 발생하여 전혀 다른 값이 저장



**실수형 타입**

- 과거에는 실수를 표현할 때 float형을 많이 사용했지만, 하드웨어의 발달로 인한 메모리 공간의 증가로 현재에는 double형을 가장 많이 사용

![img](https://mblogthumb-phinf.pstatic.net/MjAxODExMDlfMTMx/MDAxNTQxNzcyMjYzNzEz.qiEJM61N2-FNl3IOnyrw_CdWYyipoxWw6mNKlAcD8zcg.9jeWNQ66QYBuBZeOmg1DQfR7xSx8UKA4OagRmXJkluIg.PNG.je1206/Screen_Shot_2018-11-09_at_14.04.09.png?type=w800)

- 실수형 데이터의 타입을 결정할 때에는 표현 범위 이외에도 유효 자릿수를 반드시 고려해야 함

![img](https://mblogthumb-phinf.pstatic.net/MjAxODExMDlfMTkw/MDAxNTQxNzcyNDM3Nzg1.GkssewnD6PXuQYbtiS7ZyJe_A-nO8qtT2eGuiu_jLPEg.mmg_oDPx3B6ZxHd3dHTpTBjC21wAA-CmQwKOADLdf6wg.PNG.je1206/Screen_Shot_2018-11-09_at_14.07.04.png?type=w800)



**문자형 타입**

![img](https://mblogthumb-phinf.pstatic.net/MjAxODExMDlfMTc4/MDAxNTQxNzcyNTk1Mzgx.T3XKUUQwaKbgI_ZKtEn3yurrxV1-uJ62ShvXbBboh-sg.-mLrQcFATvh_VNFhltrCSjKnNMBhyNOb9DdYB7o0QW8g.PNG.je1206/Screen_Shot_2018-11-09_at_14.09.43.png?type=w800)



**bool 형 타입**

- bool형은 참(true)이나 거짓(false) 중 한 가지 값만을 가질 수 있는 불리언 타입
- C++에서는 어떤 값도 bool형으로 묵시적 타입 변환이 가능
- 이때 0인 값은 거짓(false)으로, 0이 아닌 값은 참(true)으로 자동 변환



**auto 키워드를 이용한 선언**

- 변수 선언 시 초깃값과 같은 타입으로 변수를 선언할 수 있도록 해 줌
- 즉, 변수를 초기화할 때 특정 타입을 명시하는 대신에, auto 키워드를 사용하여 초깃값에 맞춰 타입이 자동으로 선언되도록 설정 가능
- auto 키워드는 함수의 매개변수와 함께 사용 불가능
- int형 변수 a와 float형 변수 b의 합을 auto 키워드를 통해 sum 변수에 저장하고 출력

```c++
#include <iostream>
using namespace std;

int main() {
	int a = 5;
	float b = 3.5;
	auto sum = a + b;
	cout << "sum : " << sum << "\n";
	return 0;
}
```

- auto 키워드를 통해 sum 변수를 함수의 반환 값으로 초기화

```c++
#include <iostream>
using namespace std;

int add(int x, int y) {
	return x + y;
}
int main() {
	auto sum = add(5, 7);
	cout << "sum : " << sum << "\n";
	return 0;
}
```



## 2.4. 부동 소수점 수

**실수의 표현 방식**

- 고정 소수점(fixed point) 방식
- 부동 소수점(floating point) 방식



**고정 소수점 방식**

- 고정 소수점 방식은 정수부와 소수부의 자릿수가 크지 않으므로 표현 범위가 매우 작음

- 32비트 실수를 고정 소수점 방식으로 표현하면 다음과 같음

![고정 소수점 방식](https://www.tcpschool.com/lectures/img_c_fixed_point.png)



**부동 소수점 방식**

- 실수는 보통 정수부와 소수부로 나누지만, 가수부와 지수부로 나누어 표현할 수도 있음

- 현재 대부분의 시스템에서는 부동 소수점 방식으로 실수를 표현

- 표현 방식

  - 소수 부분을 가지는 아라비아 숫자로 표현
  - e 또는 E를 사용하여 지수 표기법으로 표현

  ![img](https://www.tcpschool.com/lectures/img_cpp_exponent.png)



**IEEE 부동 소수점 방식**

- 32비트의 float형 실수를 IEEE 부동 소수점 방식으로 표현하면 다음과 같음

![32비트 부동 소수점](https://www.tcpschool.com/lectures/img_c_floating_point_32.png)

- 64비트의 double형 실수를 IEEE 부동 소수점 방식으로 표현하면 다음과 같음

![64비트 부동 소수점](https://www.tcpschool.com/lectures/img_c_floating_point_64.png)



## 2.5. 타입 변환

- 표현 범위가 좁은 타입으로의 타입 변환은 데이터의 손실이 발생
- C++에서는 다음과 같은 경우에 자동으로 타입 변환을 수행
  - 다른 타입끼리의 대입, 산술 연산 시
  - 함수에 인수를 전달할 때



**묵시적 타입 변환(자동 타입 변환)**

- 묵시적 타입 변환은 대입 연산이나 산술 연산에서 컴파일러가 자동으로 수행해주는 타입 변환
- C++에서는 대입 연산 시 연산자의 오른쪽에 존재하는 데이터의 타입이 연산자의 왼쪽에 존재하는 데이터의 타입으로 묵시적 타입 변환이 진행
- 컴파일러가 자동으로 수행하는 타입 변환은 언제나 데이터의 손실이 최소화되는 방향으로 이루어짐
  - char형 → short형 → int형 → long형 → float형 → double형 → long double형
- 다음 예제는 대입 연산에서 일어나는 묵시적 타입 변환

```c++
int num1 = 3.1415;	// int형 변수에 실수 대입: 소수 부분 자동 삭제
int num2 = 8.3E12;	// int형 변수가 저장 최대 범위 벗어남: 알 수 없는 결과 출력
double num3 = 5;	// 범위가 큰 double형 변수에 범위가 작은 int형 데이터 대입

cout << "num1에 저장된 값은 " << num1 << "입니다." << endl;
cout << "num2에 저장된 값은 " << num2 << "입니다." << endl;
cout << "num3에 저장된 값은 " << num3 << "입니다.";

/*
num1에 저장된 값은 3입니다.
num2에 저장된 값은 2147483647입니다.
num3에 저장된 값은 5입니다.
*/
```

- 다음 예제는 산술 연산에서 일어나는 묵시적 타입 변환

```c++
double result1 = 5 + 3.14;	
// 데이터 손실 최소화되도록 int형 데이터가 double형으로 자동 타입 변환
double result2 = 5.0f + 3.14;	// 마찬가지로 float -> double

cout << "result1에 저장된 값은 " << result1 << "입니다." << endl;
cout << "result2에 저장된 값은 " << result2 << "입니다.";

/*
result1에 저장된 값은 8.14입니다.
result2에 저장된 값은 8.14입니다.
*/
```



**명시적 타입 변환**

- 사용자가 타입 캐스트(cast) 연산자를 사용하여 강제적으로 수행하는 타입 변환
- 산술 연산에 대한 결괏값의 타입은 언제나 피연산자의 타입과 일치 (result1)
- 따라서 result2, result3처럼 하나의 피연산자를 명시적으로 double형으로 변환해야만 정확한 결과값을 얻을 수 있음

```c++
/*
1. (변환할타입) 변환할데이터 // C언어와 C++ 둘 다 사용 가능
2. 변환할타입 (변환할데이터) // C++에서만 사용 가능
*/
int num1 = 1;
int num2 = 4;

double result1 = num1 / num2;
double result2 = (double) num1 / num2;
double result3 = double (num1) / num2;

cout << "result1에 저장된 값은 " << result1 << "입니다." << endl;
cout << "result2에 저장된 값은 " << result2 << "입니다." << endl;
cout << "result3에 저장된 값은 " << result3 << "입니다.";

/*
result1에 저장된 값은 0입니다.
result2에 저장된 값은 0.25입니다.
result3에 저장된 값은 0.25입니다.
*/
```

