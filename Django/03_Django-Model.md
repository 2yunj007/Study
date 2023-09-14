# Django Model

> DB의 테이블을 정의하고 데이터를 조작(생성, 수정, 삭제)할 수 있는 기능을 제공

<img src="https://images.velog.io/images/qlgks1/post/b2786c2a-d435-4810-bb81-5e0f247f7c1a/django_request.png" alt="Django template form - http data request, response / sync async / race  condition" style="zoom: 80%;" />



## Model

### Model 클래스 작성

```python
# artiles/model.py
class Article(models.Model):		# models.Model -> models 모듈의 Model 클래스 상속받음
    title = models.CharField(max_length=10)	# title -> CharField 클래스의 인스턴스
    content = models.TextField()	# content -> TextField 클래스의 인스턴스
```



### model 클래스 살펴보기

- 작성한 모델 클래스는 최종적으로 DB에 테이블 구조를 만듦
- django.db.models 모듈의 Model이라는 부모 클래스를 상속받음
- Model은 model에 관련된 모든 코드가 이미 작성되어 있는 클래스
  - https://github.com/django/django/blob/main/django/db/models/base.py#
- 개발자는 가장 중요한 테이블 구조를 어떻게 설계할지에 대한 코드만 작성하도록 하기 위한 것 (프레임워크의 이점)



**클래스 변수명**

- 테이블의 각 "필드(열) 이름"



**model Field 클래스**

- 테이블 필드의 "데이터 타입"
- https://docs.djangoproject.com/en/4.2/ref/models/fields/



**model Field 클래스의 키워드 인자 (필드 옵션)**

- 테이블 필드의 "제약 조건" 관련 설정
- https://docs.djangoproject.com/en/4.2/ref/models/fields/#field-options/

- 제약 조건
  - 데이터가 올바르게 저장되고 관리되도록 하기 위한 규칙
  - ex) 숫자만 저장되도록, 문자를 100자까지만 저장되도록 하는 등



## Migrations

> model 클래스의 변경사항(필드 생성, 수정 삭제 등)을 DB에 최종 반영하는 방법

- model class --**makemigrations**--> migration 파일 (최종 설계도) --**migrate**--> db sqlite3 (DB)

<img src="https://velog.velcdn.com/images%2Fwonseok2877%2Fpost%2Fbb953dad-b66b-4b58-8322-2f7e57cc137b%2Fimage.png" alt="img" style="zoom:50%;" />

### Migrations 핵심 명령어

**model class를 기반으로 최종 설계도(migration) 작성**

`python manage.py makemigrations`



**최종 설계도를 DB에 전달하여 반영**

`python manage.py migrate`



### 추가 Migrations

**이미 생성된 테이블에 필드를 추가해야 한다면?**

#### 추가 모델 필드 작성

```python
# artiles/model.py
class Article(models.Model):
    title = models.CharField(max_length=10)
    content = models.TextField()
    created_at = models.DateTimeField(auto_now_add=True)
    updated_at = models.DateTimeField(auto_now=True)
```

```cmd
$ python manage.py makemigrations
```



- 이미 기존 테이블이 존재하기 때문에 필드를 추가할 때 필드의 기본 값 설정이 필요
- 1번은 현재 대화를 유지하면서 직접 기본 값을 입력하는 방법
- 2번은 현재 대화에서 나간 후 models.py에 기본 값 관련 설정을 하는 방법

```cmd
It is impossible to add the field 'created_at' with 'auto_now_add=True' to article without providing a default. This is because the database needs something to populate existing rows.   
 1) Provide a one-off default now which will be set on all existing rows
 2) Quit and manually define a default value in models.py.
Select an option: 1
```



- 추가하는 필드의 기본 값을 입력해야 하는 상황
- 날짜 데이터이기 때문에 직접 입력하기 보다 Django가 제안하는 기본 값을 사용하는 것을 권장
- 아무것도 입력하지 않고 enter를 누르면 Django가 제안하는 기본 값으로 설정됨

```cmd
Please enter the default value as valid Python.
Accept the default 'timezone.now' by pressing 'Enter' or provide another value.
The datetime and django.utils.timezone modules are available, so it is possible to provide e.g. timezone.now as a value.
Type 'exit' to exit this prompt
[default: timezone.now] >>>
```



- migrations 과정 종료 후 2번째 migration 파일이 생성됨을 확인 (0002_article_created_at_article_updated_at.py)
- 이처럼 Django는 설계도를 쌓아 가면서 추후 문제가 생겼을 시 복구하거나 되돌릴 수 있도록 함 (git commit과 유사)

```cmd
Migrations for 'articles':
  articles\migrations\0002_article_created_at_article_updated_at.py
    - Add field created_at to article
    - Add field updated_at to article
```



- migrate 후 테이블 필드 변화 확인

```cmd
$ python manage.py migrate
```



- **model class 변경** -> **makemigrations** -> **migrate**

  - model class에 변경사항(1)이 생겼다면,

  - 반드시 새로운 설계도를 생성(2)하고,

  - 이를 DB에 반영(3)해야 함



## Model Field

> DB 테이블의 필드(열)을 정의하며, 해당 필드에 저장되는 데이터 타입과 제약 조건을 정의



### CharField()

- 길이의 제한이 있는 문자열을 넣을 때 사용
- 필드의 최대 길이를 결정하는 max_length는 필수 인자



### TextField()

- 글자 수가 많을 때 사용



### DateTimeField()

- 날짜와 시간을 넣을 때 사용
- **auto_now**
  - 데이터가 저장될 때마다 자동으로 현재 날짜 시간을 저장
- **auto_now_add**
  - 데이터가 처음으로 생성될 때만 자동으로 현재 날짜 시간을 저장



## Admin site

### Automatic admin interface

- Django는 추가 설치 및 설정 없이 자동으로 관리자 인터페이스를 제공

- 데이터 확인 및 테스트 등을 진행하는데 매우 유용



### Admin 계정 생성

`python manage.py createsuperuser`

- email은 선택 사항이기 때문에 입력하지 않고 진행 가능
- 비밀번호 입력 시 보안상 터미널에 출력되지 않으니 무시하고 입력 이어가기
- DB에 생성된 admin 계정 확인



### admin에 모델 클래스 등록

- admin.py에 작성한 모델 클래스를 등록해야만 admin site에서 확인 가능

```python
from django.contrib import admin
# 명시적 상대경로
from . import models
# from .models import Article

# Article 모델 클래스를 admin에 등록
admin.site.register(models.Article)
```

- admin site 로그인 후 등록된 모델 클래스 확인
- 데이터 생성, 수정, 삭제 테스트
- 테이블 확인



## 참고

### 데이터베이스 초기화

1. migration 파일 삭제

   - 0001_initial.py
   - 0002_article_created_at_article_updated_at.py

2. db.sqlite3 파일 삭제

3. 아래 파일과 폴더를 지우지 않도록 주의

   - __ init __.py

   - migrations 폴더



### Migrations 기타 명령어

**migrations 파일들이 migrate 됐는지 안 됐는지 여부를 확인하는 명령어**

`python manage.py showmigrations`

- '[X]' 표시가 있으면 migrate가 완료되었음을 의미



**설계도가 어떤 언어로 번역되었는지 확인하는 명령어**

`python manage.py sqlmigrate articles 0001`

- 위 명령어는 articles 앱의 0001번 설계도가 어떤 언어로 번역되었는지 확인하는 명령어
- 즉, 해당 migrations 파일이 SQL 언어(DB에서 사용하는 언어)로 어떻게 번역되어 DB에 전달되었는지 확인하는 명령어



### 첫 migrate 시 출력 내용이 많은 이유

- Django 프로젝트가 동작하기 위해 미리 작성되어 있는 기본 내장 app들에 대한 migration 파일들이 함께 migrate 되기 때문

```cmd
$ python manage.py migrate
Operations to perform:
  Apply all migrations: admin, articles, auth, contenttypes, sessions
Running migrations:
  Applying contenttypes.0001_initial... OK
  Applying auth.0001_initial... OK
  Applying admin.0001_initial... OK
  Applying admin.0002_logentry_remove_auto_add... OK
  Applying admin.0003_logentry_add_action_flag_choices... OK
  # (중략)
```



### SQLlite

- 데이터베이스 관리 시스템 중 하나이며 Django의 기본 데이터베이스로 사용됨
- 파일로 존재하며 가볍고 호환성이 좋음



### CRUD

- 소프트웨어가 가지는 기본 데이터 처리 기능
- Create, Read, Update, Delete
