# Django REST framework 1

## REST API

### API (Application Programming Interface)

> 애플리케이션과 프로그래밍으로 소통하는 방법

- 클라이언트-서버처럼 서로 다른 프로그램에서 요청과 응답을 받을 수 있도록 만든 체계



#### Web API

- 웹 서버 또는 웹 브라우저를 위한 API
- 현대 웹 개발은 하나부터 열까지 직접 개발하기보다 여러 Open API들을 활용하는 추세
- 대표적인 Third Party Open API 서비스 목록
  - Youtube API
  - Google Map API
  - Naver Papago API
  - Kakao Map API



### REST (Representational State Transfer)

> API Server를 개발하기 위한 일종의 소프트웨어 설계 방법론 "약속(규칙X)"



#### RESTful API

> REST라는 설계 디자인 약속을 지켜 구현한 API

- REST 원리를 따르는 시스템을 RESTful 하다고 부름
- "자원을 정의"하고 "자원에 대한 주소를 지정"하는 전반적인 방법을 서술
- "각각 API 구조를 작성하는 모습이 너무 다르니 약속을 만들어서 다같이 통일해서 쓰자!"



#### REST에서 자원을 정의하고 주소를 지정하는 방법

- 자원의 식별
  - URL
- 자원의 행위
  - HTTP Methods
- 자원의 표현
  - JSON 데이터
  - 궁극적으로 표현되는 데이터 결과물



### 자원의 식별

#### URI (Uniform Resource Identifier, 통합 자원 식별자)

> 인터넷 리소스(자원)을 식별하는 문자열

- 가장 일반적인 URL는 웹 주소로 알려진 URL



#### URL (Uniform Resource Locator, 통합 자원 위치)

> 웹에서 주어진 리소스의 주소

- 네트워크 상에 리소스가 어디 있는지를 알려 주기 위한 약속

![전체 URL](https://developer.mozilla.org/ko/docs/Learn/Common_questions/Web_mechanics/What_is_a_URL/mdn-url-all.png)



**Schema (or Protocol)**

- 브라우저가 리소스를 요청하는 데 사용해야 하는 규약
- URL의 첫 부분은 브라우저가 어떤 규약을 사용하는지를 나타냄
- 기본적으로 웹은 HTTP(S)를 요구하며 메일을 열기 위한 mailto:, 파일을 전송하기 위한 ftp: 등 다른 프로토콜도 존재 



**Domain Name**

- 요청 중인 웹 서버를 나타냄
- 어떤 웹 서버가 요구되는지를 가리키며 직접 IP 주소를 사용하는 것도 가능하지만, 사람이 외우기 어렵기 때문에 주로 Domain Name으로 사용
- 예를 들어 도메인 google.com의 IP 주소는 142.251.42.142



**Port**

- 웹 서버의 리소스에 접근하는데 사용되는 기술적인 문(Gate)
- HTTP 프로토콜의 표준 포트
  - HTTP - 80
  - HTTPS - 443
- 표준 포트만 생략 가능



**Path**

- 웹 서버의 리소스 경로
- 초기에는 실제 파일이 위치한 물리적 위치를 나타냈지만, 오늘날은 실제 위치가 아닌 추상화된 형태의 구조를 표현
- 예를 들어 /articles/create/가 실제 articles 폴더 안에 create 폴더 안을 나타내는 것은 아님



**Parameters**

- 웹 서버에 제공하는 추가적인 데이터
- '&' 기호로 구분되는 key-value 쌍 목록
- 서버는 리소스를 응답하기 전에 이러한 파라미터를 사용하여 추가 작업을 수행할 수 있음



**Anchor**

- 일종의 "북마크"를 나타내며 브라우저에 해당 지점에 있는 콘텐츠를 표시
- fragment identifier(부분 식별자)라고 부르는 '#' 이후 부분은 서버에 전송되지 않고 브라우저에게 해당 지점으로 이동할 수 있도록 함



### 자원의 행위

#### HTTP Request Methods

> 리소스에 대한 행위(수행하고자 하는 동작)를 정의

- HTTP verbs라고도 함



**GET**

- 서버에 리소스의 표현을 요청
- GET을 사용하는 요청은 데이터만 검색해야 함



**POST**

- 데이터를 지정된 리소스에 제출
- 서버의 상태를 변경



**PUT**

- 요청한 주소의 리소스를 수정



**DELETE**

- 지정된 리소스를 삭제



#### HTTP response status codes

> 특정 HTTP 요청이 성공적으로 완료되었는지 여부를 나타냄

- Informational responses (100-199)
- Successful responses (200-299)
- Redirection messages (300-399)
- Client error responses (400-499)
- Server error responses (500-599)



### 자원의 표현

#### 그동안 서버가 응답(자원을 표현)했던 것

- 지금까지 Django 서버는 사용자에게 페이지(html)만 응답하고 있었음
- 하지만 서버가 응답할 수 있는 것은 페이지 뿐만 아니라 다양한 데이터 타입을 응답할 수 있음
- REST API 이 중에서도 JSON 타입으로 응답하는 것을 권장
- Django는 더 이상 Templates 부분에 대한 역할을 담당하지 않게 되며, Front-end와 Back-end가 분리되어 구성됨
  - 이제부터 Django를 사용해 RESTful API 서버를 구축할 것



## DRF (Django REST framework)

- Django에서 Restful API 서버를 쉽게 구축할 수 있도록 도와주는 오픈소스 라이브러리



### Serialization

> "직렬화"
>
> 여러 시스템에서 활용하기 위해 데이터 구조나 객체 상태를 나중에 재구성할 수 있는 포맷으로 변환하는 과정

- 어떠한 언어나 환경에서도 나중에 다시 쉽게 사용할 수 있는 포맷으로 변환하는 과정



## DRF with Single Model

### 프로젝트 준비

- 사전 제공된 drf 프로젝트 기반 시작
- 가상 환경 생성, 활성화 및 패키지 설치
- migrate 진행
- 준비된 fixtures 파일을 load하여 실습용 초기 데이터 입력
  - `python manage.py loaddata articles.json`



### GET - List

- 게시글 데이터 목록 조회하기
- 게시글 데이터 목록을 제공하는 ArticleListSerializer 정의

```py
# articles/serializers.py
from rest_framework import serializers
from .models import Article


class ArticleListSerializer(serializers.ModelSerializer):
    class Meta:
        model = Article
        fields = ('id', 'title', 'content',)
```



- url 및 view 함수 작성
  - 데이터가 HTML에 출력되도록 페이지와 함께 응답했던 과거의 view 함수와 달리 JSON 데이터로 serialization 하여 페이지 없이 응답 

```py
# articles/urls.py
urlpatterns = [
    path('articles/', views.article_list),
]
```

```py
# articles/views.py
from rest_framework.response import Response
from rest_framework.decorators import api_view

from .models import Article
from .serializers import ArticleListSerializer


@api_view(['GET'])
def article_list(request):
    articles = Article.objects.all()
    serializer = ArticleListSerializer(articles, many=True)
    return Response(serializer.data)
```



#### ModelSerializer

> Django 모델과 연결된 Serializer 클래스



#### 'api_view' decorator

- DRF view 함수에서는 필수로 작성되며 view 함수를 실행하기 전 HTTP 메서드를 확인
- 기본적으로 GET 메서드만 허용되며 다른 메서드 요청에 대해서는 405 Method Not Allowed로 응답
- DRF view 함수가 응답해야 하는 HTTP 메서드 목록을 작성



### GET - Detail

- 단일 게시글 데이터 조회하기
- 각 게시글의 상세 정보를 제공하는 ArticelSerializer 정의

```py
# articles/serializers.py
class ArticleSerializer(serializers.ModelSerializer):
    class Meta:
        model = Article
        fields = '__all__'
```



- url 및 view 함수 작성

```py
# articles/urls.py
urlpatterns = [
    ...,
    path('articles/<int:article_pk>/', views.article_detail),
]
```

```py
# articles/views.py
from .serializers import ArticleListSerializer, ArticleSerializer

@api_view(['GET'])
def article_detail(request, article_pk):
    article = Article.objects.get(pk=article_pk)
    serializer = ArticleSerializer(article)     # 단일 인스턴스 객체 하나이기 때문에 many X
    return Response(serializer.data)
```



### POST

- 게시글 데이터 생성하기
- 데이터 생성이 성공했을 경우 201 Created를 응답
- 데이터 생성이  실패했을 경우 400 Bad request를 응답

- article_list view 함수 구조 변경 (method에 따른 분기 처리)

```py
# articles/views.py
@api_view(['GET', 'POST'])
def article_list(request):
    if request.method == 'GET':
        articles = Article.objects.all()
        serializer = ArticleListSerializer(articles, many=True)
        return Response(serializer.data)
    
    elif request.method == 'POST':
        serializer = ArticleSerializer(data=request.data)
        if serializer.is_valid():
            serializer.save()
            return Response(serializer.data, status=status.HTTP_201_CREATED)
        return Response(serializer.errors, status=status.HTTP_400_BAD_REQUEST)
```



### DELETE

- 게시글 데이터 삭제하기
- 요청에 대한 데이터 삭제가 성공했을 경우는 204 No Content 응답

```py
# articles/views.py
@api_view(['GET', 'DELETE'])
def article_detail(request, article_pk):
    article = Article.objects.get(pk=article_pk)
    if request.method == 'GET':
        serializer = ArticleSerializer(article)
        return Response(serializer.data)

    elif request.method == 'DELETE':
        article.delete()
        return Response(status=status.HTTP_204_NO_CONTENT)
```



### PUT

- 게시글 데이터 수정하기
- 요청에 대한 데이터 수정이 성공했을 경우는 200 OK 응답

```py
# articles/views.py

@api_view(['GET', 'DELETE', 'PUT'])
def article_detail(request, article_pk):
    ...
    elif request.method == 'PUT':
        serializer = ArticleSerializer(article, data=request.data)
        # serializer = AticleSerializer(article, data=request.data)
        if serializer.is_valid():
            serializer.save()
            return Response(serializer.data)
        return Response(serializer.errors, status=status.HTTP_400_BAD_REQUEST)
```



### raise_exception

- is_valid()는 유효성 검사 오류가 있는 경우 ValidationError 예외를 발생시키는 선택적 raise_exception 인자를 사용할 수 있음
- DRF에서 제공하는 기본 예외 처리기에 의해 자동으로 처리되며 기본적으로 HTTP 400 응답을 반환

```py
# articles/views.py
@api_view(['GET', 'POST'])
def article_list(request):
    ...
    elif request.method == 'POST':
        serializer = ArticleSerializer(data=request.data)
        if serializer.is_valid(raise_exception=True):
            serializer.save()
            return Response(serializer.data, status=status.HTTP_201_CREATED)
        # return Response(serializer.errors, status=status.HTTP_400_BAD_REQUEST)
```



# Django REST framework 2

## DRF with N:1 Relation

### 프로젝트 준비

- Comment 클래스 정의 및 데이터베이스 초기화

```py
# articles/models.py
class Comment(models.Model):
    article = models.ForeignKey(Article, on_delete=models.CASCADE)
    content = models.TextField()
    created_at = models.DateTimeField(auto_now_add=True)
    updated_at = models.DateTimeField(auto_now=True)
```



- Migration 및 fixtures 데이터 로드

```cmd
$ python manage.py makemigrations
$ python manage.py migrate
$ python manage.py loaddata articles.json comments.json
```



### GET - List

- 댓글 목록 조회를 위한 CommentSerializer 정의

```py
# articles/serializers.py
class CommentSerializer(serializers.ModelSerializer):
   class Meta:
        model = Comment
        fields = '__all__' 
```



- url 작성

```py
# articles/urls.py
urlpatterns = [
    ...,
    path('comments/', views.comment_list),
]
```



- view 함수 작성

```py
# articles/views.py
@api_view(['GET'])
def comment_list(request):
    comments = Comment.objects.all()
    serializer = CommentSerializer(comments, many=True)
    return Response(serializer.data)
```



### GET - Detail

- 단일 댓글 조회를 위한 url 및 view 함수 작성

```py
# articles/urls.py
urlpatterns = [
    ...,
    path('comments/<int:comment_pk>/', views.comment_detail),
]
```

```py
# articles/views.py
@api_view(['GET', 'DELETE', 'PUT'])
def comment_detail(request, comment_pk):
    comment = Comment.objects.get(pk=comment_pk)
    serializer = CommentSerializer(comment)
    return Response(serializer.data)
```



### POST

- 단일 댓글 생성을 위한 url 및 view 함수 작성
- serializer 인스턴스의 save() 메서드는 특정 Serializer 인스턴스를 저장하는 과정에서 추가 데이터를 받을 수 있음

```py
# articles/urls.py
urlpatterns = [
    ...,
    path('articles/<int:article_pk>/comments/', views.comment_create),
]
```

```py
# articles/views.py
@api_view(['POST'])
def comment_create(request, article_pk):
    article = Article.objects.get(pk=article_pk)
    serializer = CommentSerializer(data=request.data)
    if serializer.is_valid(raise_exception=True):
        serializer.save(article=article)
        return Response(serializer.data, status=status.HTTP_201_CREATED)
```



- 상태 코드 400 응답 확인
  -  CommentSerializer에서 외래 키에 해당하는 article field 또한 사용자로부터 입력받도록 설정되어 있기 때문에 서버 측에서 누락되었다고 판단한 것

- 읽기 전용 필드 추가
  - 데이터를 전송하는 시점에, "유효성 검사에서 제외시키고, 데이터 조회 시에는 출력"하는 필드

```py
# articles/serializer.py
class CommentSerializer(serializers.ModelSerializer):
    class Meta:
        model = Comment
        fields = '__all__'
        read_only_fields = ('article',)
```



### DELETE & PUT

- 단일 댓글 삭제 및 수정을 위한 view 함수 작성

```py
# articles/views.py
@api_view(['GET', 'DELETE', 'PUT'])
def comment_detail(request, comment_pk):
    comment = Comment.objects.get(pk=comment_pk)
    if request.method == 'GET':
        serializer = CommentSerializer(comment)
        return Response(serializer.data)

    elif request.method == 'DELETE':
        comment.delete()
        return Response(status=status.HTTP_204_NO_CONTENT)
    
    elif request.method == 'PUT':
        serializer = CommentSerializer(comment, data=request.data)
        if serializer.is_valid(raise_exception=True):
            serializer.save()
            return Response(serializer.data)
```



### 응답 데이터 재구성

#### 댓글 조회 시 게시글 출력 내역 변경

- 댓글 조회 시 게시글 번호만 제공해 주는 것이 아닌 '게시글의 제목'까지 제공하기

```py
# articles/serializers.py
class CommentSerializer(serializers.ModelSerializer):
    class ArticleTitleSerializer(serializers.ModelSerializer):
        class Meta:
            model = Article
            fields = ('title',)     # 참조하고 싶은 것 추가 가능

    # override
    article = ArticleTitleSerializer(read_only=True)

    class Meta:
        model = Comment
        fields = '__all__'
        # 기본 필드에 대해서만 가능, 위에서 덮어 씌웠기 때문에 주석 처리
        # read_only_fields = ('article',)
```



### 역참조 데이터 구성

#### Article -> Comment 간 역참조 관계를 활용한 JSON 데이터 재구성

- 아래 2가지 사항에 대한 데이터 재구성하기
  - 단일 게시글 조회 시 해당 게시글에 작성된 **댓글 목록 데이터**도 함께 붙여서 응답
  - 단일 게시글 조회 시 해당 게시글에 작성된 **댓글 개수 데이터**도 함께 붙여서 응답

- Nested relationships
  - 모델 관계 상으로 참조하는 대상은 참조되는 대상의 표현에 포함되거나 중첩될 수 있음
  - 이러한 중첩된 관계는 serializers를 필드로 사용하여 표현 가능
  - 클래스 선언 순서 유의
- 'source'
  - 필드를 채우는 데 사용할 속성의 이름
  - 점 표기법(dotted notation)을 사용하여 속성을 탐색할 수 있음

```py
# articles/serializers.py
class ArticleSerializer(serializers.ModelSerializer):
    comment_set = CommentSerializer(many=True, read_only=True)
    comment_count = serializers.IntegerField(source='comment_set.count', read_only=True)

    class Meta:
        model = Article
        fields = '__all__'
```

- [주의] 읽기 전용 필드 지정 이슈
  - 특정 필드를 override 혹은 추가한 경우 read_only_fields는 동작하지 않음
  - 해당 필드의 read_only 키워드 인자로 작성해야 함



## API 문서화

### OpenAPI Specification (OAS)

> RESTful API를 설명하고 시각화하는 표준화된 방법

- API에 대한 세부사항을 기술할 수 있는 공식 표준

- OAS 기반 API에 대한 문서를 생성하는데 도움을 주는 오픈소스 프레임워크
  - Swagger
  - Redoc



#### drf-spectacular 라이브러리

- DRF 위한 OpenAPI 3.0 구조 생성을 도와주는 라이브러리
  - 'drf-spectacular' 공식 문서에 과정이 나와 있음
- 설치 및 등록

```cmd
$ pip install drf-spectacular
```

```py
# settings.py
INSTALLED_APPS = [
    ...,
    'drf_spectacular',	# 하이픈 아니고 언더바 주의!!
    ...,
]
```



- 관련 설정 코드 입력 (OpenAPI 스키마 자동 생성 코드)

```py
# settings.py

REST_FRAMEWORK = {
    'DEFAULT_SCHEMA_CLASS': 'drf_spectacular.openapi.AutoSchema',
}
```



- swagger, redoc 페이지 제공을 위한 url 작성

```py
# drf/urls.py
from drf_spectacular.views import SpectacularAPIView, SpectacularRedocView, SpectacularSwaggerView

urlpatterns = [
    ...,
    path('api/schema/', SpectacularAPIView.as_view(), name='schema'),
    path('api/schema/swagger-ui/', SpectacularSwaggerView.as_view(url_name='schema'), name='swagger-ui'),
    path('api/schema/redoc/', SpectacularRedocView.as_view(url_name='schema'), name='redoc'),
]
```



#### OAS의 핵심 이점 - "설계 우선" 접근법

- API를 먼저 설계하고 명세를 작성한 후, 이를 기반으로 코드를 구현하는 방식
- API의 일관성을 유지하고, API 사용자는 더 쉽게 API를 이해하고 사용할 수 있음
- 또한 OAS를 사용하면 API가 어떻게 작동하는지를 시각적으로 보여 주는 문서를 생성할 수 있으며, 이는 API를 이해하고 테스트하는 데 매우 유용
- 이런 목적으로 사용되는 도구각 Swagger-UI 또는 ReDoc



## 참고

### Django shortcuts functions

- render()
- redirect()
- **get_object_or_404()**
  - 모델 manager objects에서 get()을 호출하지만, 해당 객체가 없을 땐 기존 DoesNotExist 예외 대신 Http404를 raise 함

```py
# articles/views.py
from django.shortcuts import get_object_or_404

# article = Article.objects.get(pk=article_pk)
article = get_object_or_404(Article, pk=article_pk)
```

- **get_list_or_404()**
  - 모델 manager objects에서 filter()의 결과를 반환하고, 해당 객체 목록이 없을 땐 Http404를 raise 함

```py
# articles/views.py
from django.shortcuts import get_object_or_404, get_list_or_404

# article = Article.objects.all()
article = get_list_or_404(Article, pk=article_pk)
```



#### 사용하는 이유

- 클라이언트에게 "서버에 오류가 발생하여 요청을 수행할 수 없다(500)"라는 원인이 정확하지 않은 에러를 제공하기 보다는, 적절한 예외 처리를 통해 클라이언트에게 보다 정확한 에러 현황을 전달하는 것도 매우 중요한 개발 요소 중 하나이기 때문
- api로 개발할 때 사용함이 적합

#### 
