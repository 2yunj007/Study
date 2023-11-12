# Basic Syntax 1

## Template Syntax

> DOM을 기본 구성 요소 인스턴스의 데이터에 선언적으로 바인딩(Vue Instance와 DOM을 연결)할 수 있는 HTML 기반 템플릿(확장된 문법 제공) 구문을 사용



### Template Syntax 종류

#### Text Interpolation

```html
<p>Message: {{ msg }}</p>
```

- 데이터 바인딩의 가장 기본적인 형태
- 이중 중괄호 구문(콧수염 구문)을 사용
- 콧수염 구문은 해당 구성 요소 인스턴스의 msg 속성 값으로 대체
- msg 속성이 변경될 때마다 업데이트됨



#### Raw HTML

```html
<div v-html="rawHtml"></div>
```

```javascript
const rawHtml = ref('<span style="color:red">This should be red.</span>')
```

- 콧수염 구문은 데이터를 일반 텍스트로 해석하기 때문에 실제 HTML을 출력하려면 v-html을 사용해야 함



#### Attribute Bindings

```html
<div v-bind:id="dynamicId"></div>
```

```javascript
const dynamicId = ref('my-id')
```

- 콧수염 구문은 HTML 속성 내에서 사용할 수 없기 때문에 v-bind를 사용
- HTML의 id 속성 값을 vue의 dynamicId 속성과 동기화되도록 함
- 바인딩 값이 null이나 undefind인 경우 렌더링 요소에서 제거됨



#### JavaScript Expressions

```html
<div>{{ number + 1 }}</div>
<div>{{ ok ? 'Yes' : 'No' }}</div>
<div>{{ msg.split('').reverse().join('') }}</div>
<div v-bind:id="`list-${id}`"></div>
```

- Vue는 모든 데이터 바인딩 내에서 JavaScript 표현식의 모든 기능을 지원
- Vue 템플릿에서 JavaScript 표현식을 사용할 수 있는 위치
  - 콧수염 구문 내부
  - 모든 directive의 속성 값 (v-로 시작하는 특수 속성)

- **주의 사항**
  - 각 바인딩에는 하나의 단일 표현식만 포함될 수 있음
  -  표현식은 값으로 평가할 수 있는 코드 조각 (return 뒤에 사용할 수 있는 코드여야 함)
  - 아래와 같은 식은 작동하지 않음
    - `{{ const number = 1}}`
    - `{{ if (ok) { return message } }}`



### Directive

> 'v-' 접두사가 있는 특수 속성

- Directive의 속성 값은 단일 JavaScript 표현식이어야 함 (v-for, v-on 제외)

- 표현식 값이 변경될 때 DOM에 반응적으로 업데이트 적용

- ex) v-if는 seen 표현식 값의 T/F를 기반으로 <p> 요소를 제거/삽입

  ```html
  <p v-if="seen">Hi There</p>
  ```



#### Directive - Argument

- 일부 directive는 directive 뒤에 콜론(:)으로 표시되는 인자를 사용할 수 있음
- 아래 예시의 href는 HTML a 요소의 href 속성 값을 myUrl 값에 바인딩하도록 하는 v-bind 인자

```html
<a v-bind:href="myUrl">Link</a>
```



- 아래 예시의 click은 이벤트 수신할 이벤트 이름을 작성하는 v-on의 인자

```html
<div v-bind:id="`list-${id}`"></div>
```



#### Directive - Modifiers

- .(dot)로 표시되는 특수 접미사로, directive가 특별한 방식으로 바인딩되어야 함을 나타냄
- 예를 들어 .prevent는 발생한 이벤트에서 event.preventDefault()를 호출하도록 v-on에 지시하는 modifier
- chained 가능

```html
<!-- <form @submit.prevent="onSubmit">...</form> -->
<form v-on:submit.prevent="onSubmit">
  <input type="submit">
</form>
```



#### Built-in Directives

- v-text
- v-show
- v-if
- v-for
- ...
- https://vuejs.org/api/built-in-directives.html



## Dynamically data binding

### v-bind

> 하나 이상의 속성 또는 컴포넌트 데이터를 표현식에 동적으로 바인딩

https://vuejs.org/api/built-in-directives.html#v-bind

- 사용처
  - Attribute Bindings
  - Class and style Bindings



#### Attribute Bindings

- HTML 속성 값을 Vue의 상태 속성 값과 동기화되도록 함

```html
<div id="app">
  <img v-bind:src="imageSrc" alt="#">
  <a v-bind:href="myUrl">Link</a>
</div>
```



- v-bind shorthand (약어)
  - ':' (colon)

```html
<div id="app">
  <img :src="imageSrc" alt="#">
  <a :href="myUrl">Link</a>
</div>
```



- Dynamic attribute name (동적 인자 이름)
  - 대괄호로 감싸서 directive argument에 JavaScript 표현식을 사용할 수도 있음
  - JavaScript 표현식에 따라 동적으로 평가된 값이 최종 argument 값으로 사용됨
  - 대괄호 안에 작성하는 이름은 반드시 소문자로만 구성 가능 (브라우저가 속성 이름을 소문자로 강제 변환)

```html
<p :[dynamicattr]="dynamicValue">.....</p>
```

```javascript
const dynamicattr = ref('title')
const dynamicValue = ref('Hello')
// <p title="Hello">.....</p>
```



#### Class and Style Bindings

- 클래스와 스타일은 모두 속성이므로 v-bind를 사용하여 다른 속성과 마찬가지로 동적으로 문자열 값을 할당할 수 있음
- 그러나 단순히 문자열 연결을 사용하여 이러한 값을 생성하는 것은 번거롭고 오류가 발생하기 쉬움
- Vue는 클래스 및 스타일과 함께 v-bind를 사용할 때 객체 또는 배열을 활용한 개선 사항을 제공



##### Binding HTML Classes - Binding to Objects

- 객체를 `:class`에 전달하여 클래스를 동적으로 전환할 수 있음
- ex) isActive의 T/F에 의해 active 클래스의 존재가 결정됨

```javascript
const isActive = ref(false)
```

```html
<div :class="{ active: isActive }">Text</div>
```



- 객체에 더 많은 필드를 포함하여 여러 클래스를 전환할 수 있음
- ex) `:class` directive를 일반 클래스 속성과 함께 사용 가능

```javascript
const isActive = ref(true)
const hasInfo = ref(true)
```

```html
<div class="static" :class="{ active: isActive, 'text-primary':hasInfo }">Text</div>
<!-- <div class="static active text-primary">Text</div> -->
```



- 반드시 inline 방식으로 작성하지 않아도 됨

```javascript
const isActive = ref(false)
const hasInfo = ref(true)

// ref는 반응 객체의 속성으로 액세스되거나 변경될 때 자동으로 unwrap
const classObj = ref({
  active: isActive,
  'text primary': hasInfo
})
```

```html
<div class="static" :class="classObj">Text</div>
```



- `:class`를 배열에 바인딩하여 클래스 목록을 적용할 수 있음

```javascript
const activeClass = ref('active')
const infoClass = ref('text-primary')
```

```html
<div :class="[activeClass, infoClass]">Text</div>

<!-- 배열 구문 내에서 객체 구문 사용 -->
<div :class="[{ active: isActive }, infoClass]">Text</div>
```



##### Binding Inline Styles - Binding to Objects

- `:style`은 JavaScript 객체 값에 대한 바인딩을 지원 (HTML style 속성에 해당)

```javascript
const activeColor = ref('crimson')
const fontSize = ref(50)
```

```html
<div :style="{ color: activeColor, fontSize: fontSize + 'px' }">Text</div>
```



- 실제 CSS에서 사용하는 것처럼 `:style`은 kebab-cased 키 문자열도 지원 (단, camelCase 작성을 권장)

```html
<div :style="{ 'font-size': fontSize + 'px' }">Text</div>
```



- 템플릿을 더 깔끔하게 작성하려면 스타일 객체에 직접 바인딩하는 것을 권장

```javascript
const styleObj = ref({
  color: activeColor,
  fontSize: fontSize.value + 'px'
})
```

```html
<div :style="styleObj">Text</div>
```

- `fontSize.value`로 써야 하는 이유
  - ref 객체 + 'px'이 됨
  - 반응형 변수가 다른 반응형 변수의 속성 값으로 들어갈 경우에는 auto unwrap됨



- 여러 스타일 객체의 배열에 `:style`을 바인딩할 수 있음
- 작성한 객체는 병합되어 동일한 요소에 적용

```javascript
const styleObj = ref({
  color: activeColor,
  fontSize: fontSize.value + 'px'
})
const styleObj2 = ref({
  color: 'blue',
  border: '1px solid black'
})
```

```html
<div :style="[styleObj, styleObj2]">Text</div>
```



## Event Handling

### v-on

> DOM 요소에 이벤트 리스너를 연결 및 수신

https://vuejs.org/api/built-in-directives.html#v-on



#### v-on 구성

`v-on:event="handler"`

- handler 종류
  - Inline handlers: 이벤트가 트리거될 때 실행될 JavaScript 코드
  - Method handlers: 컴포넌트에 정의된 메서드 이름
- v-on shorthand (약어)
  - '@'
  - `@event="handler"`



#### Inline handlers

- Inline handlers는 주로 간단한 상황에 사용

```javascript
const count = ref(0)
```

```html
<button v-on:click="count++">Add 1</button>
<p>Count: {{ count }}</p>
```



#### Method Handlers

- Inline handlers로는 불가능한 대부분의 상황에서 사용
- Method Handlers는 이를 트리거하는 기본 DOM Event 객체를 자동으로 수신

```javascript
const name = ref('Alice')
const myFunc = function (event) {
  console.log(event)
  console.log(event.currentTarget)
  console.log(`hello, ${name.value}`)
}
```

```html
<button @click="myFunc">Hello</button>
```



#### Inline Handlers에서의 메서드 호출

- 메서드 이름에 직접 바인딩하는 대신 Inline Handlers에서의 메서드를 호출할 수도 있음
- 이렇게 하면 기본 이벤트 대신 사용자 지정 인자를 전달할 수 있음

```javascript
const greeting = function (message) {
  console.log(message)
}
```

```html
<button @click="greeting('hello')">Say hello</button>
<button @click="greeting('bye')">Say bye</button>
```



#### Inline Handlers에서의 event 인자에 접근하기

- Inline Handlers에서 원래 DOM 이벤트에 접근하기
- `$event` 변수를 사용하여 메서드에 전달

```javascript
const warning = function (message, event) {
  console.log(message)
  console.log(event)
}
```

```html
<button @click="warning('경고입니다.', $event)">Submit</button>
```



#### Event Modifiers

-  Vue는 v-on에 대한 Event Modifiers를 제공해 `even.preventDefault()`와 같은 구문을 메서드에서 작성하지 않도록 함
- `stop`, `prevent`, `self` 등 다양한 modifiers를 제공
  - stop: 이벤트의 버블링을 막고, 기본 동작 취소시킴
- 메서드는 DOM 이벤트에 대한 처리보다는 데이터에 관한 논리를 작성하는 것에 집중할 것
- Modifiers는 chained 되게끔 작성할 수 있으며 이때는 작성된 순서로 실행되기 대문에 작성 순서에 유의

```html
<form @submit.prevent="onSubmit">
  <input type="submit">
</form>
<a @click.stop.prevent="onLink">Link</a>
```



#### Key Modifiers

- Vue는 키보드 이벤트를 수신할 때 특정 키에 관한 별도 modifiers를 사용할 수 있음
- ex) key가 Enter일 때만 onSubmit 이벤트 호출하기

```html
<input type="text" @keyup.enter="onSubmit">
```



## Form Input Bindings

- form을 처리할 때 사용자가 input에 입력하는 값을 실시간으로 JavaScript 상태에 동기화해야 하는 경우 (양방향 바인딩)
- 양방향 바인딩 방법
  - v-bind와 v-on을 함께 사용
  - v-model 사용



### v-bind와 v-on을 함께 사용

- v-bind를 사용하여 input 요소의 value 속성 값을 입력 값으로 사용
- v-on을 사용하여 input 이벤트가 발생할 때마다 input 요소의 value 값을 별도 반응형 변수에 저장하는 핸들러를 호출

```javascript
const inputText1 = ref('')
const onInput = function (event) {
  inputText1.value = event.currentTarget.value
}
```

```html
<p>{{ inputText1 }}</p>
<input type="text" @input="onInput" :value="inputText1">
```



### v-model

> form input 요소 또는 컴포넌트에서 양방향 바인딩을 만듦

https://vuejs.org/api/built-in-directives.html#v-model

- v-model을 사용하여 사용자 입력 데이터와 반응형 변수를 실시간 동기화

```javascript
const inputText2 = ref('')
```

```html
<p>{{ inputText2 }}</p>
<input type="text" v-model="inputText2">
```

- IME가 필요한 언어(한국어, 중국어, 일본어 등)의 경우 v-model이 제대로 업데이트되지 않음
- 해당 언어에 대해 올바르게 응답하려면 v-bind와 v-on 방법을 사용해야 함
- v-model과 다양한 입력(input) 양식
  - v-model은 단순 text input 뿐만 아니라 Checkbox, Radio, Select 등 다양한 타입의 사용자 입력 방식과 함께 사용 가능



#### Checkbox 활용

- 단일 체크박스와 boolean 값 활용

```javascript
const checked = ref(false)
```

```html
<input type="checkbox" id="checkbox" v-model="checked">
<label for="checkbox">{{ checked }}</label>
```



- 여러 체크박스와 배열 활용
  - 해당 배열에는 현재 선택된 체크박스의 값이 포함됨

```javascript
const checkNames = ref([])
```

```html
<div>Checked names: {{ checkedNames }}</div>

<input type="checkbox" id="alice" value="Alice" v-model="checkedNames">
<label for="alice">Alice</label>

<input type="checkbox" id="bella" value="Bella" v-model="checkedNames">
<label for="bella">Bella</label>
```



#### Select 활용

- select에서 v-model 표현식의 초기 값이 어떤 option과도 일치하지 않는 경우 select 요소는 "선택되지 않은(unselected)" 상태로 렌더링 됨

```javascript
const selected = ref('')
```

```html
<div>Selected: {{ selected }}</div>

<select v-model="selected">
  <option disabled value="">Please select one</option>
  <option>Alice</option>
  <option>Bella</option>
  <option>Cathy</option>
</select>
```



## 참고

### IME (Input Method Editor)

- 사용자가 입력 장치에서 기본적으로 사용할 수 없는 문자(비영어권 언어)를 입력할 수 있도록 하는 운영 체제 구성 프로그램
- 일반적으로 키보드 키보다 자모가 더 많은 언어에서 사용해야 함
- IME가 동작하는 방식과 Vue의 양방향 바인딩(v-model) 동작 방식이 상충하기 때문에 한국어 입력 시 예상대로 동작하지 않았던 것



# Basic Syntax 2

## Computed Property

### computed()

> 계산된 속성을 정의하는 함수

- 미리 계산된 속성을 사용하여 템플릿에서 표현식을 단순하게 하고 불필요한 반복 연산을 줄임



#### computed 기본 예시

- 할 일이 남았는지 여부에 따라 다른 메시지를 출력하기

```javascript
const todos = ref([
  { text: 'Vue 실습' },
  { text: '자격증 공부' },
  { text: 'TIL 작성' }
])
```

```html
<h2>남은 할 일</h2>
<p>{{ todos.length > 0 ? '아직 남았다' : '퇴근' }}</p>
```

- 템플릿이 복잡해지며 todos에 따라 계산을 수행하게 됨
- 만약 이 계산을 템플릿에 여러 번 사용하는 경우에는 반복이 발생



- computed 적용
- 반응성 데이터를 포함하는 복잡한 로직의 경우 computed를 활용하여 미리 값을 계산

```javascript
const { createApp, ref, computed } = Vue

const restOfTodos = computed(() => {
  return todos.value.length > 0 ? '아직 남았다' : '퇴근!'
})
```

```html
<h2>남은 할 일</h2>
<p>{{ restOfTodos }}</p>
```



#### computed 특징

- 반환되는 값은 computed ref이며 일반 refs와 유사하게 계산된 결과를 .value로 참조할 수 있음 (템플릿에서는 .value 생략 가능)
- computed 속성은 의존된 반응형 데이터를 자동으로 추적
- 의존하는 데이터가 변경될 때만 재평가
  - restOfTodos의 계산은 todos에 의존하고 있음
  - 따라서 todos가 변경될 때만 restOfTodos가 업데이트됨



#### computed 주의사항

##### computed의 반환 값은 변경하지 말 것

- computed의 반환 값은 의존하는 데이터의 파생된 값
- 일종의 snapshot이며 의존하는 데이터가 변경될 때마다 새 snapshot이 생성됨
- snapshot을 변경하는 것은 의미가 없으므로 계산된 반환 값은 읽기 전용으로 취급되어야 하며 변경되어서는 안 됨
- 대신 새 값을 얻기 위해서는 의존하는 데이터를 업데이트해야 함



##### computed 사용 시 원본 배열 변경하지 말 것

- computed에서 reverse() 및 sort() 사용 시 원본 배열을 변경하기 때문에 복사본을 만들어서 진행해야 함

```javascript
return numbers.reverse() // X
return [...numbers].reverse()
```



### Computed vs. Methods

#### Computed와 동일한 로직을 처리할 수 있는 method

- computed 속성 대신 method로도 동일한 기능을 정의할 수 있음
- 두 가지 접근 방식은 실제로 완전히 동일

```javascript
const getRestOfTodos = function () {
  return todos.value.length > 0 ? '아직 남았다' : '퇴근!'
}
```

```html
<h2>남은 할 일</h2>
<p>{{ getRestOfTodos() }}</p>
```



#### computed와 method의 차이

- computed 속성은 의존된 반응형 데이터를 기반으로 캐시(cached)됨
- 의존하는 데이터가 변경된 경우에만 재평가됨
- 즉, 의존된 반응형 데이터가 변경되지 않는 한 이미 계산된 결과에 대한 여러 참조는 다시 평가할 필요 없이 이전에 계산된 결과를 즉시 반환
- 반면 method 호출은 다시 렌더링이 발생할 때마다 항상 함수를 실행



#### Cache (캐시)

- 데이터나 결과를 일시적으로 저장해 두는 임시 저장소
- 이후에 같은 데이터나 결과를 다시 계산하지 않고 빠르게 접근할 수 있도록 함
- 웹 페이지의 캐시 데이터
  - 페이지 일부 데이터를 브라우저 캐시에 저장 후 같은 페이지에 다시 요청 시 모든 데이터를 다시 응답 받는 것이 아닌 캐시 된 데이터를 사용하여 더 빠르게 웹 페이지를 렌더링



#### computed와 method의 적절한 사용처

- computed
  - 의존하는 데이터에 따라 결과가 빠귀는 계산된 속성을 만들 때 유용
  - 동일한 의존성을 가진 여러 곳에서 사용할 때 계산 결과를 캐신하여 중복 계산 방지

- method
  - 단순히 특정 동작을 수행하는 함수를 정의할 때 사용
  - 데이터에 의존하는지 여부와 관계없이 항상 동일한 결과를 반환하는 함수



## Conditional Rendering

### v-if

> 표현식의 값을 T/F를 기반으로 요소를 조건부로 렌더링



#### v-if 예시

- `v-else` directive를 사용하여 v-if에 대한 else 블록을 나타낼 수 있음

```javascript
const isSeen = ref(true)
```

```html
<p v-if="isSeen">true일때 보여요</p>
<p v-else>false일때 보여요</p>
<button @click="isSeen = !isSeen">토글</button>
```



- `v-else-if` directive를 사용하여 v-if에 대한 else if 블록을 나타낼 수 있음

```javascript
const name = ref('Cathy')
```

```html
<div v-if="name === 'Alice'">Alice입니다</div>
<div v-else-if="name === 'Bella'">Bella입니다</div>
<div v-else-if="name === 'Cathy'">Cathy입니다</div>
<div v-else>아무도 아닙니다.</div>
```



#### 여러 요소에 대한 v-if 적용

- v-if는 directive이기 때문에 단일 요소에만 연결 가능
- 이 경우 template 요소에 v-if를 사용하여 하나 이상의 요소에 대해 적용할 수 있음 (v-else, v-else-if 모두 적용 가능)

```html
<template v-if="name === 'Cathy'">
  <div>Cathy입니다</div>
  <div>나이는 30살입니다</div>
</template>
```



### v-show

> 표현식의 값을 T/F를 기반으로 요소의 가시성(visibility)을 전환



#### v-show 예시

- v-show 요소는 항상 렌더링되어 DOM에 남아 있음
- CSS display 속성만 전환하기 때문

```javascript
const isSeen = ref(true)
```

```html
<div v-show="isShow">v-show</div>
```

- `<div style="display: none;">v-show</div>`



### v-if vs. v-show

- v-if
  - 초기 조건이 false인 경우 아무 작업도 수행하지 않음
  - 토글 비용이 높음
- v-show
  - 초기 조건에 관계 없이 항상 렌더링
  - 초기 렌더링 비용이 더 높음
- 무언가를 매우 자주 전환해야 하는 경우에는 v-show를, 실행 중에 조건이 변경되지 않는 경우에는 v-if를 권장



## List Rendering

### v-for

> 소스 데이터를 기반으로 요소 또는 템플릿 블록을 여러 번 렌더링



#### v-for 구조

- v-for는 alias in expression 형식의 특수 구문을 사용하여 반복되는 현재 요소에 대한 별칭(alias)를 제공
- 인덱스(객체에서는 키)에 대한 별칭을 지정할 수 있음



#### v-for 예시

- 배열 반복

```javascript
const myArr = ref([
  { name: 'Alice', age: 20 },
  { name: 'Bella', age: 21 }
])
```

```html
<div v-for="(item, index) in myArr">
  {{ index }} // {{ item.name }}
</div>
```



- 객체 반복

```javascript
const myObj = ref({
  name: 'Cathy',
  age: 30
})
```

```html
<div v-for="(value, key, index) in myObj">
  {{ index }} / {{ key }} / {{ value }}
</div>
```



#### 여러 요소에 대한 v-for 적용

- template 요소에 v-for를 사용하여 하나 이상의 요소에 대해 반복 렌더링 할 수 있음

```html
<ul>
  <template v-for="item in myArr">
    <li>{{ item.name }}</li>
    <li>{{ item.age }}</li>
    <hr>
  </template>
</ul>
```



#### 중첩된 v-for

```javascript
const myInfo = ref([
  { name: 'Alice', age: 20, friends: ['Bella', 'Cathy', 'Dan'] },
  { name: 'Bella', age: 21, friends: ['Alice', 'Cathy'] }
])
```

```html
<ul v-for="item in myInfo">
  <li v-for="friend in item.friends">
    {{ item.name }} - {{ friend }}
  </li>
</ul>
```



### v-for with key

- 반드시 v-for와 key를 함께 사용함
  - 내부 컴포넌트의 상태를 일관되게 유지
  - 데이터의 예측 가능한 행동을 유지 (Vue 내부 동작 관련)
- key는 반드시 각 요소에 대한 고유한 값을 나타낼 수 있는 식별자여야 함

```javascript
let id = 0

const items = ref([
  { id: id++, name: 'Alice' },
  { id: id++, name: 'Bella' },
])
```

```html
<div v-for="item in items" :key="item.id">
  <!-- content -->
  {{ item }}
</div>
```



- 배열의 인덱스를 v-for의 key로 사용하지 말 것
  - 인덱스는 식별자가 아닌 배열의 항목 위치만 나타내기 때문에 Vue가 DOM을 변경할 때 여러 컴포넌트간 데이터 공유 시 문제 발생



### v-for with v-if

- 동일 요소에 v-for과 v-if를 함께 사용하지 않음
  - 동일한 요소에서 v-if가 v-for보다 우선순위가 더 높기 때문
  - v-if 조건은 v-for 범위의 변수에 접근할 수 없음



#### 해결법

- computed를 활용해 필터링된 목록을 반환하여 반복하도록 설정

```javascript
const completeTodos = computed(() => {
  return todos.value.filter((todo) => !todo.isComplete)
})
```

```html
<ul>
  <li v-for="todo in completeTodos" :key="todo.id">
    {{ todo.name }}
  </li>
</ul>
```



- v-for와 template 요소를 사용하여 v-if를 이동

```html
<ul>
  <template v-for="todo in todos" :key="todo.id">
    <li v-if="!todo.isComplete">
      {{ todo.name }}
    </li>
  </template>
</ul>
```



## Watchers

### watch()

> 반응성 데이터를 감시하고, 감시하는 데이터가 변경되면 콜백 함수를 호출



#### watch 구조

```javascript
watch(variable, (newValue, oldValue) => {
	// do something
})
```

- variable
  - 감시하는 변수
- newValue
  - 감시하는 변수가 변화된 값
  - 콜백 함수의 첫 번째 인자
- oldValue
  - 콜백 함수의 두 번째 인자



#### watch 예시

- 감시하는 변수에 변화가 생겼을 때 기본 동작 확인하기

```html
<button @click="count++">Add 1</button>
<p>Count: {{ count }}</p>
```

```javascript
const count = ref(0)

const countWatch = watch(count, (newValue, oldValue) => {
  console.log(`newValue: ${newValue}, oldValue: ${oldValue}`)
})
```



- 감시하는 변수에 변화가 생겼을 때 연관 데이터 업데이트하기

```html
<input v-model="message">
<p>Message length: {{ messageLength }}</p>
```

```javascript
const message = ref('')
const messageLength = ref(0)

const messageWatch = watch(message, (newValue, oldValue) => {
  messageLength.value = newValue.length
})
```



### Computed와 watchers

- Computed
  - 동작 : 의존하는 데이터 속성의 계산된 값을 반환
  - 사용 목적 : 템플릿 내에서 사용되는 데이터 연산용
  - 사용 예시 : 연산된 길이, 필터링 된 목록 계산 등

- watchers
  - 동작 : 특정 데이터 속성의 변화를 감시하고 작업을 수행
  - 사용 목적 : 데이터 변경에 따른 특정 작업 처리용
  - 사용 예시 : 비동기 API 요청, 연관 데이터 업데이트 등
- computed와 watch 모두 의존(감시)하는 원본 데이터를 직접 변경하지 않음



## Lifecycle Hooks

> Vue 인스턴스의 생애주기 동안 특정 시점에 실행되는 함수

- 개발자가 특정 단계에서 의도하는 로직이 실행될 수 있도록 함



### Lifecycle Hooks 예시

- Vue 컴포넌트 인스턴스가 초기 렌더링 및 DOM 요소 생성이 완료된 후 특정 로직을 수행하기

```javascript
const { createApp, ref, onMounted } = Vue
```

```javascript
onMounted(() => {
  console.log('mounted')
})
```



- 반응형 데이터의 변경으로 인해 컴포넌트의 DOM이 업데이트된 후 특정 로직을 수행하기

````html
<button @click="count++">Add 1</button>
<p>Count: {{ count }}</p>
<p>{{ message }}</p>
````

```javascript
const { createApp, ref, onMounted, onUpdated } = Vue
```

```javascript
const count = ref(0)
const message = ref(null)

onUpdated(() => {
  message.value = 'updated~!'
})
```



### Lifecycle Hooks 특징

- Vue는 Lifecycle Hooks에 등록된 콜백 함수들을 인스턴스와 자동으로 연결함
- 이렇게 동작하려면 hooks 함수들은 반드시 동기적으로 작성되어야 함
- 인스턴스 생애 주기의 여러 단계에서 호출되는 다른 hooks도 있으며, 가장 일반적으로 사용되는 것은 onMounted, onUpdated,  onUnmounted



## Vue Style Guide

- https://vuejs.org/style-guide/
