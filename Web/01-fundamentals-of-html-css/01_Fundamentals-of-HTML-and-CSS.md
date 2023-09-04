# Fundamentals of HTML and CSS

## 웹 소개

### 용어

- **World Wide Web**
  - 인터넷으로 연결된 컴퓨터들이 정보를 공유하는 거대한 정보 공간

- **Web**
  - Web site, Web application 등을 통해 사용자들이 정보를 검색하고 상호 작용하는 기술

- **Web site**
  - 인터넷에서 여러 개의 Web page가 모인 것으로, 사용자들에게 정보나 서비스를 제공하는 공간

- **Web page**
  - HTML, CSS 등의 웹 기술을 이용하여 만들어진, 'Web site'를 구성하는 하나의 요소



### Web page 구성 요소

- House
  - Steel Frame "Structure"
  - Paint "Styling"
  - Turn on light "Behavior"

- Web page
  - HTML "Structure"
  - CSS "Styling"
  - Javascript "Behavior"



## 웹 구조화

### HTML(HyperText Markup Language)

> 웹 페이지의 의미와 구조를 정의하는 언어

- **HyperText**
  - 웹 페이지를 다른 페이지로 연결하는 링크
  - 참조를 통해 사용자가 한 문서에서 다른 문서로 즉시 접근할 수 있는 텍스트
- **Markup Language**
  - 태그 등을 이용하여 문서나 데이터의 구조를 명시하는 언어
  - ex) HTML, Markdown



### Structure of HTML

#### HTML 구조

- `<!DOCTYPE html>`
  - 해당 문서가 html로 문서라는 것을 나타냄

- `<html></html>`
  - 전체 페이지의 콘텐츠를 포함
- `<meta>`
  - 닫는 태그가 필요 없음
- `<title></title>`
  - 브라우저 팁 및 즐겨찾기 시 표시되는 제목으로 사용
- `<head></head>`
  - HTML 문서에 관련된 설명, 설정 등
  - 사용자에게 보이지 않음
- `<body></body>`
  - 페이지에 표시되는 모든 콘텐츠



#### HTML Element(요소)

- 하나의 요소는 여는 태그와 닫는 태그 그리고 그 안의 내용으로 구성됨
- 닫는 태그는 태그 이름 앞에 슬래시가 포함되며 닫는 태그가 없는 태그도 존재



#### HTML Attributes(속성)

**규칙**

- 속성은 요소 이름과 속성 사이에 공백이 있어야 함
- 하나 이상의 속성들이 있는 경우에는 속성 사이의 공백으로 구분함
- 속성 값은 열고 닫는 따옴표로 감싸야 함

**목적**

- 나타내고 싶지 않지만 **추가적인 기능, 내용**을 담고 싶을 때 사용
- CSS에서 해당 **요소를 선택**하기 위한 값으로 활용됨



#### HTML 구조 예시

```html
<!DOCTYPE html>
<html lang="en"> 
<head>
  <meta charset="UTF-8">
  <title>My page</title>
</head>
<body>
  <p>This is my page</p>
  <a href="https://www.google.co.kr/">google</a>
  <img src="images/sample.png" alt="sample-img">
  <img src="https://random.imagecdn.app/500/150/" alt="random-image">
</body>
</html>
```



### Text Structure

- HTML의 주요 목적 중 하나는 텍스트의 구조와 의미를 제공하는 것

- `<h1>Heading</h1>` : 예를 들어 h1 요소는 단순히 텍스트를 크게만 만드는 것이 아닌 현재 문서의 최상위 제목이라는 의미를 부여하는 것



#### 대표적인 HTML Text structure

- Heading & Paragraphs
  - h1~6, p
- Lists
  - ol, ul, li
- Emphasis & Importance
  - em, strong




#### HTML Text structure 예시

```html
<body>
  <h1>main heading</h1>
  <h2>sub heading</h2>
  <p>this is my page</p>
  <p>this is <em>empgasus</em></p> <!--이탤릭체-->
  <p>Hi my <strong>name is</strong> air</p> <!--볼드체-->
  <ol>  <!--list-->
    <li>파이썬</li> <!--들여쓰기는 구조 파악을 위함, 기능과는 연관 X-->
    <li>알고리즘</li>
    <li>웹</li>
  </ol>
</body>
</html>
```



## 웹 스타일링

### CSS(Cascading Style Sheet)

> 웹 페이지의 디자인과 레이아웃을 구성하는 언어



#### CSS 구문

<img src="https://velog.velcdn.com/images%2Fjin0106%2Fpost%2Ff975ee33-64c4-499e-b8c5-a29235a0e66b%2FScreen%20Shot%202021-08-02%20at%2011.11.49%20AM.png" alt="img" style="zoom: 33%;" />



#### CSS 적용 방법

- **인라인(Inline) 스타일**
  - HTML 요소 안에 style 태그에 작성
  - 가장 권장되지 않는 방식
    - CSS와 HTML 구조 정보가 혼합되어 작성되기 때문에 코드를 이해하기 어렵게 만듦

- **내부(Internal) 스타일 시트**
  - head 태크 안에 style 태그에 작성
- **외부(Extenal) 스타일 시트**
  - 별도의 CSS 파일 생성 후 HTML link 태그를 사용해 불러오기
  - 재사용성 때문에 가장 권장되는 방식



### CSS Selectors(선택자)

> HTML 요소를 선택하여 스타일을 적용할 수 있도록 하는 선택자



#### CSS Selectors 특징

- **전체 선택자(*)**
  - HTML 모든 요소를 선택
- **요소 선택자**
  - 지정한 모든 태그를 선택

- **클래스 선택자('.' (dot))**
  - 주어진 클래스 속성을 가진 모든 요소를 선택
  - 스타일 재사용 가능
  - 대부분 클래스 선택자 사용

- **아이디 선택자('#')**
  - 주어진 아이디 속성을 가진 요소 선택
  - 문서에는 주어진 아이디를 가진 요소가 하나만 있어야 함
- **자손 결합자(" " (space))**
  - 첫 번째 요소의 자손 요소들 선택
  - ex) p span은 <p> 안에 있는 모든 <span>를 선택 (하위 레벨 상관없이)
- **자식 결합자(">")**
  - 첫 번쟤 요소의 직계 자식만 선택
  - ex) ul > li은 <ul> 안에 있는 모든 <li>를 선택 (한 단계 아래 자식들만)

```html
  <style>
    /* 전체 선택자 */
    * { 
      color: red;
    }
    /* 타입 선택자 */
    /* 모든 h2 요소의 텍스트 색을 주황색으로 변경*/
    h2 {
      color: orange;
    }

    h3, h4 {
      color: blue;
    }
    /* 클래스 선택자 */
    .green {
      color: green;
    }
    /* id 선택자 */
    #purple {
      color: purple;
    }
    /* 자식 결합자 */
    .green > span {
      font-size: 50px;
    }
    /* 자손 결합자 */
    .green li {
      color: brown;
    }

  </style>
</head>

<body>
  <h1 class="green">Heading</h1>
  <h2>선택자</h2>
  <h3>연습</h3>
  <h4>반가워요</h4>
  <p id="purple">과목 목록</p>
  <ul class="green">
    <li>파이썬</li>
    <li>알고리즘</li>
    <li>웹
      <ol>
        <li>HTML</li>
        <li>CSS</li>
        <li>PYTHON</li>
      </ol>
    </li>
  </ul>
  <p class="green">Lorem, <span>ipsum</span> dolor.</p>
</body>

</html>
```



### Specificity(우선순위)

> 동일한 요소에 적용 가능한 같은 스타일을 두 가지 이상 작성했을 때 어떤 규칙이 적용되는지 결정하는 것



#### Cascode(계단식)

- 동일한 우선순위를 갖는 규칙이 적용될 때 CSS에서 마지막에 나오는 규칙이 사용됨
- 다음과 같은 코드가 작성됐을 때 h1 태그 내용의 색은 purple이 적용됨

```html
h1 {
color: red;
}

h1 {
color: purple;
}
```

- 동일한 h1 태그에 다음과 같이 스타일이 작성된다면 h1 태그 내용의 색은 red가 적용됨

```html
.make-red {
color: red;
}

h1 {
color: purple;
}
```



#### 우선순위가 높은 순

1. Imporance
   - !important
2. Inline 스타일
3. 선택자
   - id 선택자 > **class 선택자** > 요소 선택자

4. 소스 코드 순서



**!important**

> 다른 우선순위 규칙보다 우선하여 적용하는 키워드

- Cascade의 구조를 무시하고 강제로 스타일을 적용하는 방식이므로 사용을 권장하지는 않음



**속성은 되도록 'class'만 사용할 것**

- id, 요소 선택자 등 여러 선택자들과 함께 사용할 경우 우선순위 규칙에 따라 예기치 못한 스타일 규칙이 적용되어 전반적인 유지보수가 어려워지기 때문
- 문서에서 단 한 번 유일하게 적용될 스타일의 경우에만 id 선택자 사용을 고려



```html
  <style>
    h2 {
      color: darkviolet !important
    }

    p {
      color: blue;
    }

    .orange{
      color: orange;
    }

    .green {
      color: green;
    }

    #red {
      color: red;
    }
  </style>
</head>

<body>
  <p>1</p>
  <p class="orange">2</p>
  <p class="green orange">3</p> <!--소스코드에 작성된 순서대로 우선순위가 부여되므로 green-->
  <p id="red">4</p> <!--id가 우선순위가 더 높으므로 red-->
  <p>5</p>
  <h2 id="red">6</h2> <!--!important > id이므로 darkviolet-->
  <p>7</p>
  <h2>8</h2>
</body>

</html>
```



### CSS 상속

- 기본적으로 CSS는 상속을 통해 부모 요소의 속성을 자식에게 상속해 재사용성을 높임



#### 상속 여부

- 상속되는 속성
  - Text 관련 요소(font, color, text-align), opacity, visibility 등
- 상속되지 않는 특성
  - Box model 관련 요소(width, height, border, box-sizing...)
  - position 관련 요소(position, top/right/bottom/left,  z-index) 등
- CSS 상속 여부는 MDN 문서에서 확인하기
  - MDN 각 속성별 문서 하단에서 상속 여부를 확인할 수 있음



```html
  <style>
    .parent {
      /* 상속 O */
      color: red;

      /* 상속 X */
      border: 1px solid black;
    }
  </style>
</head>

<body>
  <ul class="parent">
    <li class="child">Hello</li>
    <li class="child">Bye</li>
  </ul>
</body>
```





## 참고

### HTML 관련 사항

- **요소(태그) 이름**은 대소문자를 구분하지 않지만 **소문자** 사용을 권장
- **속성의 따옴표**는 작은 따옴표와 큰 따옴표를 구분하지 않지만 **큰 따옴표** 권장
- HTML은 프로그래밍 언어와 달리 에러를 반환하지 않기 때문에 작성 시 주의
- **MDN** : 웹에서 가장 공신력 높은 표준 문서 (웹 공부할 때는 검색어에 mdn 붙이기)

- `! + TAP` : body를 바로 작성할 수 있는 구조가 자동 완성 됨
- `ctrl + B` : HTML 문서 열기