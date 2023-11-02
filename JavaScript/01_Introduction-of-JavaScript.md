# Introduction of JavaScript

## DOM

> 웹 페이지(Document)를 구조화된 객체로 제공하여 프로그래밍 언어가 페이지 구조에 접근할 수 있는 방법을 제공

- **문서의 요소들을 객체로 제공하여 다른 프로그래밍 언어에서 접근하고 조작할 수 있는 방법을 제공하는 API**

- 문서 구조, 스타일, 내용 등을 변경할 수 있도록 함
- DOM에서 모든 요소, 속성, 텍스트는 하나의 객체
- 모두 document 객체의 자식으로 구성됨



### DOM 선택

#### document.querySelector()

> 제공한 선택자와 일치하는 element 한 개 선택

- 제공한 CSS selector를 만족하는 첫 번째 element 객체를 반환 (없다면 null 반환)



#### document.querySelectorAll()

> 제공한 선택자와 일치하는 여러 element를 선택

- 제공한 CSS selector를 만족하는 NodeList를 반환



### DOM 조작

#### 'classList' property (클래스 속성 조작)

> 요소의 클래스 목록을 DOMTokenList (유사 배열) 형태로 반환



##### element.classList.add()

- 지정한 클래스 값을 추가



##### element.classList.remove()

- 지정한 클래스 값을 제거

  

##### element.classList.toggle()

- 클래스가 존재한다면 제거하고 false를 반환
- 존재하지 않으면 클래스를 추가하고 true 반환



#### 일반 속성 조작

##### Element.getAttribute()

- 해당 요소에 지정된 값을 반환 (조회)



##### Element.setAttribute(name, value)

- 지정된 요소의 속성 값을 설정
- 속성이 이미 있으면 기존 값을 갱신 (그렇지 않으면 지정된 이름과 값으로 새 속성이 추가)



##### Element.removeAttribute()

- 요소에서 지정된 이름을 가진 속성 제거



#### 'textContent' property (HTML 콘텐츠 조작)

> 요소의 텍스트 콘텐츠를 표현

```javascript
const h1Tag = document.querySelector('.heading')
console.log(h1Tag)
console.log(h1Tag.textContent)

// h1 요소의 콘텐츠를 수정
h1Tag.textContent = '싸피'
console.log(h1Tag.textContent)
```



#### DOM 요소 조작 메서드

##### document.createElement(tagName)

- 작성한 tagName의 HTML 요소를 생성하여 반환



##### Node.appendChild()

- 한 Node를 특정 부모 Node의 자식 NodeList 중 마지막 자식으로 삽입
- 추가된 Node 객체를 반환



##### Node.removeChild()

- DOM에서 자식 Node를 제거
- 제거된 Node를 반환



#### style 조작

> 해당 요소의 모든 style 속성 목록을 포함하는 속성

```javascript
const pTag = document.querySelector('p')
console.log(pTag)
console.log(pTag.style)
pTag.style.color = 'crimson'
pTag.style.fontSize = '3rem'
pTag.style.border = '1px solid black'
```

