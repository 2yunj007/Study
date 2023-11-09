# Component State Flow

## Passing Props

### Props

> 부모 컴포넌트로부터 자식 컴포넌트로 데이터를 전달하는데 사용되는 속성

- 부모 속성이 업데이트되면서 자식으로 흐르지만 그 반대는 안 됨
- 즉, 자식 컴포넌트 내부에서 props를 변경하려고 시도해서는 안 되며 불가능
- 또한 부모 컴포넌트가 업데이트될 때마다 자식 컴포넌트의 모든 props가 최신 값으로 업데이트 됨
- 부모 컴포넌트에서만 변경하고 이를 내려 받는 자식 컴포넌트는 자연스럽게 갱신



### One-Way Data Flow

> 모든 props는 자식 속성과 부모 속성 사이에 하향식 단방향 바인딩을 형성

- 단방향인 이유: 하위 컴포넌트가 실수로 상위 컴포넌트의 상태를 변경하여 앱에서의 데이터 흐름을 이해하기 어렵게 만드는 것을 방지하기 위함



### 사전 준비

1. vue 프로젝트 생성
2. 초기 생성된 컴포넌트 모두 삭제 (App.vue 제외)
3. src/assets 내부 파일 모두 삭제
4. main.js 해당 코드 삭제
   - `import './asset/main.css'`

5. App > Parent > ParentChild 컴포넌트 관계 작성



### Props 선언

- 부모 컴포넌트에서 보낸 props를 사용하기 위해서는 자식 컴포넌트에서 명시적인 props 선언이 필요

  


#### Props 작성

- 부모 컴포넌트 Parent에서 자식 컴포넌트 ParentChild에 보낼 props 작성

```vue
<!-- Parent.vue -->
<template>
  <div>
    <ParentChild my-msg="ssafy" />
  </div>
</template>
```



#### Props 선언 2가지 방식

1. 문자열 배열을 사용한 선언

   - defineProps()를 사용하여 props를 선언

   ```vue
   <!-- ParentChild.vue -->
   <script setup>
   defineProps(['myMsg'])
   </script>
   ```

   

2. 객체를 사용한 선언

   - 객체 선언 문법의 각 객체 속성의 키는 props의 이름이 되며, 객체 속성의 값은 값이 될 데이터의 타입에 해당하는 생성자 함수(Number, String...)여야 함

   ```vue
   <!-- ParentChild.vue -->
   <script setup>
   defineProps({
     myMsg: String,
     dynamicProps: String,
   })
   </script>
   ```

   

#### prop 데이터 사용

- 템플릿에서 반응형 변수와 같은 방식으로 활용

```vue
<!-- ParentChild.vue -->
<div>
  <p>{{ myMsg }}</p>
</div>
```



- props를 객체로 반환하므로 필요한 경우 JavaScript에서 접근 가능

```vue
<script setup>
const props = defineProps({ myMsg: String })
console.log(props)	// {myMsg: 'message'}
console.log(props.myMsg)	// 'message'
</script>
```



#### 한 단계 더 prop 내려 보내기

- ParentChild 컴포넌트를 부모로 갖는 ParentGrandChild 컴포넌트 생성 및 등록
- ParentChild 컴포넌트에서 Parent로부터 받은 prop인 myMsg를 ParentGrandChild에게 전달

```vue
<!-- ParentGrandChild.vue -->
<template>
  <div>
    <p>{{ myMsg }}</p>
  </div>
</template>

<script setup>
defineProps({
  myMsg: String,
})
</script>
```

```vue
<!-- ParentChild.vue -->
<template>
  <div>
    <p>{{ myMsg }}</p>
    <ParentGrandChild :my-msg="myMsg" />
  </div>
</template>
```



### Props 세부사항

#### Props Name Casing

- 선언 및 템플릿 참조 시 (-> camelCase)
- 자식 컴포넌트로 전달 시 (-> kebab-case)



#### Static props & Dynamic props

- 지금까지 작성한 것은 Static(정적) props
- v-bind를 사용하여 동적으로 할당된 props를 사용할 수 있음



1. Dynamic props 정의

```vue
<!-- Parent.vue -->
<template>
  <div>
    <ParentChild 
      my-msg="ssafy" 
      :dynamic-props="name" 
    />
  </div>
</template>

<script setup>
import { ref } from 'vue'

const name = ref('Alice')
</scripts>
```



2. Dynamic props 선언 및 출력

```vue
<!-- Parent.vue -->
<template>
  <div>
	<p>{{ dynamicProps }}</p>
  </div>
</template>

<script setup>
defineProps({
  myMsg: String,
  dynamicProps: String,
})
</scripts>
```




## Component Events

- 부모는 자식에게 데이터를 전달(Pass Props)하며, 자식은 자신에게 일어난 일을 부모에게 알림(Emit event)




### $emit(event, ...arg)

> 자식 컴포넌트가 이벤트를 발생시켜 부모 컴포넌트로 데이터를 전달하는 역할의 메서드



### Event 발신 및 수신

- ParentChild에서 someEvent라는 이름의 사용자 정의 이벤트를 발신

```html
<!-- ParentChild.vue -->
<button @click="$emit('someEvent')">클릭</button>
```



- ParentChild의 부모 Parent는 v-on을 사용하여 발신된 이벤트를 수신
- 수신 후 처리할 로직 및 콜백함수 호출

```html
<!-- Parent.vue -->
<ParentChild 
  @some-event="someCallback"
  my-msg="ssafy" 
  :dynamic-props="name" 
/>
```

```javascript
const someCallback = function () {
  console.log('ParentChild가 발신한 emit 이벤트를 수신했습니다.')
}
```



### emit 이벤트 선언

- defineEmits()를 사용하여 명시적으로 발신할 이벤트를 선언할 수 있음
- script에서 $emit 메서드를 접근할 수 없기 때문에 defineEmit()는 $emit 대신 사용할 수 있는 동등한 함수를 반환



- 이벤트 선언 방식으로 추가 버튼 작성 및 결과 확인

```javascript
<!-- ParentChild.vue -->
const emit = defineEmits(['someEvent'])

const buttonClick = function () {
  emit('someEvent')
}
```

```html
<button @click="buttonClick">클릭</button>
```



### emit 이벤트 인자

- ParentChild에서 이벤트를 발신하여 Parent로 추가 인자 전달하기

```javascript
<!-- ParentChild.vue -->
const emit = defineEmits(['someEvent', 'emitArgs'])

const emitArgs = function () {
  emit('emitArgs', 1, 2, 3)
}
```

```html
<button @click="emitArgs">추가 인자 전달</button>
```



- ParentChild에서 발신한 이벤트를 Parent에서 수신

```html
<!-- Parent.vue -->
<ParentChild 
  @some-event="someCallback"
  my-msg="ssafy" 
  :dynamic-props="name" 
  @emit-args="getNumbers"
/>
```

```javascript
const getNumbers = function (...args) {
  console.log(args)
}
```



### emit Event 실습

- 최하단 컴포넌트 ParentGrandChild에서 Parent 컴포넌트의 name 변수 변경 요청하기
- ParentGrandChild에서 이름 변경을 요청하는 이벤트 발신

```javascript
<!-- ParentGrandChild.vue -->
const emit = defineEmits(['updateName'])

const updateName = function () {
  emit('updateName')
}
```

```html
<button @click="updateName">이름 변경!</button>
```



- 이벤트 수신 후 이름 변경을 요청하는 이벤트 발신

```javascript
<!-- ParentChild.vue -->
const emit = defineEmits(['someEvent', 'emitArgs', 'updateName'])

const updateName = function () {
  emit('updateName')
}
```



- 이벤트 수신 후 이름 변수 변경 메서드 호출
- 해당 변수를 prop으로 받는 모든 곳에서 자동 업데이트

```html
<!-- Parent.vue -->
<ParentChild @update-name="updateName"/>
```

```javascript
const updateName = function () {
  name.value = 'Bella'
}
```



## 참고

### 정적 & 동적 props 주의사항

- 첫 번째는 정적 props로 문자열로써의 "1"을 전달
- 두 번째는 동적 props로 숫자로써의 1을 전달

```html
<!-- 1 -->
<SomeComponent num-props="1" />

<!-- 2 -->
<SomeComponent :num-props="1" />
```



### Prop 선언을 객체 선언 문법으로 권장하는 이유

- prop에 타입을 지정하는 것은 컴포넌트를 가독성이 좋게 문서화하는데 도움이 되며, 다른 개발자가 잘못된 유형을 전달할 때에 브라우저 콘솔에 경고를 출력하도록 함
- 추가로 prop에 대한 유효성 검사로써 활용 가능

```javascript
defineProps({
    // 여러 타입 허용
    propB: [String, Number],
    propC: {
        type: String,
        required: true
    },
   // 기본 값을 가지는 숫자형
    propD: {
        type: Number,
        default: 10
    },
})
```



### emit 이벤트도 객체 선언 문법으로 작성 가능

- props 타입 유효성 검사와 유사하게 emit 이벤트 또한 객체 구문으로 선언된 경우 유효성을 검사할 수 있음

```javascript
const emit = defineEmits({
    // 유효성 검사 없음
    click: null,
    // submit 이벤트 유효성 검사
    submit: ({ email, password }) => {
        if (email && password) {
            return true
        } else {
            console.warn('submit 이벤트가 옳지 않음')
            return false
        }
    }
})
const submitForm = function (email, password) {
    emit('submit', {email, password})
}
```

