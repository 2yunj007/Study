# Django ORM with view

## Read

### 전체 게시글 조회

```py
# articles/views.py
from django.shortcuts import render
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



### 단일 게시글 조회

```py
# articles/urls.py
from django.urls import path
from . import views

app_name = 'articles'
urlpatterns = [
    path('', views.index, name='index'),
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



