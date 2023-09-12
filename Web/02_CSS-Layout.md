# CSS Layout

## CSS Box model

> 모든 HTML 요소를 사각형 박스로 표현하는 개념



### Box 구성 요소

내용(content), 안쪽 여백(padding), 테두리(border), 외부 간격(margin)으로 구성되는 개념

- **Content** : 콘텐츠가 표시되는 영역
- **Margin** : 이 박스와 다른 요소 사이의 공백. 가장 바깥쪽 영역
- **Border** : 콘텐츠와 패딩을 감싸는 테두리 영역
- **Padding** : 콘텐츠 주위에 위치하는 공백 영역

![CSS3 Box Model | PoiemaWeb](https://poiemaweb.com/img/box-model-detail.png)

```html
<style>
    .box1 {
      width: 200px;
      padding-left: 25px;
      padding-bottom: 25px;
      border-width: 3px;
      border-color: black;
      border-style: solid;
      margin-left: 25px;
      margin-bottom: 50px;
    }

    .box2 {
      width: 200px;
      border: 3px black dashed;
      /* auto : 정해진 방향으로 margin을 다 나눠줌 */
      /* 왼쪽/오른쪽에 모두 적용하면 가운데 정렬 */
      /* margin-left: auto;  
      margin-right: auto;  */
      
      /* 상하/좌우 */
      margin: 100px auto;
      padding: 25px 50px;
    }
  </style>
```



### width & height 속성

- 요소의 너비와 높이를 지정

- 이때 지정되는 요소의 너비와 높이는 콘텐츠 영역을 대상으로 함

  

### box-sizing 속성

- CSS는 border가 아닌 content의 크기를 width 값으로 지정

```html
.content-box {
	box-sizing: content-box;
    }

.border-box {
	box-sizing: border-box;
}
```

<img src="https://www.tabmode.com/homepage/images/box_size_content.png" alt="img" style="zoom: 67%;" />

<img src="https://www.tabmode.com/homepage/images/box_size_border.png" alt="img" style="zoom: 67%;" />



## 박스 타입

### Normal flow

- CSS를 적용하지 않았을 경우 웹페이지 요소가 기본적으로 배치되는 방향



### block 타입 특징

- 항상 새로운 행으로 나뉨
- width와 height 속성을 사용하여 너비와 높이를 지정할 수 있음
- 기본적으로 width 속성을 지정하지 않으면 박스는 inline 방향으로 사용 가능한 공간을 모두 차지함 (너비를 사용 가능한 공간의 100%로 채우는 것)
- 대표적인 block 타입 태그
  - h1~6, p, div



### inline 타입 특징

- 새로운 행으로 나뉘지 않음
- width와 height 속성을 사용할 수 없음
- 수직 방향
  - padding, margins, borders가 적용되지만 다른 요소를 밀어낼 수는 없음
- 수평 방향
  - padding, margins, borders가 적용되어 다른 요소를 밀어낼 수 있음
- 대표적인 inline 타입 태그
  - a, img, span



### 속성에 따른 수평 정렬

- 왼쪽 정렬

  - `margin-right: auto;`
  - `text-align: left;`

- 오른쪽 정렬

  - `margin-left: auto;`
  - `text-align: right;`

- 가운데 정렬

  - `margin-right: auto;`

    `margin-left: auto;`

  - `text-align: center;`



## 기타 display 속성

### inline-block

- inline과 block 요소 사이의 중간 지점을 제공하는 display 값
- block 요소의 특징을 가짐
  - width 및 height 속성 사용 가능
  - padding, margin 및 border로 인해 다른 요소가 밀려남
- **요소가 줄 바꿈 되는 것을 원하지 않으면서(inline) 너비와 높이를 적용**하고 싶은 경우에 사용(block)



### none

- 요소를 화면에 표시하지 않고, 공간조차 부여되지 않음
- 예를 들어, 어떠한 버튼을 누르면 사라지게 하고 생겨나게 하는 동작을 할 수 있도록 함



## CSS Layout Position

### CSS Layout

> 각 요소의 위치와 크기를 조정하여 웹 페이지의 디자인을 결정하는 것

- Display, Position, Float, Flexbox 등



### CSS Position

> 요소를 Normal Flow에서 제거하여 다른 위치고 배치하는 것

- 다른 요소 위에 올리기, 화면의 특정 위치에 고정시키기 등



#### static

- 기본 값
- 요소를 Normal Flow에 따라 배치



#### relative

- 요소를 Normal Flow에 따라 배치
- 자기 자신을 기준으로 이동
- 요소가 차지하는 공간은 static일 때와 같음



#### absolute

- 요소를 Normal Flow에서 제거
- 가장 가까운 relative 부모 요소를 기준으로 이동
- 문서에서 요소가 차지하는 공간이 없어짐



#### fixed

- 요소를 Normal Flow에서 제거
- 현재 화며 영역(viewport)을 기준으로 이동
- 문서에서 요소가 차지하는 공간이 없어짐



#### sticky

- 요소를 Normal Flow에 따라 배치
- 요소가 일반적인 문서 흐름에 따라 배치되다가 스크롤이 특정 임계점에 도달하면 그 위치에 고정됨(fixed)
- 만약 다음 sticky 요소가 나오면 다음 sticky 요소가 이전 sticky 요소의 자리를 대체
  - 이전 sticky 요소가 고정되어 있던 위치와 다음 sticky 요소가 고정되어야 할 위치가 겹치게 되기 때문



### z-index

> 요소가 겹쳤을 때 어떤 요소 순으로 위에 나타낼지 결정

- 정수 값을 사용해 z축 순서를 지정
- 더 큰 값을 가진 요소가 작은 값의 요소를 덮음



## CSS Layout Flexbox

> 요소를 행과 열 형태로 배치하는 1차원 레이아웃 방식 (공간 배열 & 정렬)



### Flexbox 기본 사항

- **Flex Container**

  - `display: flex;` 혹은 `display: inline-fllex;` 가 설정된 부모 요소
  - 이 컨테이너의 1차 자식 요소들이 Flex item이 됨

  - 행/열 축을 가짐 (main axis, cross axis)
    - 기본값의 시작은 왼쪽 (main start)
    - 교차축의 시작은 위쪽 (cross start)
  - 요소가 쌓이는 방향은 바꿀 수 있음 (축이 바뀔 수 있음)
  - flexbox 속성 값들을 사용하여 자식 요소 Flex Item들을 배치

- **Flex item**

  - Flex Container 내부에 레이아웃 되는 항목

- **main axis(주 축)**

  - flex item들이 배치되는 기본 축
  - main start에서 시작하여 main end방향으로 배치
  - 아무것도 지정하지 않으면 수평 축이 main 축

- **cross axis(교차 축)**

  - main axis에 수직인 축 (그러므로 main 축만 알면 됨)
  - cross start에서 시작하여 cross end 방향으로 배치

![flex-direction: row일 경우](https://armadillo-dev.github.io/assets/images/flex-direction-row.png)




### 레이아웃 구성

- **Flex Container 지정**
  - flex item은 기본적으로 행으로 나열
  - flex item은 주축의 시작 선에서 시작
  - flex item은 교차축의 크기를 채우기 위해 늘어남
- **flex-direction 지정**
  - flex item이 나열되는 방향을 지정
  - `flex-direction: column;`을 통해 column으로 지정할 경우 주 축이 변경됨
  - -reverse로 지정하면 시작 선과 끝 선이 서로 바뀜
- **flex-wrap**
  - flex item 목록이 flex container의 하나의 행에 들어가지 않을 경우 다른 행에 배치할지 여부 설정
  - nowrap : 줄바꿈을 하지 않음 (defalut 값)
  - wrap : 줄바꿈을 함
  - wrap-reverse : 자식 박스들을 역순 배치하여 줄바꿈을 함
- **justify-content**
  - 주 축을 따라 flex item과 주위에 공간을 분배
  - `flex-start`: main axis의 시작 지점부터 flex item이 시작
  - `center`: flex item이 main axis 중앙으로 정렬
  - `space-around`: flex item이 동일한 여백으로 정렬
    - container의 시작과 끝 지점에는 flex item 사이 여백의 1/2에 해당하는 여백이 들어감
  - `space-between`: flex item이 동일한 여백으로 정렬
    - `space-around`와는 다르게 flex container의 시작과 끝에는 여백이 존재하지 않음
- **align-content**
  - 교차 축을 따라 flex item과 주위에 공간을 분배
    - flex-wrap이 wrap 또는 wrap-reverse로 설정된 여러 행에만 적용됨
    - 한 줄 짜리 행에는 (flex-wrap이 nowrap으로 설정된 경우) 효과 없음
- **align-items**
  - 교차 축을 따라 flex item 행을 정렬
  - `justify-content` 속성이 main axis에 영향을 준다면, `align-items`는 cross axis 상 위치/여백을 조정
  - `flex-start`, `flex-end`, `center`는 바라보는 축만 다를뿐, `justify-content`와 동일한 기능을 수행
  - `stretch`는 flex item의 높이를 cross axis의 길이와 동일하게 세팅
- **align-self**
  - 교차 축을 따라 개별 flex item을 정렬

- **flex-grow**
  - 남는 행 여백을 비율에 따라 각 flex item에 분배
    - 아이템 컨테이너 내에는 확장하는 비율을 지정
  - `flex-grow`의 반대는 `flex-shrink`
- **flex-basis**
  - flex item의 초기 크기 값을 지정
  - 최소한의 너비를 보장받기 위해 사용
  - `flex-basis`와 width 값을 동시에 적용한 경우 `flex-basis`가 우선
  - px, em, % 등 크기를 표현하는 모든 단위 사용 가능
- **order**
  - 해당 순서에 따라 flex item의 정렬 순서가 변경
  - order 값을 기준으로 오룸차순 정렬




### Flexbox 속성

- Flex Container 관련 속성
  - display, flex-direction, flex-wrap, justify-content, align-items, align-content
- Flex Item 관련 속성
  - align-self, flex-grow, flex-basis, order
- 목적에 따른 분류
  - 배치: flex-direction, flex-wrap
  - 공간 분배 : justify-content, align-content
  - 정렬 :align-items, align-self




### 반응형 레이아웃

> 다양한 디바이스와 화면 크기에 자동으로 적응하여 콘텐츠를 최적으로 표시하는 웹 레이아웃 방식

[CSS 플렉스박스(flex)로 반응형 레이아웃 만들기](https://blogpack.tistory.com/862)



## 참고

[CSS3 Grid, Flex, Position Layout 정리](https://medium.com/@deptno/css3-grid-flex-position-layout-%EC%A0%95%EB%A6%AC-b22820120132)

[중앙 정렬 기법 모음](https://www.freecodecamp.org/korean/news/cssro-mueosideun-jungang-jeongryeolhaneun-bangbeob-div-tegseuteu-deung/)

