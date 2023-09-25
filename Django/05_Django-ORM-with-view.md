# Django ORM with view

## Read

### 전체 게시글 조회

```py
# articles/views.py
from .models import Article


def index(request):
    articles = Article.objects.all()   
    context = {
        'articles': articles,
    }
    return render(request, 'articles/index.html', context)
```

```html
<!-- articles/index.html -->
<h1>Articles</h1>
{% for article in articles %}
<p>글 번호 : {{article.pk }}</p>
<p>글 제목 : {{ article.title }}</p>
<p>글 내용 : {{ article.content }}</p>
<hr>
{% endfor %}
```

```cmd
$ python manage.py createsuperuser	# 게시글 작성
```

```py
# articles/admin.py
from django.contrib import admin
from .models import Article

# Register your models here.
admin.site.register(Article)
```



### 단일 게시글 조회

```py
# articles/urls.py
urlpatterns = [
    ...
    path('<int:pk>/', views.detail, name='detail'),
]
```

```py
# articles/views.py
def detail(request, pk):
    article = Article.objects.get(pk=pk)
    context = {
        'article': article,
    }
    return render(request, 'articles/detail.html', context)
```

```html
<!-- templates/articles/detail.html -->
<h2>DETAIL</h2>
<h3>{{ article.pk }} 번째 글</h3>
<hr>
<p>{{ article.title }}</p>
<p>{{ article.content }}</p>
<p>{{ article.created_at }}</p>
<p>{{ article.updated_at }}</p>
<hr>
<a href="{% url "articles:index" %}">[back]</a>
```



### 단일 게시글 페이지 링크 작성

```html
<!-- 에러 발생 -->
<h1>Articles</h1>
{% for article in articles %}
<p>글 번호 : {{ article.pk }}</p>
<a href="{% url "articles:detail" %}">
  <p>글 제목 : {{ article.title }}</p>
</a>
<p>글 내용 : {{ article.content }}</p>
<hr>
{% endfor %}
```

- **NoReverseMatch** 에러가 발생할 경우 url 흐름만 따라가 보면 됨
- 다음과 같이 수정 (url 태그 인자 추가)

```html
<a href="{% url "articles:detail" article.pk %}">
```



## Create

**Create 로직을 구현하기 위해 필요한 view 함수 (게시글을 쓰고 DB에 저장되기까지)**

- 사용자 입력 데이터를 받을 페이지를 렌더링 (new)
- 사용자가 입력한 데이터를 받아 DB에 저장 (create)



### new 기능 구현

```py
# articles/urls.py
urlpatterns = [
    ...
    path('new/', views.new, name='new'),
]
```

```py
# articles/views.py
def new(request):
    return render(request, 'articles/new.html')
```

```html
<!-- templates/articles/new.html -->
<h1>NEW</h1>
<form action="{% url "articles:create" %}" method="GET">
<div>
  <label for="title">제목 : </label>
  <input type="text" id="title" name="title">
</div>
<div>
  <label for="content">내용 : </label>
  <textarea name="content" id="content" cols="30" rows="10"></textarea>
</div>
<input type="submit">
</form>
<hr>
<a href="{% url "articles:index" %}">[back]</a>
```

- new 페이지로 이동할 수 있는 하이퍼링크 작성

```html
<!-- templates/articles/index.html -->
<h1>Articles</h1>
<a href="{% url "articles:new"%}">NEW</a>
```



### create 기능 구현

```py
# articles/urls.py
urlpatterns = [
    ...
    path('create/', views.create, name='create'),
]
```

```py
# articles/views.py
def create(request):
    title = request.GET.get('title')
    content = request.GET.get('content')
    
    # 1
    # article = Article()
    # article.title = title
    # article.content = content
    # article.save()

    # 2
    # 저장이 되기 전에 유효성 검사를 해야 하기 때문에 2번이 제일 일반적인 방법
    article = Article(title=title, content=content)
    article.save()

    # 3
    # Article.object.create(title=title, content=content)

    return render(request, 'articles/create.html')
```

```html
<!-- templates/articles/create.html -->
<h1>게시글이 작성되었습니다.</h1>
```



### redirect

> 게시글 작성 후 완료를 알리는 페이지를 응답하는 것

- 게시글을 "조회해 줘!"라는 요청이 아닌 "작성해 줘!"라는 요청이기 때문에 게시글 저장 후 페이지를 응답하는 것은 POST 요청에 대한 적절한 응답이 아님
- 데이터 저장 후 페이지를 주는 것이 아닌 다른 페이지로 사용자를 보내야 함
- 사용자를 보낸다 == 사용자가 GET 요청을 한 번 더 보내도록 한다



#### redirect()

> 클라이언트가 인자에 작성된 주소로 다시 요청을 보내도록 하는 함수

- 게시글 작성 후 메인 페이지로 돌아가게 함 (redirect)
  - 글 쓰는 요청을 한 후, 이동할 페이지를 보기 위한 요청을 함

```py
from django.shortcuts import render, redirect
from .models import Article

def create(request):
    title = request.GET.get('title')
    content = request.GET.get('content')
    
    article = Article(title=title, content=content)
    article.save()

    # return render(request, 'articles/create.html')
    return redirect('articles:index')
```



## HTTP request methods

> 데이터(리소스)에 어떤 요청(행동)을 원하는지 나타내는 것 **GET** & **POST**

- **HTTP** : 네트워크 상에서 데이터를 주고 받기 위한 약속



### 'GET' Method

> 특정 리소스를 **조회(request)**하는 요청

- GET으로 데이터를 전달하면 Query String 형식으로 보내짐
  - /?title=제목&content=내용



### 'POST' Method

> 특정 리소스에 **변경(생성(create), 수정(update), 삭제(delete))**을 요구하는 요청

- POST로 데이터를 전달하면 HTTP Body에 담겨 보내짐
- url에 노출되지 않음

```html
<!-- templates/articles/new.html -->
<h1>NEW</h1>
<form action="{% url "articles:create" %}" method="POST">
	...
```

```py
# articles/views.py
def create(request):
    title = request.POST.get('title')
    content = request.POST.get('content')
    ...
```

- 403 응답 발생



### 403 Forbidden

> 서버에 요청이 전달되었지만, 권한 때문에 거절되었다는 것을 의미

- 거절된 이유 : "CSRF token이 누락되었다"
- HTTP response status code
  - 특정 HTTP 요청이 성공적으로 완료되었는지를 3자리 숫자로 표현하기로 약속한 것
  - https://developer.mozilla.org/en-US/docs/Web/HTTP/Status



### CSRF (Cross Site Request Forgery)

> "사이트 간 요청 위조"
>
> 사용자가 자신의 의지와 무관하게 공격자가 의도한 행동을 하여 특정 웹 페이지를 보안에 취약하게 하거나 수정, 삭제 등의 작업을 하게 만드는 공격 방법

- Django 서버는 해당 요청이 DB에 데이터를 하나 생성하는 (DB에 영향을 주는) 요청에 대해 "Django가 직접 제공한 페이지에서 데이터를 작성하고 있는 것인지"에 대한 확인 수단이 필요한 것
- 겉모습이 똑같은 위조 사이트나 정상적이지 않은 요청에 대한 방어 수단
- 기존
  - 요청 데이터 -> 게시글 작성
- 변경
  - 요청 데이터 + 인증 토큰 -> 게시글 작성



**그런데 왜 POST일 때만 Token을 확인할까?**

- POST는 단순 조회를 위한 GET과 달리 특정 리소스에 변경(생성, 수정, 삭제)을 요구하는 의미와 기술적인 부분을 가지고 있기 때문
- DB에 조작을 가하는 요청은 반드시 인증 수단이 필요
- 즉, DB에 대한 변경사항을 만드는 요청이기 때문에 토큰을 사용해 최소한의 신원 확인을 하는 것



**CSRF Token 적용**

`{% csrf_token %}` 

```html
<!-- templates/articles/new.html -->
<h1>NEW</h1>
<form action="{% url "articles:create" %}" method="POST">
	{% csrf_token %}
```



## Delete

```py
# articles/urls.py
urlpatterns = [
    ...
    path('<int:pk>/delete/', views.delete, name='delete'),
]
```

```py
# articles/views.py
def delete(request, pk):
    # 몇 번 게시글을 삭제할 것인지 조회
    article = Article.objects.get(pk=pk)
    # 조회한 게시글을 삭제
    article.delete()
    return redirect('articles:index')
```

```html
<!-- articles/detail.html -->
<h2>DETAIL</h2>
...
<hr>
<form action="{% url "articles:delete" article.pk %}" method="POST">
	{% csrf_token %}
	<input type="submit" value="삭제">
</form>
...
```



## Update

**Update 로직을 구현하기 위해 필요한 view 함수**

- 사용자 입력 데이터를 받을 페이지 렌더링 (edit)
- 사용자가 입력한 데이터를 받아 DB에 저장 (update)



### edit 기능 구현

```py
# articles/urls.py
urlpatterns = [
    ...
    path('<int:pk>/edit/', views.edit, name='edit'),
]
```

```py
# articles/views.py
def edit(request, pk):
    article = Article.objects.get(pk=pk)
    context = {
        'article': article,
    }
    return render(request, 'articles/edit.html', context)
```

- 수정 시 이전 데이터가 출력될 수 있도록 작성하기

```html
<!-- articles/edit.html -->
<h1>Edit</h1>
<form action="#" method="POST">
{% csrf_token %}
<div>
  <label for="title">제목 : </label>
  <input type="text" id="title" name="title" value={{ article.title }}>
</div>
<div>
  <label for="content">내용 : </label>
  <textarea name="content" id="content" cols="30" rows="10">{{ article.content }}</textarea>
</div>
<input type="submit">
</form>
<hr>
<a href="{% url "articles:index" %}">[back]</a>
```

- edit 페이지로 이동하기 위한 하이퍼링크 작성

```html
<!-- articles/detail.html -->
<a href="{% url "articles:edit" article.pk %}">EDIT</a>
```



### update 기능 구현

```py
# articles/urls.py
urlpatterns = [
    ...
    path('<int:pk>/update/', views.update, name='update'),
]
```

```py
# articles/views.py
def update(request, pk):
    title = request.POST.get('title')
    content = request.POST.get('content')

    # 수정하고자 하는 게시글을 조회
    article = Article.objects.get(pk=pk)
    article.title = title
    article.content = content
    article.save()
    
    # 수정 후에는 detail 페이지로 가는 것이 자연스러움
    return redirect('articles:detail', article.pk)  
```

```html
<!-- articles/edit.html -->
<form action="{% url "articles:update" article.pk %}" method="POST">
	...
```

