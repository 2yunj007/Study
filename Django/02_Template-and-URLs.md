# Template & URLs

## Django Template System

> 데이터 표현을 제어하면서, 표현과 관련된 부분을 담당



**HTML의 콘텐츠를 변수 값에 따라 바꾸고 싶다면?**

```python
def index(request):
    context = {
        'name': 'Jane'
    }
    return render(request, 'articles/index.html', context)
```

```html
<body>
  <h1>안녕!, {{name}}</h1>
</body>
```



### Django Template Language(DTL)

> Template에서 조건, 반복, 변수 등의 프로그래밍적 기능을 제공하는 시스템



#### DTL Syntax

**Variable**

- rander 함수의 세 번째 인자로 딕셔너리 데이터를 사용
- 딕셔너리 key에 해당하는 문자열이 template에서 사용 가능한 변수명이 됨
- dot(.)를 사용하여 변수 속성에 접근할 수 있음
- `{{variable}}`



[Built-in template tags and filters](https://docs.djangoproject.com/en/4.2/ref/templates/builtins/#ref-templates-builtins-filters)

**Filters**

- 표시할 변수를 수정할 때 사용
- chained가 가능하며 일부 필터는 인자를 받기도 함
- 약 60개의 buil-in template filters를 제공

- `{{variable|filter}}`
- `{{name|truncatewords:30}}`



**Tag**

- 반복 또는 논리를 수행하여 제어 흐름을 만듦
- 일부 태그는 시작과 종료 태그가 필요
- 약 24개의 built-in template tags를 제공
- `{% tag %}`
- `{% if %}` `{% endif %}`



**Comments**

- DTL에서의 주석
- `{# name #}`



``` python
# urls.py
urlpatterns = [
    path('admin/', admin.site.urls),
    path('articles/', views.index),
    path('dinner/', views.dinner),
]
```

```python
# views.py
import random

def dinner(request):
    foods = ['국밥', '국수', '카레', '탕수육',]
    picked = random.choice(foods)
    context = {
        'foods': foods,
        'picked': picked,
    }
    return render(request, 'articles/dinner.html', context)
```

```html
<!-- articles/dinner.html -->
  <p>{{ picked }} 메뉴가 선택되었습니다.</p>
  <p>{{ picked|length }} 글자입니다.</p>
  <h2>메뉴판</h2>
  <ul>
    {% for food in foods %}
      <li>{{ food }}</li>
    {% endfor %}
  </ul>
  {% if foods|length == 0 %}
    <p>메뉴가 소진되었습니다.</p>
  {% else %}
    <p>아직 메뉴가 남았습니다.</p>
  {% endif %}
```



## 템플릿 상속(Template inheritance)

> **페이지의 공통 요소를 포함**하고, **하위 템플릿이 재정의 할 수 있는 공간**을 정의하는 기본 'skeleton' 템플릿을 작성하여 상속 구조를 구축

- 기존 템플릿 구조의 한계

  - 만약 모든 템플릿에 bootstrap을 적용하려면?

  - 모든 템플릿에 bootstrap CDN을 작성해야 할까?



### 'extends' tag

- 자식(하위) 템플릿이 부모 템플릿을 확장한다는 것을 알림
- 반드시 템플릿 최상단에 작성되어야 함 (2개 이상 사용 불가)
- `{% extends "path" %}`



### 'block' tag

- 하위 템플릿에서 재정의할 수 있는 블록을 정의 (하위 템플릿이 작성할 수 있는 공간을 지정)
- `{% block name %}{% endblock name %}`



### 상속 구조 구축

- skeleton 역할을 상위 템플릿 작성

```html
<!-- articles/base.html -->
<!doctype html>
<html lang="en">
  <head>
    ...
	{% comment %} bootstrap CDN 생략 {% endcomment %}
  </head>
  <body>
    {% block content %}
    {% endblock content %}
    {% comment %} bootstrap CDN 생략 {% endcomment %}
  </body>
</html>
```

- 템플릿 추가 경로 작성

```py
TEMPLATES = [
    {
        'BACKEND': 'django.template.backends.django.DjangoTemplates',
        # 템플릿 추가 경로를 작성하는 곳
        'DIRS': [BASE_DIR / 'templates',],
        'APP_DIRS': True,
        'OPTIONS': {
            'context_processors': [
                'django.template.context_processors.debug',
                'django.template.context_processors.request',
                'django.contrib.auth.context_processors.auth',
                'django.contrib.messages.context_processors.messages',
            ],
        },
    },
]
```

- 기존 하위 템플릿의 변화
  1. extends 태그로 articles의 base.html을 상속받음

  2. 부모가 정의한 content라고 하는 공간에 내용을 채워 넣음

```html
<!-- articles/index.html -->
{% extends "articles/base.html" %}

{% block content %}
<h1>안녕!, {{ name }}</h1>
{% endblock content %}
```

```html
<!-- articles/dinner.html -->
{% extends "articles/base.html" %}

{% block content %}
<p>{{ picked }} 메뉴가 선택되었습니다.</p>
<p>{{ picked|length }} 글자입니다.</p>
<h2>메뉴판</h2>
<ul>
  {% for food in foods %}
    <li>{{ food }}</li>
  {% endfor %}
</ul>
{% if empty_list|length == 0 %}
  <p>메뉴가 소진되었습니다.</p>
{% else %}
  <p>아직 메뉴가 남았습니다.</p>
{% endif %}
{% endblock content %}
```



## HTML form (요청과 응답)

- Sending and Retrieving form data
  - HTML form element를 통해 사용자와 애플리케이션 간의 상호작용 이해하기

- HTML form은 HTTP 요청을 서버에 보내는 가장 편리한 방법



### 'form' element

- 사용자로부터 할당된 데이터를 서버로 전송

- 웹에서 사용자 정보를 입력하는 여러 방식 (text, password, checkbox 등)을 제공

- **'action' & 'method'**

  -  form의 핵심 속성 2가지

  -  "데이터를 어디(action)로 어떤 방식(method)으로 요청할지"

     - **action**

       - 입력 데이터가 전송될 URL을 지정 (목적지)

       - 만약 이 속성을 지정하지 않으면 데이터는 현재 form에 있는 페이지의 URL로 보내짐

     - **method**
- 데이터를 어떤 방식으로 보낼 것인지 정의
       
- 데이터 HTTP request methods (GET, POST)를 지정



### 'input' element

- 사용자의 데이터를 입력받을 수 있는 요소
- type 속성 값에 따라 다양한 유형의 입력 데이터를 받음

- **'name' attribute**

  - input의 핵심 속성

  - 입력한 데이터에 붙이는 이름 (key)

  - 데이터를 제출했을 때 서버는 name 속성에 설정된 값을 통해서만 사용자가 입력한 데이터에 접근할 수 있음



### Query String Parameters

- 사용자의 입력 데이터를 URL 주소에 파라미터를 통해 서버로 보내는 방법
- 문자열은 앰퍼샌드(&)로 연결된 key=value 쌍으로 구성되며, 기본 URL과 물음표(?)로 구분됨
- http://host:port/path?key=value&key=value



### fake Naver 실습

```py
# urls.py
urlpatterns = [
    path('admin/', admin.site.urls),
    path('articles/', views.index),
    path('dinner/', views.dinner),
    path('search/', views.search),
]
```

```py
# view.py
def search(request):
    return render(request, 'articles/search.html')
```

```html
<!-- articles/search.html -->
{% extends "articles/base.html" %}

{% block content %}
  <h1>fake naver</h1>
  <form action="https://search.naver.com/search.naver/" method="GET">
    <label for="message">검색어:</label>
    <input type="text" name="query" id="message">
    <input type="submit">
  </form>
{% endblock content %}
```



**Naver에서 검색 후 URL 확인**

https://search.naver.com/search.naver?query=hello

- 물음표 앞 부분은 목적지 URL
- 'query' : Input의 name
- 'hello' : input에 입력한 데이터



### form 활용

- 사용자 입력 데이터를 받아 그대로 출력하는 서버 만들기



 #### throw 로직 생성

```python
# urls.py
urlpatterns = [
    ...,
    path('throw/', views.throw),
]
```

```python
# views.py
def throw(request):
    return render(request, 'articles/throw.html')
```

```html
<!-- articels/throw.html -->
{% extends "base.html" %}

{% block content %}
  <h1>Throw</h1>
  <form action="/catch/" method="GET">
    <label for="message">메세지:</label>
    <input type="text" name="message" id="message">
    <input type="submit">
  </form>
{% endblock content %}
```



 #### catch 로직 생성

```python
# urls.py
urlpatterns = [
    ...,
    path('catch/', views.catch),
]
```

```python
# views.py
def catch(request):
    # 사용자로 부터 요청을 받아서
    # 요청에서 사용자 입력 데이터를 찾아
    # context에 저장 후 catch 템플릿에 출력
    message = request.GET.get('message')
    context = {
        'message': message,
    }
    return render(request, 'articles/catch.html', context)
```

```html
<!-- articels/catch.html -->
{% extends "base.html" %}

{% block content %}
  <h1>Throw로부터 {{ message }}를 받았습니다!</h1>
{% endblock content %}
```



### HTTP request 객체

> form으로 전송한 데이터뿐만 아니라 모든 요청 관련 데이터가 담겨 있음 (view 함수의 첫 번째 인자)



**request 객체 살펴보기**

```python
print(request)
print(type(request))
print(request.GET)
print(request.META)
print(request.GET.get('message'))
'''
<WSGIRequest: GET '/catch/?message=%EB%B0%A9%EA%B0%80%EB%B0%A9%EA%B0%80'>
<class 'django.core.handlers.wsgi.WSGIRequest'>
<QueryDict: {'message': ['방가방가']}>
{생략}
방가방가
'''
```



## Django URLs

### URL dispatcher

- URL 패턴을 정의하고 해당 패턴이 일치하는 요청을 처리할 view 함수를 연결(매핑)



## 변수와 URL

**현재 URL 관리의 문제점**

- 다음 코드와 같이 템플릿의 많은 부분이 중복되고, URL의 일부만 변경되는 상황이라면 계속해서 비슷한 URL과 템플릿을 작성해 나가야 할까?

```python
urlpatterns = [
    path('articles/1/', ...),
    path('articles/1/', ...),
    path('articles/1/', ...),
    path('articles/1/', ...),
    ...
]
```



### Variable Routing

> URL 일부에 변수를 포함시키는 것 (**변수는 view 함수의 인자로 전달**할 수 있음)



#### Variable routing 작성법

- `<path_converter:variable_name>`
  	- Path converters : URL 변수의 타입을 지정 (str, int 등 5가지 타입 지원)

```python
path('articles/<int:num>/', views.detail)
path('articles/<str:name>/', views.greeting)
```



#### Variable routing 실습 (1)

```python
# urls.py
urlpatterns = [
    # path('hello/<str:name>/', views.greeting),
    path('hello/<name>/', views.greeting),
]
```

```python
# views.py
def greeting(request, name):	# name : url에 있는 변수명을 인자로 받음
    context = {
        'name': name,
    }
    return render(request, 'articles/greeting.html', context)
```

```html
<!-- articles/greeting.html -->
{% extends "base.html" %}

{% block content %}
  <h1>안녕, {{ name }}</h1>
{% endblock content %}
```



#### Variable routing 실습 (2)

```python
# urls.py
urlpatterns = [
    path('articles/<int:num>/', views.detail),
]
```

```python
# views.py
def detail(request, num):
    context = {
        'num': num,
    }
    return render(request, 'articles/detail.html', context)
```

```html
<!-- articles/detail.html -->
{% extends "base.html" %}

{% block content %}
  <h1>여기는 {{ num }}번 게시글 페이지입니다.</h1>
{% endblock content %}

```





## App과 URL

**2 번째 앱 pages 생성 후 발생할 수 있는 문제점**

- view 함수 이름이 같거나 같은 패턴의 url 주소를 사용하게 되는 경우
- 아래 코드와 같이 해결할 수 있으나 더 좋은 방법이 필요
- "URL을 각자 app에서 관리하자"

```python
# firstpjt/urls.py
from articles import views as articles_views
from pages import views as pages_views

urlpatterns = [
    ...,
    path('pages', pages_views.index),
]
```



### App URL mapping

> 각 앱에 URL을 정의하는 것

- 프로젝트와 각 앱이 URL을 나누어 관리를 편하게 하기 위함



**기존 url 구조**

<img src="https://blog.kakaocdn.net/dn/mTa3I/btrdIMQm1g8/fdjS7Hz1ZppUtyAUZAGwU1/img.png" alt="img" style="zoom: 50%;" />

**변경된 url 구조**

<img src="https://blog.kakaocdn.net/dn/N8SOb/btrdMIzHW2I/6UD2bGfkGKK17VaWYVQLr1/img.png" alt="img" style="zoom:50%;" />



### include()

- 프로젝트 내부 앱들의 URL을 참조할 수 있도록 매핑하는 함수
- URL의 일치하는 부분까지 잘라내고, 남은 문자열 부분은 후속 처리를 위해 include된 URL로 전달
- 코드를 include 할 수도 있음 (코드 관리의 편의성을 위함)
  - html 파일 앞에 '_'(언더바)를 붙여 include를 위한 파일임을 명시적으로 표현함




### url 구조 변화

```py
# firstpjt/urls.py
from django.contrib import admin
from django.urls import path, include

urlpatterns = [
    path('admin/', admin.site.urls),
    path('articles/', include('articles.urls')),
    path('pages/', include('pages.urls')),
]
```

```python
# articles/urls.py
from django.urls import path
# 명시적 상대경로
from . import views

urlpatterns = [
    path('index/', views.index),
    path('dinner/', views.dinner),
    path('search/', views.search),
    path('throw/', views.throw),
    path('catch/', views.catch),
    path('hello/<name>/', views.greeting),
]
```

```python
# pages/urls.py
from django.urls import path
# 명시적 상대경로
from . import views

urlpatterns = [
    path('index/', views.index),
]
```



## URL 이름 지정

**url 구조 변경에 따른 문제점**

- 기존 'articles/' 주소가 'articles/index/'로 변경됨에 따라 해당 주소를 사용하는 모든 위치를 찾아가 변경해야 함
- "URL에 이름을 지어 주면 이름만 기억하면 되지 않을까?"



### Naming URL patterns

> URL에 이름을 지정하는 것 (path 함수의 name 인자를 정의해서 사용)

```python
# articles/urls.py
from django.urls import path
from . import views

urlpatterns = [
    path('index/', views.index, name='index'),
    path('dinner/', views.dinner, name='dinner'),
    path('search/', views.search, name='search'),
    path('throw/', views.throw, name='throw'),
    path('catch/', views.catch, name='catch'),
    path('hello/<name>/', views.greeting, name='greeting'),
    path('articles/<int:num>/', views.detail, name='detail'),
]
```

```python
# pages/urls.py
from django.urls import path
from . import views

urlpatterns = [
    path('index/', views.index, name='index'),
]
```



### 'url' tag

- 주어진 URL 패턴의 이름과 일치하는 절대 경로 주소를 반환
- `{% url "url-name" arg1 arg2 %}`



### URL 표기 변화

- href 속성 값뿐만 아니라 form의 action 속성처럼 **url을 작성하는 모든 위치에서 변경**

```html
<!-- articles/index.html -->
{% extends "base.html" %}

{% block content %}
  <h1>안녕, {{ name }}</h1>
  <!-- <a href="/dinner/">dinner</a> -->
  <a href="{% url "dinner" %}">dinner</a>
  <a href="{% url "search" %}">search</a>
  <a href="{% url "throw" %}">throw</a>
  <a href="{% url "index" %}">두번째 앱의 메인 페이지</a>
{% endblock content %}
```



## URL 이름 공간

**URL 이름 지정 후 남은 문제**

- aricles 앱의 url 이름과 pages 앱의 url 이름이 같은 상황
- 단순히 이름만으로는 완벽하게 분리할 수 없음
- "이름에 성(key)을 붙이자"



### 'app_name' 속성 지정

- app_name 변수 값 설정

```python
# articles/urls.py
app_name = 'articles'
urlpatterns = [
    ...,
]
```



### URL tag의 최종 변화

- 마지막으로 url 태그가 사용하는 모든 곳의 표기 변경하기

- `{% url 'index' %}` -> `{%url 'articles:index' %}`



## 참고

### Trailing Slashes

- django URL 끝에 '/'가 없다면 자동으로 붙임 (Django의 url 설계 철학)
- 기술적인 측면에서, foo.com/bar와 foo.com/bar/는 서로 다른 URL
  - 검색 엔진 로봇이나 웹 트래픽 분석 도구에서는 이 두 주소를 서로 다른 페이지로 봄
- 그래서 Django는 검색 엔진이 혼동하지 않기 위해 붙이는 것을 선택한 것
- 그러나 모든 프레임워크가 이렇게 동작하는 것은 아니니 주의