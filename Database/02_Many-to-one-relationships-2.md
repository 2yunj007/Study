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
- 우리가 기억할 것은 User 모델은 직접 참조하지 않는다는 것
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