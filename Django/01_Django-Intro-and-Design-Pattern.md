# Django Intro & Design Pattern

## Framework

>  웹 애플리케이션을 빠르게 개발할 수 있도록 도와주는 도구 (개발에 필요한 기본 구조, 규칙, 라이브러리 등)



**프레임워크를 사용하는 이유**

- 기본적인 구조, 도구, 규칙 등을 제공하기 때문에 개발자는 필수적으로 해야 하는 개발에만 집중할 수 있음
- 여러 라이브러리를 제공해 개발 속도를 빠르게 할 수 있음 (생산성)
- 유지보수와 확장에 용이해 소프트웨어의 품질을 높임



**Django framework**

> Python 기반의 대표적인 웹 프레임워크



## 클라이언트와 서버

### Client

> 서비스를 **요청**하는 주체 (웹 사용자의 인터넷이 연결된 장치, 웹 브라우저)



### Server

> 클라이언트의 요청에 **응답**하는 주체 (웹 페이지, 앱을 저장하는 컴퓨터)

- Django를 사용해서 서버를 구현할 것



### 웹 페이지를 보게 되는 과정

<img src="https://velog.velcdn.com/images/dnflekf2748/post/3ae851c0-f5a1-4b24-a9f0-00dd4d1efe2d/%EC%84%9C%EB%B2%84%ED%81%B4%EB%9D%BC%EC%9D%B4%EC%96%B8.png" alt="post-thumbnail" style="zoom:67%;" />

1. 웹 브라우저(클라이언트)에서 'google.com'을 입력
2. 브라우저는 인터넷에 연결된 전세계 어딘가에 있는 구글 컴퓨터(서버)에게 'Google 홈페이지.html' 파일을 달라고 요청
3. 요청을 받은 구글 컴퓨터는 데이터베이스에서 'Google 홈페이지.html' 파일을 찾아 응답
4. 전달받은 'Google 홈페이지.html' 파일을 웹 브라우저가 사람이 볼 수 있도록 해석해 주면서 사용자는 구글의 메인 페이지를 보게 됨



## 프로젝트 및 가상 환경

### 가상 환경

> Python 애플리케이션과 그에 따른 패키지들을 격리하여 관리할 수 있는 **독립적인** 실행 환경



**가상 환경을 사용하는 이유**

- 의존성 관리
  - 라이브러리 및 패키지를 각 프로젝트마다 독립적으로 사용 가능
- 팀 프로젝트 협업
  - 모든 팀원이 동일한 환경과 의존성 위에서 작업하여 버전간 충돌을 방지



**가상 환경이 필요한 시나리오 1**

1. 한 개발자가 2개의 프로젝트(A와 B)를 진행해야 함
2. 프로젝트 A는 requests 패키지 버전 1을 사용해야 함
3. 프로젝트 B는 requests 패키지 버전 2을 사용해야 함
4. 하지만 파이썬 환경에서 패키지는 1개의 버전만 존재할 수 있음
5. A와 B 프로젝트가 다른 패키지 버전 사용을 위한 **독립적인 개발 환경**이 필요



**가상 환경이 필요한 시나리오 2**

1. 한 개발자가 2개의 프로젝트(A와 B)를 진행해야 함
2. 프로젝트 A는 water라는 패키지를 사용해야 함
3. 프로젝트 B는 fire라는 패키지를 사용해야 함
4. 하지만 파이썬 환경에서 water 패키지와 fire 패키지를 함께 사용하면 충돌이 발생하여 설치 불가능
5. A와 B 프로젝트의 패키지 충돌을 피하기 위해 각각 **독립적인 개발 환경**이 필요



#### 가상 환경 설정

[Python 가상 환경 설정](https://married-spot-253.notion.site/Python-89514730def04096b9b42b4824c50967)

**가상 환경 venv 생성**

`python -m venv venv`

- 가상 환경으로 이동하는 개념이 아니라 **on/off** 개념

- 두 번째 venv는 가상 환경 이름 (이름을 venv로 하는 것은 암묵적인 약속)
- `.gitignore`에 들어가 있어야 함



**가상 환경 활성화**

`source venv/Scripts/activate`

- 명령어를 입력할 때마다 (venv) 출력됨 (가상 환경 내에 있다는 표시)



**가상 환경 종료**

`deactivate`



**환경에 설치된 패키지 목록 확인**

`pip list`

- 가상 환경 비활성화 상태

```cmd
$ pip list
Package                           Version
--------------------------------- --------
anyio                             3.7.1
argon2-cffi                       21.3.0
argon2-cffi-bindings              21.2.0
arrow                             1.2.3
asttokens                         2.2.1
async-lru                         2.0.3
...
```

- 가상 환경 활성화 상태
  - 환경이 분리되어 있음을 알 수 있음

```cmd
$ pip list
Package    Version
---------- -------
pip        23.2.1
setuptools 58.1.0
(venv)
```



### 의존성 패키지

- 한 소프트웨어 패키지가 다른 패키지의 기능이나 코드를 사용하기 때문에 그 패키지가 존재해야만 제대로 작동하는 관계
- 사용하려는 패키지가 설치되지 않았거나, 호환되는 버전이 아니면 오류가 발생하거나 예상치못한 동작을 보일 수 있음
- 개발 환경에서는 각각의 프로젝트가 사용하는 패키지와 그 버전을 정확히 관리하는 것이 중요



**패키지 목록이 필요한 시나리오**

- 만약 2명(A와 B)의 개발자가 하나의 프로젝트를 함께 개발한다고 함
- 팀원 A가 먼저 가상 환경을 생성 후 프로젝트를 설정하고 관련된 패키지를 설치하고 개발하다가 협업을 위해 github에 프로젝트를 push 함
- 팀원 B는 해당 프로젝트를 clone 받고 실행해 보려고 하지만 실행되지 않음
- 팀원 A가 프로젝트를 위해 어떤 패키지를 설치했고, 어떤 버전을 설치했는지 A의 가상 환경 상황을 알 수 없음
- 가상 환경에 대한 모습, 즉 패키지 목록이 공유되어야 함



#### 의존성 패키지 생성

**A가 할 일**

1. 의존성 패키지 목록(requirements) 텍스트를 생성하여 공유

   `pip freeze`

   `pip freeze > requirements.txt`

   - 패키지 설치 시마다 진행



**B가 할 일**

1. 가상 환경 생성

2. 패키지 다운로드

   `pip install -r requirements.txt`



### Django

#### Django 프로젝트 생성

**생성 전 루틴**

1. 가상 환경 생성

   `python -m venv venv`

2. 가상 환경 활성화

   `source venv/Scripts/activate`

3. Django 설치

    `pip install Django`

4. 의존성 파일 생성

   `pip freeze > requirements.txt`

5. .gitignore 파일 생성 (첫 add 전)

6. git 저장소 생성



**Django 프로젝트 생성**

`django-admin startproject firstpjt .`

- firstpjt라는 이름의 프로젝트 생성



**Django 서버 실행**

`python manage.py runserver`

- manage.py와 동일한 경로에서 명령어 진행
- 꼭 ctrl+c로 종료시켜 줌



**서버 확인**

http://127.0.0.1:8000/ 접속 후 확인



### LTS(Long-Term Support)

- 프레임워크나 라이브러리 등의 소프트웨어에서 장기간 지원되는 안정적인 버전을 의미할 때 사용
- 기업이나 대규모 프로젝트에서는 소프트웨어 업그레이드에 많은 비용과 시간이 필요하기 때문에 안정적이고 장기간 지원되는 버전이 필요
- https://www.djangoproject.com/download/



## Django 프로젝트와 앱

### Django project

> 애플리케이션의 집합 (DB 설정 , URL 연결, 전체 앱 설정 등을 처리)



### Django application

> 독립적으로 작동하는 기능 단위 모듈 (각자 특정한 기능을 담당하며 다른 앱들과 함께 하나의 프로젝트를 구성)



#### 앱사용 과정

**앱 생성**

`python manage.py startapp articles`

- 앱의 이름은 **복수형**으로 지정하는 것을 권장



**앱 등록**

- 반드시 **앱을 생성한 후에 등록**해야 함 (등록 후 생성은 불가능)

```python
# firstpjt - setting.py

INSTALLED_APPS = [
    'articles',  # 추가
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
]
```



## 디자인 패턴

> 소프트웨어 설계에서 발생하는 문제를 해결하기 위한 일반적인 해결책 (공통적인 문제를 해결하는 데 쓰이는 형식화된 관행)



### MVC 디자인 패턴

>  (Model, View, Controller)
>
> 애플리케이션을 구조화하는 대표적인 패턴 (데이터, 사용자 인터페이스, 비즈니스 로직을 분리)

- 시각적 요소와 뒤에서 실행되는 로직을 서로 영향 없이, 독립적이고 쉽게 유지 보수할 수 있는 애플리케이션을 만들기 위해



### MTV 디자인 패턴

> (Model, Template, View)
>
> Django에서 애플리케이션을 구조화하는 패턴 (기존 MVC 패턴과 동일하나 명칭만 다름)



### 프로젝트 구조

- **setting.py** : 프로젝트의 모든 설정을 관리

- **urls.py** : URL과 이에 해당하는 적절한 view를 연결

- **____init____.py** : 해당 폴더를 패키지로 인식하도록 설정

- **asgi.py** : 비동기식 웹 서버와 연결 관련 설정

- **wsgi.py** : 웹 서버와의 연결 관련 설정

- **manage.py** : Django 프로젝트와 다양한 방법으로 상호작용하는 커맨드라인 유틸리티

- **admin.py** : 관리자용 페이지 설정

- **models.py** : DB와 관련된 Model을 정의, MTV: M

- **views.py** : HTTP 요청을 처리하고 해당 요청에 대한 응답을 반환 (url, mode, temlpate과 연계), MTV: V

- **apps.py** : 앱의 정보가 작성된 곳

- **test.py** : 프로젝트 테스트 코드를 작성하는 곳



## 요청과 응답

<img src="https://images.velog.io/images/qlgks1/post/b2786c2a-d435-4810-bb81-5e0f247f7c1a/django_request.png" alt="Django template form - http data request, response / sync async / race  condition" style="zoom: 80%;" />

**데이터 흐름에 따른 코드 작성**

- URLs -> View > Template



### URLs

- http://127.0.0.1:8000/alticles/로 요청이 왔을 때 views 모듈의 index 뷰 함수를 호출
- 더 많은 일을 할 수 있도록 url를 늘려 줘야 함

```python
# urls.py
from django.contrib import admin
from django.urls import path
from articles import views	# articles 패키지에서 views 모듈을 가져오는 것

urlpatterns = [
    path('admin/', admin.site.urls),	# url 경로는 반드시 '/'(end slash)로 끝나야 함
    # path('articles/', views에 있는 어떠한 함수를 호출),
    path('articles/', views.index),		# ','(trailing comma) 작성 권장
]
```



### View

- 특정 경로에 있는 template과 request 객체를 결합해 응답 객체를 반환하는 index view 함수 정의
- 모든 view 함수는 첫 번째 인자로 **request(요청) 객체를 필수적으로 받음** (사용하지 않아도)
  - 요청에 대한 객체가 필수라는 것. 객체명이 'request'가 아니더라도 기능적인 문제 없음
  - 그러나 관행적으로 바꾸지 않음

```python
# views.py
from django.shortcuts import render

# Create your views here.
def index(request):
    # url로부터 호출되면
    # 메인 페이지 응답 객체를 만들어서 반환
    return render(request, 'articles/index.html')	# 템플릿 경로
```



### Template

1. **articles 앱 폴더 안에** templates 폴더 생성
   - **폴더명은 반드시 templates**여야 하며 개발자가 직접 생성해야 함
2. templates 폴더 안에 articles 폴더 생성
3. articles 폴더 안에 템플릿 파일(index.html) 생성 
4. http://127.0.0.1:8000/articles/ 페이지 확인



**Django에서 template을 인식하는 경로 규칙**

app폴더 / templates / index.html

app폴더 / templates / articles / index.html

​				    ↑

- django는 이 지점까지의 기본 경로로 인식하기 때문에 이 지점 이후의 template 경로를 작성해야 함
- templates까지가 약속. 그 이후의 템플릿 경로는 상관없음



## 참고

### MTV 디자인 패턴 정리

- **Model**
  - 데이터와 관련된 로직을 관리
  - 응용프로그램의 데이터 구조를 정의하고 데이터베이스의 기록을 관리
- **Template**
  - 레이아웃 화면을 처리
  - 화면상의 사용자 인터페이스 구조와 레이아웃을 정의

- **View**
  - Model & Template과 관련한 로직을 처리해서 응답을 반환
  - 클라이언트의 요청에 대해 처리를 분기하는 역할
  - View 예시
    - 데이터가 필요하면 model에 접근해서 데이터를 가져옴
    - 가져온 데이터를 template로 보내 화면을 구성
    - 구성된 화면을 응답으로 만들어 클라이언트에게 반환



### render 함수

- `render(request, template_name, context)`
- 주어진  템플릿을 주어진 컨텍스트 데이터와 결합하고 렌더링 된 텍스트와 함께 HttpResponse(응답) 객체를 반환하는 함수



### 실습

**github repo에 django 프로젝트 push**

1. 가상 환경 생성

2. 활성화

3. 장고 설치

4. 패키지 목록 생성

5. git init 또는 git ignore 설정

6. 프로젝트 다 만들고 (메인페이지 출력)

7. add commit push