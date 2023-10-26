# Controlling event

> 무언가 일어났다는 신호, 사건

- 모든 DOM 요소는 이러한 event를 만들어 냄



## Event object

- DOM에서 이벤트가 발생했을 때 생성되는 객체
- 이벤트 종류
  - mouse, input, keyboard, touch
  - https://developer.mozilla.org/en-US/docs/Web/API/Event



## Event handler

> 이벤트가 발생했을 때 실행되는 함수

- 사용자의 행동에 어떻게 반응할지를 JavaScript 코드로 표현한 것



### .addEventListener()

> 대표적인 이벤트 핸들러 중 하나

- 특정 이벤트를 DOM 요소가 수신할 때마다 **콜백 함수**를 호출
- `EvenvtTarget.addEventListener(type, handler)`
  	- EventTarget: DOM 요소
  	- type: 수신할 이벤트
  	- handler: 콜백 함수
- 대상에 특정 이벤트가 발생하면, 지정한 이벤트를 받아 할 일을 등록함



**type**

- 수신할 이벤트 이름
- 문자열로 작성 (ex. 'click')



**handler**

- 발생한 이벤트 객체를 수신하는 콜백 함수
- 콜백 함수는 발생한 Event object를 유일한 매개변수로 받음



#### 활용

- "버튼을 클릭하면 버튼 요소 출력하기"
  - 버튼에 이벤트 처리기를 부착하여 클릭 이벤트가 발생하면 이벤트가 발생한 버튼 정보 출력

- 요소에 addEventListener를 부착하게 되면 내부의 this 값은 대상 요소를 가리키게 됨 (event 객체의 currentTarget 속성 값과 동일)

- 발생한 이벤트를 나타내는 Event 객체를 유일한 매개변수로 받음
- 아무것도 반환하지 않음