# JavaScript Reference data types

## 함수

> 참조 자료형에 속하며 모든 함수는 Function object

- 참조 자료형 (Objects)
  - Object, Array, Function
  - 객체의 주소가 저장되는 자료형 (가변, 주소가 복사)



### 함수 구조

- 함수 이름
- 함수의 매개변수
- 함수의 body를 구성하는 statement
- return 값이 없다면 undefined를 반환

```javascript
function name ([param[, param, [..., param]]]) {
    statements
    return value
}
```



### 함수 정의 2가지 방법

#### 선언식 (function declaration)

- 익명 함수 사용 불가능
- 호이스팅 있음

```javascript
function funcName () {
    statements
}
```



#### 표현식 (function expression)

- 익명 함수 사용 가능
- 호이스팅 없음
- 사용 권장

```javascript
const funcName = function () {
    statements
}
```



#### 함수 표현식  특징

- 함수 이름이 없는 '익명 함수'를 사용할 수 있음
- 선언식과 달리 표현식으로 정의한 함수는 호이스팅되지 않으므로 함수를 정의하기 전에 먼저 사용할 수 없음

```javascript
// 함수 선언식
add(1, 2) 	// 3
function add(num1, num2) {
  return num1 + num2
}

// 함수 표현식
sub(2, 1)	// ReferenceError
const sub = function (num1, num2) {
  return num1 - num2
}
```



### 매개변수

#### 기본 함수 매개변수

- 값이 없거나 undefined가 전달될 경우 이름 붙은 매개변수를 기본값으로 초기화

```javascript
const greeting = function (name = 'Anonymous') {
  return `Hi ${name}`
}

console.log(greeting())	// Hi Anonymous
```



#### 나머지 매개변수

- 임의의 수 인자를 '배열'로 허용하여 가변 인자를 나타내는 방법
- 작성 규칙
  - 함수 정의 시 나머지 매개변수 하나만 작성할 수 있음
  - 나머지 매개변수는 함수 정의에서 매개변수 마지막에 위치해야 함

```javascript
const myFunc = function (num1, num2, ...restArgs) {
  return [num1, num2, restArgs]
}

console.log(myFunc(1, 2, 3, 4, 5))	// [1, 2, [3, 4, 5]]
console.log(myFunc(1, 2))	// [1, 2, []]
```



#### 매개변수와 인자의 개수 불일치

- 매개변수 개수 > 인자 개수
  - 누락된 인자는 undefined로 할당

```javascript
const threeArgs = function (num1, num2, num3) {
  return [num1, num2, num3]
}

console.log(threeArgs())		// [undefined, undefined, undefined]
console.log(threeArgs(1))		// [1, undefined, undefined]
console.log(threeArgs(2, 3))	// [2, 3. undefined]
```



- 매개변수 개수 < 인자 개수
  - 초과 입력한 인자는 사용하지 않음

```javascript
const noArgs = function () {
  return 0
}

console.log(noArgs(1, 2, 3))		// 0

const twoArgs = function (num1, num2) {
  return [num1, num2]
}

console.log(twoArgs(1, 2, 3))	// [1, 2]
```



### '...' 전개 구문 (Spread syntax)

- 배열이나 문자열과 같이 반복 가능한 항목을 펼치는 것 (확장, 전개)
- 전개 대상에 따라 역할이 다름
  - 배열이나 객체의 요소를 개별적인 값으로 분리하거나 다른 배열이나 객체의 요소를 현재 배열이나 객체에 추가하는 등

1. 함수와의 사용
   1. 함수 호출 시 인자 확장
   2. 나머지 매개변수 (압축)

```javascript
// 1. 함수 호출 시 인자 확장
function myFunc(x, y, z) {
  return x + y + z
}

let numbers = [1, 2, 3]

console.log(myFunc(...numbers)) // 6


// 2. 나머지 매개변수
function myFunc2(x, y, ...restArgs) {
  return [x, y, restArgs]
}

console.log(myFunc2(1, 2, 3, 4, 5)) // [1, 2, [3, 4, 5]]
console.log(myFunc2(1, 2)) // [1, 2, []]
```

2. 객체와의 사용 (객체 파트에서 진행)

3. 배열과의 활용 (배열 파트에서 진행)



### 화살표 함수 (Arrow function expressions)

> 함수 표현식의 간결한 표현법

1. function 키워드 제거 후 매개변수와 중괄호 사이에 화살표(=>) 작성
2. 함수의 매개변수가 하나뿐이라면, 매개변수의 '()' 제거 가능
   - 단 생략하지 않는 것을 권장
3. 함수 본문의 표현식이 한 줄이라면 '{}'과 'return' 제거 가능

```javascript
const arrow1 = function (name) {
  return `hello, ${name}`
}

// 1. function 키워드 삭제 후 화살표 작성
const arrow2 = (name) => { return `hello, ${name}` }

// 2. 인자가 1개일 경우에만 () 생략 가능
const arrow3 = name => { return `hello, ${name}` }

// 3. 함수 본문이 return을 포함한 표현식 1개일 경우에 {} & return 삭제 가능
const arrow4 = name => `hello, ${name}`
```



#### 화살표 함수 심화

- 인자가 없다면 '()' or '_'로 표시 가능
- object를 return 한다면 return을 명시적으로 작성해야 함
- return을 작성하지 않으면 객체를 소괄호로 감싸야 함



## 객체 (Object)

> 키로 구분된 데이터 집합을 저장하는 자료형



### 구조 및 속성

#### 객체 구조

- 중괄호를 이용해 작성
- 중괄호 안에는 key: value 쌍으로 구성된 속성(property)를 여러 개 작성 가능
- key는 문자형만 사용
  - 한 개의 단어일 경우 따옴표 생략 가능
- value는 모든 자료형 허용

```javascript
const user = {
  name: 'Alice',
  'key with space': true,
  greeting: function () {
    return 'hello'
  }
}
```



#### 속성 참조

- 점('.', chaining operator) 또는 대괄호([])로 객체 요소 접근
- key 이름에 띄어쓰기 같은 구분자가 있으면 대괄호 접근만 가능

```javascript
// 조회
console.log(user.name) // Alice
console.log(user['key with space']) // true

// 추가
user.address = 'korea'
console.log(user) // {name: 'Alice', key with space: true, address: 'korea', greeting: ƒ}

// 수정
user.name = 'Bella'
console.log(user.name) // Bella

// 삭제
delete user.name
console.log(user) // {key with space: true, address: 'korea', greeting: ƒ}
```



#### 'in' 연산자

- 속성 객체에 존재하는지 여부를 확인

```javascript
// in 연산자
console.log('greeting' in user) // true
console.log('country' in user) // false
```



### 객체와 함수

#### Method

> 객체 속성에 정의된 함수

- object.method() 방식으로 호출
- 메서드는 객체를 '행동' 할 수 있게 함

```javascript
console.log(user.greeting()) // hello
```



#### 'this' keyword

> 함수나 메서드를 호출한 객체를 가리키는 키워드

- 함수 내에서 객체의 속성 및 메서드에 접근하기 위해 사용
- Python의 self가 선언 시 값이 이미 정해지는 것에 비해 JavaScript의 this는 함수가 호출되기 전까지 값이 할당되지 않고 호출 시에 결정됨 (동적 할당) 

```javascript
const person = {
  name: 'Alice',
  greeting: function () {
    return `Hello my name is ${this.name}`
  },
}

console.log(person.greeting)	// Hello my name is Alice
```



**this 호출 방법에 따른 차이**

- 단순 호출 시 this
  - 가리키는 대상 -> 전역 객체
- 메서드 호출 시 this
  - 가리키는 대상 -> 메서드를 호출한 객체

```javascript
// 1.1 단순 호출
const myFunc = function () {
  return this
}
console.log(myFunc()) // window

// 1.2 메서드 호출
const myObj = {
  data: 1,
  myFunc: function () {
    return this
  }
}
console.log(myObj.myFunc()) // myObj
```



**중첩된 함수에서의 this 문제점과 해결책**

- forEach의 인자로 작성된 콜백 함수는 일반적인 함수 호출이기 때문에 this가 전역 객체를 가리킴
- 화살표 함수는 자신만의 this를 가지지 않기 때문에 외부 함수에서의 this 값을 가져옴

```javascript
// 2.1 일반 함수
const myObj2 = {
  numbers: [1, 2, 3],
  myFunc: function () {
    this.numbers.forEach(function (number) {
      console.log(this) // window
    })
  }
}
console.log(myObj2.myFunc())

// 2.2 화살표 함수
const myObj3 = {
  numbers: [1, 2, 3],
  myFunc: function () {
    this.numbers.forEach((number) => {
      console.log(this) // myObj3
    })
  }
}
console.log(myObj3.myFunc())
```



### 추가 객체 문법

#### 단축 속성

- 키 이름과 값으로 쓰이는 변수의 이름이 같은 경우 단축 구문을 사용할 수 있음

```javascript
const name = 'Alice'
const age = 30

// const user = {
//   name: name,
//   age: age,
// }

const user = {
  name: name,
  age: age,
}
```



#### 단축 메서드

- 메서드 선언 시 function 키워드 생략 가능

```javascript
const myObj1 = {
  myFunc: function () {
    return 'Hello'
  }
}

const myObj2 = {
  myFunc() {
    return 'Hello'
  }
}
```



#### 계산된 속성 (computed property name)

- 키가 대괄호 ([])로 둘러싸여 있는 속성
  - 고정된 값이 아닌 변수 값을 사용할 수 있음

```javascript
const product = prompt('물건 이름을 입력해주세요')
const prefix = 'my'
const suffix = 'property'

const bag = {
  [product]: 5,
  [prefix + suffix]: 'value',
}

console.log(bag) // {연필: 5, myproperty: 'value'}
```



#### 구조 분해 할당 (destructing assignment)

- 배열 또는 객체를 분해하여 속성을 변수에 쉽게 할당할 수 있는 문법

```javascript
const userInfo = {
  firstName: 'Alice',
  userId: 'alice123',
  email: 'alice123@gmail.com'
}

// const firstName = userInfo.name
// const userId = userInfo.userId
// const email = userInfo.email

// const { firstName } = userInfo
// const { firstName, userId } = userInfo
const { firstName, userId, email } = userInfo

// Alice alice123 alice123@gmail.com
console.log(firstName, userId, email)
```



- '함수의 매개변수'로 객체 구조 분해 할당 활용 가능

```javascript
function printInfo({ name, age, city }) {
  console.log(`이름: ${name}, 나이: ${age}, 도시: ${city}`)
}

const person = {
  name: 'Bob',
  age: 35,
  city: 'London',
}

// 함수 호출 시 객체를 구조 분해하여 함수의 매개변수로 전달
printInfo(person) // '이름: Bob, 나이: 35, 도시: London'
```



#### Object with '전개 구문'

- 객체 복사: 객체 내부에서 객체 전개
- 얕은 복사에 활용 가능

```javascript
const obj = { b: 2, c: 3, d: 4 }
const newObj = { a: 1, ...obj, e: 5 }
console.log(newObj) // {a: 1, b: 2, c: 3, d: 4, e: 5}
```



#### 유용한 객체 메서드

- Object.keys()
- Object.values()

```javascript
const profile = {
  name: 'Alice',
  age: 30,
}

console.log(Object.keys(profile)) // ['name', 'age']
console.log(Object.values(profile)) // ['Alice', 30]
```



#### Optional chaining ('?.')

- 속성이 없는 중첩 객체를 에러 없이 접근할 수 있음
- 만약 참조 대상이 null 또는 undefined라면 에러가 발생하는 것 대신 평가를 멈추고 undefined를 반환

```javascript
const user = {
  name: 'Alice',
  greeting: function () {
    return 'hello'
  }
}

console.log(user.address.street)	// Uncaught TypeError
console.log(user.address?.street)	// undifined

console.log(user.nonMethod())		// Uncaught TypeError
console.log(user.nonMethod?.())		// undifined
```



- Optional chaining이 없다면 다음과 같이 '&&' 연산자를 사용해야 함

```javascript
console.log(user.address && user.address.street)	// undefined
```



**Optional chaining 주의 사항**

- Optional chaining은 존재하지 않아도 괜찮은 대상에서만 사용해야 함 (남용 X)
  - 왼쪽 평가 대상이 없어도 괜찮은 경우에만 선택적으로 사용

```javascript
// Bad
user?.address?.street

// Good
user.address?.street
```



- Optional chaining 앞의 변수는 반드시 선언되어 있어야 함

```javascript
console.log(myObj?.address)	// Uncaught ReferenceError
```



### JSON (JavaScript Object Notation)

- Key-Value 형태로 이루어진 자료 표기법
- JavaScript의 Object와 유사한 구조를 가지고 있지만 JSON은 형식이 있는 "문자열"
- JavaScript에서 JSON을 사용하기 위해서는 Object 자료형으로 변경해야 함



#### Object <-> JSON 변환하기

```javascript
const jsObject = {
  coffee: 'Americano',
  iceCream: 'Cookie and cream',
}

// Object -> JSON
const objToJson = JSON.stringify(jsObject)
console.log(objToJson)  // {"coffee":"Americano","iceCream":"Cookie and cream"}
console.log(typeof objToJson)  // string

// JSON -> Object
const jsonToObj = JSON.parse(objToJson)
console.log(jsonToObj)  // { coffee: 'Americano', iceCream: 'Cookie and cream' }
console.log(typeof jsonToObj)  // object
```



### 참고

#### new 연산자

`new construntor[([arguments])]`

- 사용자 정의 객체 타입을 생성
- 매개변수
  - constructor: 객치 인스턴스의 타입을 기술(명세)하는 함수
  - arguments: constructor와 함께 호출될 값 목록

```javascript
function Member(name, age, sId) {
  this.name = name
  this.age = age
  this.sId = sId
}

const member3 = new Member('Bella', 21, 20226543)

console.log(member3) // Member { name: 'Bella', age: 21, sId: 20226543 }
console.log(member3.name) // Bella
```





## 배열 (Array)

> **순서가 있는** 데이터 집합을 저장하는 자료구조

- 대괄호([])를 이용해 작성
- 배열 요소 자료형: 제약 없음
- length 속성을 사용해 배열에 담긴 요소가 몇 개인지 알 수 있음
  - 음수 인덱스가 없기 때문에 마지막 값에 접근하고 싶으면 length 속성 사용

```javascript
const names = ['Alice', 'Bella', 'Cathy',]

console.log(names[0]) // Alice
console.log(names[1]) // Bella
console.log(names[2]) // Cathy

console.log(names.length) // 3
```



### 배열과 메서드

#### push()

> 배열의 끝에 요소를 추가

```javascript
names.push('Dan')
console.log(names) // ['Alice', 'Bella', 'Dan']
```



#### pop()

> 배열의 끝 요소를 제거하고 제거한 요소를 반환

```javascript
const names = ['Alice', 'Bella', 'Cathy',]

console.log(names.pop()) // Cathy
console.log(names) // ['Alice', 'Bella']
```



#### unshift()

> 배열 앞에 요소를 추가

```javascript
names.unshift('Eric')
console.log(names) // ['Eric', 'Bella', 'Dan']
```



#### shift()

> 배열의 앞 요소를 제거하고, 제거한 요소를 반환

```javascript
console.log(names.shift()) // Alice
console.log(names) // ['Bella', 'Dan']
```



### Array Helper Method

> 배열을 순회하며 특정 로직을 수행하는 메서드

- 메서드 호출 시 인자로 함수(콜백 함수)를 받는 것이 특징



#### forEach()

> 인자로 주어진 함수(콜백 함수)를 배열 요소 각각에 대해 실행

`arr.forEach(callback(item[, index[, array]]))`

- 콜백 함수는 3가지 매개변수로 구성
  - item: 처리할 배열의 요소
  - index 처리할 배열 요소의 인덱스 (선택 인자)
  - array: forEach를 호출한 배열 (선택 인자)
- 반환 값: undefined

```javascript
// 일반 함수
names.forEach(function (item, index, array) {
  console.log(`${item} / ${index} / ${array}`)
})

// 화살표 함수
names.forEach((item, index, array) => {
  console.log(`${item} / ${index} / ${array}`)
})

// Alice / 0 / Alice,Bella,Cathy
// Bella / 1 / Alice,Bella,Cathy
// Cathy / 2 / Alice,Bella,Cathy
```



#### 콜백 함수 (Callback function)

> 다른 함수에 인자로 전달되는 함수

- 외부 함수 내에서 호출되어 일종의 루틴이나 특정 작업을 진행

```javascript
const numbers1 = [1, 2, 3,]

numbers1.forEach(function (num)) {
	console.log(num ** 2)
}

// 1
// 4
// 9
```

```javascript
const numbers2 = [1, 2, 3,]

const.callBackFunction = function (num) {
    console.log(num ** 2)
}

numbers2.forEach(callBackFunction)	// 콜백 함수로 쓰일 때는 뒤에 '()' 안 붙임

// 1
// 4
// 9
```



#### map()

> 배열 내의 모든 요소 각각에 대해 함수(콜백 함수)를 호출하고, 함수 호출 결과를 모아 새로운 배열을 반환

`arr.map(callback(item, [, index[, array]]))`

- 매개변수
  - item: 처리할 배열의 요소
  - index: 처리할 배열 요소의 인덱스 (선택 인자)
  - array: map을 호출한 배열 (선택 인자)
- 반환 값: 배열의 각 요소에 대해 실행한 "callback의 결과를 모은 새로운 배열"
  - 기본적으로 forEach 동작 원리와 같지만 forEach와 달리 새로운 배열을 반환함

```javascript
const names = ['Alice', 'Bella', 'Cathy',]

const result1 = names.map(function (name)) {
	return name.length
}

const result2 = names.map((name)  {
	return name.length
})

console.log(result1)	// [5, 5, 5]
console.log(result2)	// [5, 5, 5]
```

```javascript
const numbers = [1, 2, 3,]
const doubleNumber = numbers.map((number) => {
    return number * 2
})

console.log(doubleNumber)	// [2, 4, 6]
```



#### 배열 순회 총합

- **for loop**
  - 배열의 인덱스를 이용하여 각 요소에 접근
  - break, continue 사용 가능
- **for...of**
  - 배열 요소에 바로 접근 가능
  - break, continue 사용 가능
- **forEach**
  - 간결하고 가독성이 높음
  - callback 함수를 이용하여 각 요소를 조작하기 용이
  - break, continue 사용 불가능
  - 사용 권장

```javascript
// for loop
for (let idx = 0; idx < names.length; idx++) {
  console.log(idx, names[idx])
}

// for...of
for (const name of names) {
  console.log(name)
}

// forEach
names.forEach((name, idx) => {
  console.log(idx, name)
})
```



### 추가 배열 문법

#### Array with '전개 구문'

- 배열 복사

```javascript
let parts = ['어깨', '무릎']
let lyrics = ['머리', ...parts, '발']

console.log(lyrics)		// ['머리', '어깨', '무릎', '발']
```



#### 기타 Array Helper Method

**filter**

> 콜백 함수의 반환 값이 참인 요소들만 모아서 새로운 배열을 반환

```javascript
const words = ['spray', 'limit', 'elite', 'exuberant', 'destruction', 'present']

const result = words.filter((word) => word.length > 6)

console.log(result)		// ["exuberant", "destruction", "present"]
```



**find**

> 콜백 함수의 반환 값이 참이면 해당 요소를 반환

```javascript
const array1 = [5, 12, 8, 130, 44]

const found = array1.find((element) => element > 10)

console.log(found)	// 12
```



**some**

> 배열의 요소 중 하나라도 판별 함수를 통과하면 참을 반환

```javascript
const array = [1, 2, 3, 4, 5]

const even = (element) => element % 2 === 0

console.log(array.some(even))	// true
```



**every**

> 배열의 모든 요소가 판별 함수를 통과하면 참을 반환

```javascript
const isBelowTheshold = (currentValue) => currentValue < 40

const array 1 = [1, 30, 39, 29, 10, 13]

console.log(array1.every(isBelowTheshold))	// true
```



### 참고

#### 콜백 함수 구조를 사용하는 이유

- 함수의 재사용성 측면

  - 함수를 호출하는 코드에서 콜백 함수의 동작을 자유롭게 변경할 수 있음
  - 예를 들어, map 함수는 콜백 함수를 인자로 받아 배열의 각 요소를 순회하며 콜백 함수를 실행
  - 이때, 콜백 함수는 각 요소를 변환하는 로직을 담당하므로, map 함수를 호출하는 코드는 간결하고 가독성 높아짐

- 비동기적 처리 측면

  - ```javascript
    console.log('a')
    
    setTimeout(() => {
        console.log('b')
    }, 3000)
    
    console.log('c')
    
    // a
    // c
    // b
    ```

  - setTimeout 함수는 콜백 함수를 인자로 받아 일정 시간이 지난 후에 실행됨

  - 이때, setTimeout 함수는 비동기적으로 콜백 함수를 실행하므로, 다른 코드의 실행을 방해하지 않음



#### "배열은 객체다"

- 배열은 키와 속성들을 담고 있는 참조 타입의 객체
- 배열은 인덱스를 키로 가지며 length 프로퍼티를 갖는 특수한 객체
- 배열의 요소를 대괄호 접근법을 사용해 접근하는 건 객체 문법과 같음
- 다만 배열의 키는 숫자라는 점
- 숫자형 키를 사용함으로써 배열은 객체 기본 기능 이외에도 순서가 있는 컬렉션을 제어하게 해 주는 특별한 메서드를 제공