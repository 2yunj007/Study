# Django ORM

## ORM(Object Relational Mapping)

> 객체 지향 프로그래밍 언어를 사용하여 호환되지 않는 유형의 시스템 간에 데이터를 변환하는 기술

<img src="https://velog.velcdn.com/images%2Fhwaya2828%2Fpost%2F812181b9-e42d-4350-bd3e-2cc12db7c7ee%2F%EC%8A%A4%ED%81%AC%EB%A6%B0%EC%83%B7%202021-08-15%20%EC%98%A4%EC%A0%84%2011.12.40.png" alt="img" style="zoom: 33%;" />



## QuerySet API

> ORM에서 데이터를 검색, 필터링, 정렬 및 그룹화 하는데 사용하는 도구

- API를 사용하여 SQL이 아닌 Python 코드로 데이터를 처리
- `Article.objects.all()`
  - `Arcicle` : Model class
  - `objects` : Manager
  - `all()` : Queryset API (메서드)
- Python의 모델 클래스와 인스턴스를 활용해 DB에 데이터를 저장, 조회, 수정, 삭제하는 것이 목적



### Query

- 데이터베이스에 특정한 데이터를 보여 달라는 요청
- "쿼리문을 작성한다."
  - 원하는 데이터를 얻기 위해 데이터베이스에 요청을 보낼 코드를 작성
- 파이썬으로 작성한 코드가 ORM에 의해 SQL로 변환되어 데이터베이스에 전달되며, 데이터베이스의 응답 데이터를 ORM이 QuerySet이라는 자료 형태로 변환하여 우리에게 전달



### QuerySet

- 데이터베이스에게서 전달 받은 객체 목록 (데이터 모음)
  - 순회가 가능한 데이터로써 1개 이상의 데이터를 불러와 사용할 수 있음 (반복문에 사용 가능)
  - Django ORM을 통해 만들어진 자료형
  - 단, 데이터베이스가 단일한 객체를 반환할 때는 QuerySet이 아닌 모델(Class)의 인스턴스로 반환됨



## QuerySet API 실습

### Create

#### QuerySet API 실습 사전 준비

- 외부 라이브러리 설치 및 설정

```cmd
$ pip install ipython
$ pip install django_extensions
$ pip freeze > requirements.txt
```

```python
INSTALLED_APPS = [
    # app 등록 권장 순서
    # 1. normal app
    'articles',
    # 2. third party app
    'dajngo-extensions',
    # 3. django app
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
]
```



#### Django shell 실행

- **Django shell**
  - Django 환경 안에서 실행되는 python shell
  - 입력하는 QuerySet API 구문이 Django 프로젝트에 영향을 미침
  - `exit`를 입력하여 종료
  - 'ctrl + L'로 터미널 정리

```cmd
$ python manage.py shell_plus
```



#### 데이터 객체를 생성하는 방법

**첫 번째 방법**

- 특정 테이블에 새로운 행을 추가하여 데이터 추가
  - 인스턴스 article을 활용하여 인스턴스 변수 활용하기

```cmd
>>> article = Article()	# Article(class)로부터 article(instance) 생성
>>> article
<Article: Article object (None)>

>>> article.title = 'first'		# 인스턴스 변수(title)에 값을 할당
>>> article.content = 'django!'	# 인스턴스 변수(content)에 값을 할당
```



- save를 하지 않으면 아직 DB에 값이 저장되지 않음 (표가 비어져 있음)
  - `save()` : 객체를 데이터베이스에 저장하는 메서드

```python
>>> article
<Article: Article object (None)>

>>> Article.objects.all()	# Article 클래스의 모든 데이터를 조회하는 메서드
<QuerySet []>
```



- save를 호출하고 확인하면 저장된 것을 확인할 수 있음

```python
>>> article.save()
>>> article
<Article: Article object (1)>	# 저장 후 출력하면 None -> 1(ID)
>>> article.id
1
>>> article.pk
1
>>> Article.objects.all()
<QuerySet [<Article: Article object (1)>]>	# 인덱스로 접근해야 함 (값이 하나이더라도)

>>> article.created_at
datetime.datetime(2023, 9, 15, 1, 28, 10, 948657, tzinfo=datetime.timezone.utc)
```



**두 번째 방법**

- 처음에 인스턴스를 만들 때 초깃값을 키워드 인자로 설정한 뒤에 저장하는 방식

```python
>>> article = Article(title="second", content="django!")

# 아직 저장되어 있지 않음
>>> article
<Article: Article object (None)>

# save를 호출해야 저장됨
>>> article.save()
>>> article
<Article: Article object (2)>
Article.objects.all()
<QuerySet [<Article: Article object (1)>, <Article: Article object (2)>]>
```



**세 번째 방법**

- QuerySet API 중 create() 메서드 활용

```python
# 위 2가지 방법과 달리 바로 저장 이후 바로 생성된 데이터가 반환됨
>>> Article.objects.create(title="third", content="django!")
<Article: Article object (3)>

# article은 현재 Article의 2번째 객체가 저장된 인스턴스
# 즉, 3번을 아직 담지 않았기 때문에 2가 출력됨
>>> article.pk
2
```



### Read

#### all()

> 전체 데이터 조회

```python
>>> articles = Article.objects.all()

>>> for article in articles:
...:     print(article.title)
first
second
third
```



#### get()

> 단일 데이터 조회

- 객체를 찾을 수 없으면 DoesNotExist 예외를 발생시키고, 둘 이상의 객체를 찾으면 MultipleObjectsReturned 예외를 발생시킴
- 그렇기 때문에 primary key와 같이 고유성(uniqueness)을 보장하는 조회에서 사용해야 함

```python
>>> Article.objects.get(pk=1)
<Article: Article object (1)>

>>> Article.objects.get(pk=100)
DoesNotExist: Article matching query does not exist.

>>> Article.objects.get(content="django!")
MultipleObjectsReturned: get() returned more than one Article -- it returned 3!
```



#### filter()

> 특정 조건 데이터 조회

- 조건을 만족하지 않으면 빈 QuerySet을 반환
- 조건에 상관없이 (하나의 데이터를 조회해도) QuerySet을 반환

```python
>>> Article.objects.filter(content="django!")
<QuerySet [<Article: Article object (1)>, <Article: Article object (2)>, <Article: Article object (3)>]>

>>> Article.objects.filter(content="abc")
<QuerySet []>

>>> Article.objects.filter(title="first")
<QuerySet [<Article: Article object (1)>]>
```



### Update

- 인스턴스 변수를 변경 후 save 메서드 호출

```python
# 수정할 인스턴스 조회
>>> article = Article.objects.get(pk=1)

# 인스턴스 변수를 변경
>>> article.title = 'byebye'

# 저장
>>> article.save()

# 정상적으로 변경된 것을 확인
>>> article.title
'byebye'
```



### Delete

- 삭제하려는 데이터 조회 후 delete 메서드 호출

```python
# 삭제할 인스턴스 조회
>>> article = Article.objects.get(pk=1)

# delete 메서드 호출 (삭제된 객체가 반환)
>>> article.delete()
(1, {'articles.Article': 1})

# 삭제한 데이터는 더 이상 조회 불가능
>>> article = Article.objects.get(pk=1)
DoesNotExist: Article matching query does not exist.
```



- 전체 데이터 삭제 가능

```python
>>> article = Article.objects.all()

>>> article.delete()
(2, {'articles.Article': 2})

>>> article
<QuerySet []>
```



## 참고

### Field lookups

- 특정 레코드에 대한 조건을 설정하는 방법
- QuerySet 메서드 filter(), exclude() 및 get()에 대한 키워드 인자로 지정됨
  	- https://docs.djangoproject.com/en/4.2/ref/models/querysets/#field-lookups

```python
# Field lookups 예시

# content 칼럼에 'dja'가 포함된 모든 데이터 조회
>>> Article.objects.filter(content__contains='dja')
```



### ORM, QuerySet API를 사용하는 이유

- 데이터베이스 쿼리를 추상화하며 Django 개발자가 데이터베이스와 직접 상호작용하지 않아도 되도록 함
- 데이터베이스와의 결합도를 낮추고 개발자가 더욱 직관적이고 생산적으로 개발할 수 있도록 도움



### 실습 참고

```python
# makemigrations를 하면 아무런 변화가 없다고 뜸
# 설계도에 아무런 영향을 주지 않기 때문

def __str__(self):
	return f'{self.pk}번 게시글 데이터'
```

