# Many to one relationships 1

> N:1 or 1:N
>
> 한 테이블의 0개 이상의 레코드가 다른 테이블의 레코드 한 개와 관련된 관계



## 개요

### Comment(N) - Article(1)

- 0개 이상의 댓글을 1개의 게시글에 작성될 수 있음
- Comment에 Article에 대한 외래 키가  필요함
  - 즉, Comment에 외래 키가 3인 댓글을 불러오는 것은 게시글 3번에 대한 댓글을 불러오는 것




### ForeignKey()

> N:1 관계 설정 모델 필드



## 댓글 모델 구현

### 댓글 모델 정의

- ForeignKey() 클래스의 인스턴스 이름은 참조하는 모델 클래스 이름의 단수형으로 작성하는 것을 권장
- ForeignKey 클래스를 작성하는 위치와 관계없이 외래 키는 테이블 필드 마지막에 생성됨

```py
# articles/models.py
class Comment(models.Model):
    article = models.ForeignKey(Article, on_delete=models.CASCADE)
    content = models.CharField(max_length=200)
    created_at = models.DateTimeField(auto_now_add=True)
    updated_at = models.DateTimeField(auto_now=True)
```



### ForeignKey(to, on_delete)

> 참조하는 모델 class 이름

- 외래 키가 참조하는 객체(1)가 사라졌을 때, 외래 키를 가진 객체(N)를 어떻게 처리할지를 정의하는 설정 (데이터 무결성)
  - 데이터 무결성: 데이터의 정확성과 일관성을 유지하고 보증하는 것



#### on_delete의 'CASCADE'

- 부모 객체(참조된 객체)가 삭제됐을 때 이를 참조하는 객체도 삭제
- 기타 설정 값 참고
  - https://docs.djangoproject.com/en/4.2/ref/models/fields/#arguments



### Migration

- 댓글 테이블의 article_id 필드 확인
- 참조하는 클래스 이름의 소문자(단수형)로 작성하는 것이 권장되었던 이유는 아래와 같이 자동으로 생성되기 때문
  - '참조 대상 클래스 이름' + '_' + '클래스 이름'



## 댓글 생성

- shell_plus 실행 및 게시글 작성
  - `python manage.py shell_plus`

```shell
Article.objects.create(title='title', content='content')
```



- 댓글 생성

```shell
# Comment 클래스의 인스턴스 comment 생성
comment = Comment()

# 인스턴스 변수 저장
comment.content = '댓글1'

# DB에 댓글 저장
comment.save()

# 에러 발생
IntegrityError: NOT NULL constraint failed: articles_comment.article_id
# articles_comment 테이블의 ForeinKeyField, article_id 값이 저장 시 누락되었기 때문
```



- comment 인스턴스를 통한 article 값 참조하기

```shell
# 게시글 조회
article = Article.objects.get(pk=1)

# 외래 키 데이터 입력
comment.article = article
# 또는 comment.article_id = article.pk처럼 pk 값을 직접 외래 키 컬럼에
# 넣어 줄 수도 있지만 권장하지 않음

# 댓글 저장 및 확인
comment.save()

comment.pk
=> 1

comment.content
=> 'first comment'

# 클래스 변수명인 article로 조회 시 해당 참조하는 게시물 객체를 조회할 수 있음
comment.article
=> <Article: Article object (1)>

# article_pk는 존재하지 않는 필드이기 때문에 사용 불가
comment.article_id
=> 1

# 1번 댓글이 작성된 게시물의 pk 조회
comment.article.pk
=> 1

# 1번 댓글이 작성된 게시물의 content 조회
comment.article.content
=> 'content'
```



- 두 번째 댓글 생성

```shell
comment = Comment(content='댓글2', article=article)
comment.save()

comment.pk
=> 2

comment
=> <Comment: Comment object (2)>

comment.article.pk
=> 1
```



## 관계 모델 참조

### 역참조

> N:1 관계에서 1에서 N을 참조하거나 조회하는 것 (1 -> N)

- N은 외래 키를 가지고 있어 물리적으로 참조가 가능하지만 1은 N에 대한 참조 방법이 존재하지 않아 별도의 역참조 이름 필요



#### 역참조 사용 예시

`article.comment_set.all()`

- 모델 인스턴스.realated manager(역참조 이름).QuerySet API



#### related manager

> N:1 혹은 M:N 관계에서 역참조 시에 사용하는 매니저

- 'objects' 매니저를 통해 queryset api를 사용했던 것처럼 related manager를 통해 queryset api를 사용할 수 있게 됨



##### Related manager 이름 규칙

- N:1 관계에서 생성되는 Related manager의 이름은 참조하는 "모델명_set" 이름 규칙으로 만들어짐
- 해당 댓글의 게시글 (Comment -> Articel)
  - `comment.article`
- 게시글의 댓글 목록 (Article -> Comment)
  - `article.comment_set.all()`



##### Related manager 연습

- 1번 게시글에 작성된 모든 댓글 내용 출력 (역참조)

```shell
comment = article.comment_set.all()

for comment in comments:
	print(comment.content)
```



## 댓글 구현

### CREATE

- 사용자로부터 댓글 데이터를 입력받기 위한 CommentForm 정의

```py
# articles/forms.py
from .models import Article, Comment

class CommentForm(forms.ModelForm):
    class Meta:
        model = Comment
        fields = '('content',)'
        # fields = '__all__'
```

- CommentForm의 출력 필드 조정
  - Comment 클래스의 외래 키 필드 article 또한 데이터 입력이 필요한 필드이기 때문에 all로 작성하면 외래 키 필드를 입력받게 됨
  - 외래 키 필드는 사용자 입력 값으로 받는 것이 아닌 view 함수 내에서 다른 방법으로 전달받아 저장되어야 함



- detail view 함수에서 CommentForm을 사용하여 detail 페이지에 렌더링

```py
# articles/views.py
from .forms import ArticleForm, CommentForm

def detail(request, pk):
    article = Article.objects.get(pk=pk)
    comment_form = CommentForm()
    context = {
        'article': article,
        'comment_form': comment_form, 
    }
    return render(request, 'articles/detail.html', context)
```

```html
<!-- articles/detail.html -->
<form action="#" method="POST">
    {% csrf_token %}
    {{ comment_form }}
    <input type="submit">
</form>
```



- url 작성 및 action 값 작성

```py
# articles/urls.py
urlpatterns = [
    ...,
    path('<int:pk>/comments/', views.comments_create, name="comments_create"),
]
```

```html
<!-- articles/detail.html -->
<form action="{% url "articles:comments_create" article.pk %}" method="POST">
    {% csrf_token %}
    {{ comment_form }}
    <input type="submit">
</form>
```



- comments_create view 함수 정의
  - save의 commit 인자를 활용해 외래 키 데이터 추가 입력
  - `save(commit=False)`: DB에 저장하지 않고 인스턴스만 반환

```py
# articles/views.py
def comments_create(request, pk):
    # 게시글 조회
    article = Article.objects.get(pk=pk)
    # CommentForm으로 사용자로부터 입력받은 데이터
    comment_form = CommentForm(request.POST)
    if comment_form.is_valid():
        comment = comment_form.save(commit=False)
        comment.article = article
        comment.save()
        return redirect('articles:detail', article.pk)
    context = {
        'article': article,
        'comment_form': comment_form,
    }
    return render(request, 'articles/detail.html', context)
```



### READ

- detail view 함수에서 전체 댓글 데이터를 조회

```py
# articles/views.py
from .models import Article, Comment

def detail(request, pk):
    article = Article.objects.get(pk=pk)
    comment_form = CommentForm()
    comments = article.comment_set.all()
    # 아래와 같이 입력하면 게시글에 상관없이 모든 댓글을 조회하게 됨
    # comments = Comment.objects.all()
    context = {
        'article': article,
        'comment_form': comment_form, 
        'comments': comments,
    }
    return render(request, 'articles/detail.html', context)
```



- 전체 댓글 출력 및 확인

```html
<!-- articles/detail.html -->
<h4>댓글 목록</h4>
  <ul>
    {% for comment in comments %}
    <li>{{ comment.content }}</li>
    {% endfor %}
  </ul>
```



### DELETE

- 댓글 삭제 url 작성

```py
# articles/urls.py
urlpatterns = [
    ...,
    path('<int:article_pk>/comments/<int:comment_pk>/delete/',
         views.comments_delete,
         name='comments_delete'
         ),
]
```



- 댓글 삭제 view 함수 정의

```py
# articles/views.py
from .models import Article, Comment

def comments_delete(request, article_pk, comment_pk):
    # 댓글 조회
    comment = Comment.objects.get(pk=comment_pk)
    comment.delete()
    return redirect('articles:detail', article_pk)
```



- 댓글 삭제 버튼 생성

```html
<!-- articles/detail.html -->
<h4>댓글 목록</h4>
  <ul>
    {% for comment in comments %}
    <li>
      {{ comment.content }}
      <form action="{% url "articles:comments_delete" article.pk comment.pk %}" method="POST">
        {% csrf_token %}
        <input type="submit" value="삭제">
      </form>
    </li>
    {% endfor %}
  </ul>
```



### 참고

#### admin site 등록

- Comment 모델을 admin site에 등록해 CRUD 동작 확인하기

```py
# articles/admin.py
from .models import Article, Comment

admin.site.register(Article)
admin.site.register(Comment)
```



#### 댓글이 없는 경우 대체 콘텐츠 출력

- DTL 'for empty' 태그 사용

```html
{% for comment in comments %}
    <li>
      {{ comment.content }}
      <form action="{% url "articles:comments_delete" article.pk comment.pk %}" method="POST">
        {% csrf_token %}
        <input type="submit" value="삭제">
      </form>
    </li>
{% empty %}
  <p>댓글이 없습니다</p>
{% endfor %}
```



#### 댓글 개수 출력하기

- DTL filter - 'length' 사용

```html
{{ comments|length }}

{{ article.comment_set.all|length }}
```



- QuerysetAPI - 'count()' 사용

```html
{{ article.comment_set.count }}
```



# Many to one relationships 2

## Article & User

### 모델 관계 설정

- User 외래 키 정의

```py
# articles/models.py
class Article(models.Model):
    user = models.ForeignKey(settings.AUTH_USER_MODEL, on_delete=models.CASCADE)
    title = models.CharField(max_length=10)
    content = models.TextField()
    created_at = models.DateTimeField(auto_now_add=True)
    updated_at = models.DateTimeField(auto_now=True)
```



### User 모델을 참조하는 2가지 방법

- django 프로젝트의 '내부적인 구동 순서'와 '반환 값'에 따른 이유
- 우리가 기억할 것은 **User 모델은 직접 참조하지 않는다는 것**
  - 수정사항이 발생하면 직접 참조한 User 모델을 모두 수정해 주어야 함
- `settings.AUTH_USER_MODEL`를 사용하는 이유
  - User 객체가 아직 존재하지 않는 경우가 있기 때문

|           | get_user_model()                | settings.AUTH_USER_MODEL |
| --------- | ------------------------------- | ------------------------ |
| 반환 값   | User Object (객체)              | accounts.User (문자열)   |
| 사용 위치 | models.py가 아닌 다른 모든 위치 | models.py                |



### Migration

- 기본적으로 모든 컬럼은 NOT NULL 제약조건이 있기 때문에 데이터가 없이는 새로운 필드가 추가되지 못함
  - 기본값 설정 필요
- 1을 입력하고 Enter 진행 (다음 화면에서 직접 기본 값 입력)

```cmd
$ python manage.py makemigrations

It is impossible to add a non-nullable field 'user' to article without specifying a default. This is because the database needs something to populate existing rows.
Please select a fix:
 1) Provide a one-off default now (will be set on all existing rows with a null value for this column)
 2) Quit and manually define a default value in models.py.
Select an option:
```



- 추가되는 외래 키 user_id에 어떤 데이터를 넣을 것인지 직접 입력해야 함
- 마찬가지로 1 입력하고 Enter 진행
- 그러면 기존에 작성된 게시글이 있다면 모두 1번 회원이 작성한 것으로 처리됨

```cmd
Please enter the default value as valid Python.
The datetime and django.utils.timezone modules are available, so it is possible to provide e.g. timezone.now as a value.
Type 'exit' to exit this prompt
```



- migrations 파일 생성 후 migrate 진행
  - `$ python manage.py migrate`

- article 테이블의 user_id 필드 생성 확인



### 게시글 CREATE

- 기존 ArticleForm 출력 변화 확인
  - User 모델에 대한 외래 키 데이터 입력을 위해 불필요한 input이 출력
- ArticleForm 출력 필드 수정

```python
# articles/forms.py

class ArticleForm(forms.ModelForm):
    
    class Meta:
        model = Article
        # fields = ('title', 'content',)
        exclude = ('user',)
```



- 게시글 작성 시 에러 발생
  - "NOT NULL constraint failed: articles_article.user_id"
  - user_id 필드 데이터가 누락되었기 때문

- 게시글 작성 시 작성자 정보가 함께 저장될 수 있도록 save의 commit 옵션 활용

```python
# articles/views.py

@login_required
def create(request):
    if request.method == 'POST':
        form = ArticleForm(request.POST)
        if form.is_valid():
            article = form.save(commit=False)
            article.user = request.user		# 현재 로그인된 유저
            # article.save()
            form.save()		# 권장
            return redirect('articles:detail', article.pk)
    else:
        ...
```



### 게시글 READ

- 각 게시글의 작성자 이름 출력

```html
<!-- articles/index.html -->
<h2>DETAIL</h2>
<h3>{{ article.pk }} 번째 글</h3>
<hr>
<p>작성자 : {{ article.user }}</p>
<p>제목: {{ article.title }}</p>
<p>내용: {{ article.content }}</p>
<p>작성 시각: {{ article.created_at }}</p>
<p>수정 시각: {{ article.updated_at }}</p>
```



### 게시글 UPDATE

- 게시글 수정 요청 사용자와 게시글 작성 사용자를 비교하여 본인의 게시글만 수정할 수 있도록 하기

```py
# articles/views.py
@login_required
def update(request, pk):
    article = Article.objects.get(pk=pk)
    if request.user == article.user:
        if request.method == 'POST':
            form = ArticleForm(request.POST, instance=article)
            if form.is_valid:
                form.save()
                return redirect('articles:detail', article.pk)
        else:
            form = ArticleForm(instance=article)
    else:
        return redirect('articles:index')
    context = {
        'article': article,
        'form': form,
    }
    return render(request, 'articles/update.html', context)
```



- 해당 게시글의 작성자가 아니라면 수정/삭제 버튼 출력하지 않도록 하기

```html
<!-- articles/detail.html -->
{% if request.user == article.user %}
  <a href="{% url "articles:update" article.pk %}">UPDATE</a>
  <form action="{% url "articles:delete" article.pk %}" method="POST">
    {% csrf_token %}
    <input type="submit" value="삭제">
  </form>
{% endif %}
```



### 게시글 DELETE

- 삭제를 요청하려는 사람과 게시글을 작성한 사람을 비교하여 본인의 게시글만 삭제할 수 있도록 하기

```py
# articles/view.py
@login_required
def delete(request, pk):
    article = Article.objects.get(pk=pk)
    if request.user == article.user:
        article.delete()
    return redirect('articles:index')
```



## Comment & User

### Comment - User 모델 관계 설정

- User 외래 키 정의

```py
# articles/models.py
class Comment(models.Model):
    article = models.ForeignKey(Article, on_delete=models.CASCADE)
    user = models.ForeignKey(settings.AUTH_USER_MODEL, on_delete=models.CASCADE)
    content = models.CharField(max_length=200)
    created_at = models.DateTimeField(auto_now_add=True)
    updated_at = models.DateTimeField(auto_now=True)
```



### Migration

- 이전에 Article와 User 모델 관계 설정 때와 동일한 상황
- 기존 Comment 테이블에 새로운 컬럼이 빈 값으로 추가될 수 없기 때문에 기본 값 설정 과정이 필요

```cmd
$ python manage.py makemigrations

# ... 공통 과정 생략

$ python manage.py migrate
```

- comment 테이블 user_id 필드 확인



### 댓글 CREATE

- 댓글 작성 시 이전에 게시글 작성할 때와 동일한 에러 발생
  - 댓글의 user_id 필드 데이터가 누락되었기 때문

- 댓글 작성 시 작성자 정보를 함께 저장할 수 있도록 작성

```py
# articles/views.py
def comments_create(request, pk):
    article = Article.objects.get(pk=pk)
    comment_form = CommentForm(request.POST)
    if comment_form.is_valid():
        comment = comment_form.save(commit=False)
        comment.article = article
        comment.user = request.user
        comment_form.save()
        return redirect('articles:detail', article.pk)
    ...
```



### 댓글 READ

- 댓글 출력 시 댓글 작성자와 함께 출력

```html
<!-- articles/detail.html -->
{% for comment in comments %}
      <li>
        {{comment.user}} - {{ comment.content }}
        ...
      </li>
    {% endfor %}
```



### 댓글 DELETE

- 댓글 삭제 요청 사용자와 댓글 작성 사용자를 비교하여 본인의 댓글만 삭제할 수 있도록 하기

```py
# articles/views.py
def comments_delete(request, article_pk, comment_pk):
    comment = Comment.objects.get(pk=comment_pk)
    if request.user == comment.user:
        comment.delete()    
    return redirect('articles:detail', article_pk)
```



- 해당 댓글의 작성자가 아니라면, 댓글 삭제 버튼을 출력하지 않도록 함

```html
<!-- articles/detail.html -->
<ul>
  {% for comment in comments %}
    <li>
      {{comment.user}} - {{ comment.content }}
      {% if request.user == comment.user %}
        <form action="{% url "articles:comments_delete" article.pk comment.pk %}" method="POST">
          {% csrf_token %}
          <input type="submit" value="삭제">
        </form>
      {% endif %}
    </li>
  {% endfor %}
</ul>
```



## 참고

### 인증된 사용자만 댓글 작성 및 삭제

```py
# articles/views.py
@login_required
def comments_create(request, pk):
    pass

@login_required
def comments_delete(request, article_pk, comment_pk):
    pass
```
