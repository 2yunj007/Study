# Fundamentals of Bootstrap

## Bootstrap

> CSS 프론트엔드 프레임워크(Toolkit)

- 미리 만들어진 다양한 디자인 요소들을 제공하여 웹 사이트를 빠르고 쉽게 개발할 수 있도록 함
- 브라우저 간의 디자인 차이를 없애기 위해 CSS를 reset 함 (불필요한 요소 제거)
  - Bootstrap에 그러한 코드가 내장되어 있음



### 개요

#### Bootstrap 사용해 보기

1. Bootstrap 공식 문서

   - https://getbootstrap.com/

2. Docs -> Introduction -> Quick start

3. 2번 "Include Bootstrap's CSS and JS" 코드 확인 및 가져오기

   - https://getbootstrap.com/docs/5.3/getting-started/introduction/#quick-start

   - head와 body에 bootstrap CDN이 포함된 코드 블록



#### Bootstrap 기본 사용법

```html
<p class="mt-5">Hello, world!</p>
```

`mt-5` : {property}{sides}-{size}



##### Bootstrap에서 클래스 이름으로 Spacing을 표현하는 방법

- Bootstrap에는 특정한 규칙이 있는 클래스 이름으로 이미 스타일 및 레이아웃이 작성되어 있음

| Property                    | Sides                                                        | Size                                                         |
| --------------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| m : margin<br />p : padding | t : top<br />b: botton<br />s : left<br />e : right<br />y : top, bottom<br />x : left, right<br />blank : 4 sides | 0 : 0 rem, 0px<br />1 : 0.25rem, 4px<br />2 : 0.5rem, 8px<br />3 : 1rem, 16px<br />4 : 1.5rem, 24px<br />5 : 3rem, 48px<br />auto : auto, auto |



### Typography

- 제목, 본문 텍스트, 목록 등

- Display heading : 기존 Heading보다 더 눈에 띄는 제목이 필요할 경우 (더 크고 약간 다른 스타일)
- Inline text elements : HTML inline 요소에 대한 스타일
- Lists : HTML list 요소에 대한 스타일



### Colors

- Text, Border, Background 및 다양한 요소에 사용하는 Bootstrap의 색상 키워드



#### Bootstrap 실습

- 너비와 높이가 각각 200px인 정사각형 작성하기

```html
<!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="UTF-8">
  <meta http-equiv="X-UA-Compatible" content="IE=edge">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Document</title>
  <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.1/dist/css/bootstrap.min.css" rel="stylesheet"
    integrity="sha384-4bw+/aepP/YC94hEpVNVgiZdgIC5+VKNBQNGCHeKRQN+PtmoHDEXuppvnDJzQIu9" crossorigin="anonymous">
  <style>
    .box {
      width: 200px;
      height: 200px;
    }
  </style>
</head>

<body>
  <div class="box border border-dark bg-info"></div>

  <script src="https://cdn.jsdelivr.net/npm/bootstrap@5.3.1/dist/js/bootstrap.bundle.min.js"
    integrity="sha384-HwwvtgBNo3bZJJLYd8oVXjrBZt8cqVSpeBNS5n7C8IVInixGAoxmnlMuBnhbgrkm"
    crossorigin="anonymous"></script>
</body>
</html>
```



### Component

> Bootstrap에서 제공하는 UI 관련 요소 (버튼, 네비게이션 바, 카드, 폼, 드롭다운 등)

- Alerts, Badges, Buttons, Cards, Navbar...
- 일관된 디자인을 제공하여 웹 사이트 구성 요소를 구축하는 데 유용하게 활용



## Semantic Web

> 웹 데이터를 의미론적으로 구조화된 형태로 표현하는 방식

- "이 요소가 시각적으로 어떻게 보여질까?" -> "이 요소가 가진 목적과 역할은 무엇일까?"

- 태그에 의미를 부여



### Semantic in HTML

`<h1>Heading</h1>`

- 페이지 최상위 제목 의미를 제공하는 semantic 요소 h1
- 브라우저에 의해 제목처럼 보이도록 스타일이 지정됨



#### HTML Semantic Element

> 기본적인 모양과 기능 이외에 의미를 가지는 HTML 요소

- **검색엔진** 및 개발자가 웹 페이지 콘텐츠를 이해하기 쉽도록
- header, nav, main, article, section, aside, footer

![HTML Semantic Elements](https://www.w3schools.com/html/img_sem_elements.gif)



### Semantic in CSS

#### OOCSS(Object Oriented CSS)

> 객체 지향적 접근법을 적용하여 CSS를 구성하는 방법론

- CSS를 효율적이고 유지 보수가 용이하게 작성하기 위한 일련의 가이드라인 (CSS 방법론)



**OOCSS 기본 원칙**

1. 구조와 스킨을 분리
   - 구조와 스킨을 분리함으로써 재사용 가능성을 높임
   - 모든 버튼의 공통 구조를 정의 + 각각의 스킨(배경색과 폰트 색상)을 정의
     - `blue-button` -> `button-blue`

2. 컨테이너와 콘텐츠를 분리
   - 객체에 직접 적용하는 대신 객체를 둘러싸는 컨테이너에 스타일을 적용
   - 스타일을 정의할 때 위치에 의존적인 스타일을 사용하지 않도록 함
   - 콘텐츠를 다른 컨테이너로 이동시키거나 재배치할 때 스타일이 깨지는 것을 방지



## 참고

### CDN(Content Delivery Network)

> 지리적 제약 없이 빠르고 안전하게 콘텐츠를 전송할 수 있는 전송 기술

- 서버와 사용자 사이의 물리적인 거리를 줄여 콘텐츠 로딩에 소요되는 시간을 최소화 (웹 페이지 로드 속도를 높임)
- 지리적으로 사용자와 가까운 CDN 서버에 콘텐츠를 저장해서 사용자에게 전달



#### Bootstrap CDN

1. Bootstrap 홈페이지 - Download - "Compiled CSS and JS" Download
2. CDN을 통해 가져오는 bootstrap css와 js 파일을 확인
3. bootstrap.css 파일을 참고하여, 현재까지 작성한 클래스에 적용된 스타일을 직접 확인



### Bootstrap을 사용하는 이유

- 가장 인기 있고 잘 정립된 CSS 프레임워크
- 사전에 디자인된 다양한 컴포넌트 및 기능
  - 빠른 개발과 유지보수
- 손쉬운 반응형 웹 디자인 구현
- 커스터마이징(Customizing) 용이
- 크로스 브라우징(Cross browsing) 지원
  - 모든 주요 브라우저에서 작동하도록 설계되어 있음



### 의미론적인 마크업의 이점

- 검색엔진 최적화(SEO)
  - 검색엔진이 해당 웹 사이트를 분석하기 쉽게 만들어서 검색 순위에 여양을 줌
- 웹 접근성(Web Accessibility)
  - 시각 장애 사용자가 스크린 리더기로 웹 페이지를 사용할 때 추가적으로 도움