# Bootstrap Grid system

## 개요

### Bootstrap Grid system

> 웹 페이지의 레이아웃을 조정하는 데 사용되는 12개의 컬럼으로 구성된 시스템

- 반응형 디자인을 지원해 웹 페이지를 모바일, 태블릿, 데스크탑 등 다양한 기기에서 적절하게 표시할 수 있도록 도움
- 12개인 이유는 12가 적당한 크기이면서 약수(경우의 수)가 많은 수이기 때문



## Grid system 클래스와 기본 구조

- Container : Column들을 담고 있는 공간
- Column : 실제 콘텐츠를 포함하는 부분
- Gutter : 컬럼과 컬럼 사이의 여백 영역

- 1개의 row 안에 12칸의 column 영역이 구성
- 각 요소는 12칸 중 몇 개를 차지할 것인지 지정됨



### Grid System 실습

#### 기본

```html
<h2 class="text-center">Basic</h2>
  <div class="container"> <!-- 양쪽에 여백 생성됨 -->
    <div class="row">
      <div class="box col">col</div> <!-- 3등분 col-4 -->
      <div class="box col">col</div> <!-- 모두 똑같은 너비를 주는 경우, 숫자를 생략해도 됨 -->
      <div class="box col">col</div>
    </div>
    <div class="row">
      <div class="box col-4">col-4</div>
      <div class="box col-4">col-4</div>
      <div class="box col-4">col-4</div>
    </div>
    <div class="row">
      <div class="box col-2">col-2</div>
      <div class="box col-8">col-8</div>
      <div class="box col-4">col-2</div> <!-- 12칸을 넘어가면 다음 행으로 넘어감 -->
    </div>
  </div>
```



#### 중첩(Nesting)

```html
<h2 class="text-center">Nesting</h2>
  <div class="container">
    <div class="row">
      <div class="box col-4">col-4</div>
      <div class="box col-8">
        <div class="row"> <!-- 중첩된 부분 -->
          <div class="box col-6">col-6</div>
          <div class="box col-6">col-6</div>
          <div class="box col-6">col-6</div>
          <div class="box col-6">col-6</div>
        </div>
      </div>
    </div>
  </div>
```



#### 상쇄(Offset)

```html
<h2 class="text-center">Offset</h2>
  <div class="comtainer">
    <div class="row">
      <div class="box col-4">col-4</div>
      <div class="box col-4 offset-4">col-4 offset-4</div>
    </div>
    <div class="row">
      <div class="box col-3 offset-3">col-3 offset-3</div>
      <div class="box col-3 offset-3">col-3 offset-3</div>
    </div>
    <div class="row">
      <div class="box col-6 offset-3">col-6 offset-3</div>
    </div>
  </div>
```



#### Gutters

- Grid system에서 column 사이에 여백 생성
- x축은 padding, y축은 margin으로 여백 생성

```html
<h2 class="text-center">Gutters(gx-0)</h2>
  <div class="container">
    <div class="row gx-0"> <!-- col 사이의 공간 제거 -->
      <div class="col-6">
        <div class="box">col</div>
      </div>
      <div class="col-6">
        <div class="box">col</div>
      </div>
    </div>
  </div>

  <br>

  <h2 class="text-center">Gutters(gy-5)</h2>
  <div class="container">
    <div class="row gy-5"> <!-- col 위아래는 기본적으로 붙어 있기 때문에 띄움-->
      <div class="col-6">
        <div class="box">col</div>
      </div>
      <div class="col-6">
        <div class="box">col</div>
      </div>
      <div class="col-6">
        <div class="box">col</div>
      </div>
      <div class="col-6">
        <div class="box">col</div>
      </div>
    </div>
  </div>


  <br>

  <h2 class="text-center">Gutters(g-5)</h2>
  <div class="container">
    <div class="row g-5"> <!-- 상하좌우 조정 -->
      <div class="col-6">
        <div class="box">col</div>
      </div>
      <div class="col-6">
        <div class="box">col</div>
      </div>
      <div class="col-6">
        <div class="box">col</div>
      </div>
      <div class="col-6">
        <div class="box">col</div>
      </div>
    </div>
  </div>
```



# Grid system for responsive web

## 개요

### Responsive Web Design

> 디바이스 종류나 화면 크기에 상관없이, 어디서든 일관된 레이아웃 및 사용자 경험을 제공하는 디자인 기술

- Bootstrap grid system에서는 12개의 column과 6개의 breakpoints를 사용하여 반응형 웹 디자인 구현

 

## Grid system breakpoints

> 웹 페이지를 다양한 화면 크기에서 적절하게 배치하기 위한 분기점

- 화면 너비에 따라 6개의 분기점 제공 (xs, sm, md, lg, xl, xxl)
- 각 breakpoints마다 설정된 최대 너비 값 이상으로 화면이 커지면 grid system 동작이 변경됨
- ex) `col-sm-4` : 576px~767px일 때 4칸을 차지

<img src="https://wowslider.com/posts/data/upload/2017/05/grid-options.jpg" alt=" Precisely how  elements of the Bootstrap grid system  perform" style="zoom:67%;" />



### Breakpoint 실습

#### Breakpoint

```html
<h2 class="text-center">Breakpoints</h2>
  <div class="container">
    <div class="row">
      <div class="box col-12 col-sm-6 col-md-2 col-lg-4">
        col
      </div>
      <div class="box col-12 col-sm-6 col-md-8 col-lg-4">
        col
      </div>
      <div class="box col-12 col-sm-6 col-md-2 col-lg-4">
        col
      </div>
      <div class="box col-12 col-sm-6 col-md-12 col-lg-12">
        col
      </div>
    </div>
```



#### Breakpoints + offset

- offset은 사이즈가 커져도 유지됨을 주의
- `offset-sm-4`에서 준 offset을`offset-md-0`로 제거

```html
<h2 class="text-center">Breakpoints + offset</h2>
    <div class="row g-4">
      <div class="box col-12 col-sm-4 col-md-6">
        col
      </div>
      <div class="box col-12 col-sm-4 col-md-6">
        col
      </div>
      <div class="box col-12 col-sm-4 col-md-6">
        col
      </div>
      <div class="box col-12 col-sm-4 offset-sm-4 col-md-6 offset-md-0">
        col
      </div>
    </div>
```



#### Midia Query로 작성된 Grid system의 breakpoints

- 내부에 클래스 생성

```css
// Small devices (landscape phones, 576px and up)
@media (min-width: 576px) { ... }

// Medium devices (tablets, 768px and up)
@media (min-width: 768px) { ... }

// Large devices (desktops, 992px and up)
@media (min-width: 992px) { ... }

// X-Large devices (large desktops, 1200px and up)
@media (min-width: 1200px) { ... }

// XX-Large devices (larger desktops, 1400px and up)
@media (min-width: 1400px) { ... }
```



# 참고

## The Grid System

- CSS가 아닌 편집 디자인에서 나온 개념으로 구성 요소를 잘 배치해서 시각적으로 좋은 결과물을 만들기 위함
- 기본적으로 안쪽에 있는 요소들의 오와 열을 맞추는 것에서 기인
- 정보 구조와 배열을 체계적으로 작성하여 정보의 질서를 부여하는 시스템



## Grid cards

- row-cols 클래스를 사용하여 행당 표시할 열(카드) 수를 손쉽게 제어할 수 있음
- 즉, 칸 수를 배분

```html
<h2 class="text-center">Grid Cards</h2>
  <div class="container">
    <div class="row row-cols-1 row-cols-sm-3 row-cols-md-2 g-4">
      <div class="col">
        <div class="card">
          <div class="card-body">
            <h5 class="card-title">Card title</h5>
            <p class="card-text">...</p>
          </div>
        </div>
      </div>
      <div class="col">...</div>
      <div class="col">...</div>
        <div class="col">...</div>
    </div>
  </div>
```

