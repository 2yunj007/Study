# Many to many relationships 1

## Many to many relationships

> 한 테이블의 0개 이상의 레코드가 다른 테이블의 0개 이상의 레코드와 관련된 경우 (양쪽 모두에서 N:1 관계를 가짐)



### Article(M) - User(N)

> 0개 이상의 게시글은 0명 이상의 회원과 관련

- 게시글은 회원으로부터 0개 이상의 좋아요를 받을 수 있고, 회원은 0개 이상의 게시글에 좋아요를 누를 수 있음



### M:N 관계 주요 사항

- M:N 관계로 맺어진 두 테이블에는 물리적인 변화가 없음
- ManyToManyField는 중개 테이블을 자동으로 생성
- ManyToManyField는 M:N 관계를 맺는 두 모델 어디에 위치해도 상관없음
  - 대신 필드 작성 위치에 따라 참조와 역참조 방향을 주의할 것
- N:1은 완전한 종속의 관계였지만 M:N은 종속적인 관계가 아니며 '의사에게 진찰받는 환자 & 환자를 진찰하는 의사' 이렇게 2가지 형태 모두 표현 가능



## Django ManToManyField

###  ManToManyField(to, **options)

> Many to many 관계 설정 시 사용하는 모델 필드



#### 'related_name' arguments

- 역참조 시 사용하는 manager name을 변경

```py
class Patient(models.Model):
    doctors = models.ManyToManyField(Doctor, related_name='patients')
```

```py
# 변경 전
doctor.patient_set.all()

# 변경 후
doctor.patients.all()
```



#### 'symmetrical' arguments

- ManyToManyField가 동일한 모델을 가리키는 정의에서만 사용
- True일 경우
  - 기본 값
  - source 모델의 인스턴스가 target 모델의 인스턴스를 참조하면 자동으로 target 모델 인스턴스도 source 모델 인스턴스를 자동으로 참조하도록 함(대칭)
  - 즉, 자동으로 내가 당신의 친구라면 당신도 내 친구가 됨
- Flase일 경우
  - True였을 대와 반대 (대칭되지 않음)

```py
class Person(models.Model):
    friends = models.ManyToManyField('self')
    # friends = models.ManyToManyField('self', symmetrical=Flase)
```



### M:N에서의 methods

- **add()**
  - 지정된 객체를 관련 객체 집합에 추가
  - 이미 존재하는 관계에 사용하면 관계가 복제되지 않음
- **remove()**
  - 관련 객체 집합에서 지정된 모델 객체를 제거



## 좋아요

### 모델 관계 설정

- ManyToManyField 작성

```py
# articles/models.py
class Article(models.Model):
    user = models.ForeignKey(
        settings.AUTH_USER_MODEL, on_delete=models.CASCADE
    )
    like_users = models.ManyToManyField(settings.AUTH_USER_MODEL)
    title = models.CharField(max_length=10)
    content = models.TextField()
    created_at = models.DateTimeField(auto_now_add=True)
    updated_at = models.DateTimeField(auto_now=True)
```

- Migration 진행 후 **에러 발생**



#### **`user.article_set` 역참조 매니저 충돌**

- like_users 필드 생성 시 자동으로 역참조 `.article_set` 매니저가 생성됨
- 그러나 이전 N:1(Article-User) 관계에서 이미 같은 이름의 매니저를 사용 중
  - `user.article_set.all()`: 해당 유저가 작성한 모든 게시글을 조회
- 'user가 작성한 글(`user.article_set`)'과 'user가 좋아요를 누른 글(`user.article_set`)'을 구분할 수 없게 됨
- user와 관계된 ForeignKey 혹은 ManyToManyField 둘 중 하나에 related_name 작성 필요



- realted_name 작성 후 Migration 재진행

```py
# articles/models.py
class Article(models.Model):
    user = models.ForeignKey(
        settings.AUTH_USER_MODEL, on_delete=models.CASCADE
    )
    like_users = models.ManyToManyField(settings.AUTH_USER_MODEL, related_name='like_articles')
    title = models.CharField(max_length=10)
    content = models.TextField()
    created_at = models.DateTimeField(auto_now_add=True)
    updated_at = models.DateTimeField(auto_now=True)
```



#### User-Article간 사용 가능한 전체 related manager

- **article.user**
  - 게시글을 작성한 유저 - N:1
- **user.article_set**
  - 유저가 작성한 게시글(역참조) - N:1
- **article.like_users**
  - 게시글을 좋아요 한 유저 - M:N
- **user.like_articles**
  - 유저가 좋아요 한 게시글(역참조) - M:N




### 좋아요 기능 구현

- url 작성

```py
# articles/urls.py
urlpatterns = [
    ...,
    path('<int:article_pk>/likes/', views.likes, name-'likes'),
]
```



- view 함수 작성

```py
# articles/views.py
@login_required
def likes(request, article_pk):
    # 별도의 페이지 필요하지 않음
    # 어떤 게시글에 좋아요를 누른 건지
    article = Article.objects.get(pk=article_pk)
	# article = get_object_or_404(get_user_model(), pk=article_pk)
    
    # 좋아요를 추가 or 취소할지에 대한 기준:
    # 현재 좋아요 버튼을 누른 유저가 
    # 현재 게시글의 좋아요를 누른 유저 목록 전체에 있는지 없는지를 확인
    if request.user in article.like_users.all():
    # if article.like_users.filter(pk=request.user.pk).exists():
        # 취소
        article.like_users.remove(request.user)
        # request.user.like_articles.remove(article)
    else:
        # 추가
        article.like_users.add(request.user)
        # request.user.like_articles.add(article)
    return redirect('articles:index')        
```



- index 템플릿에서 각 게시글에 좋아요 버튼 출력

```html
<!-- articles/index.html -->
{% for article in articles %}
    ...
  <form action="{% url "articles:likes" article.pk %}" method="POST">
    {% csrf_token %}
    {% if request.user in article.like_users.all %}
      <input type="submit" value="좋아요 취소">
    {% else %}
      <input type="submit" value="좋아요">
    {% endif %}
  </form>
  <p>좋아요 수: {{ article.like_users.all | length }}</p>
  <hr>
{% endfor %}
```



#### .exists()

> QuerySet 결과가 포함되어 있으면 True를 반환하고 결과가 포함되어 있지 않으면 False를 반환



# Many to many relationships 2

## 팔로우

### 프로필 구현

- url 작성

```py
# accounts/urls.py
urlpatterns = [
    ...,
    # profile을 앞에 붙이는 이유는 위에 설정한 값을 제외한 모든 문자열들이 걸리게 되기 때문
    path('profile/<username>/', views.profile, name='profile'),
    # path('profile/<str:username>', views.profile, name='prifile'),    
    # str: 기본 값이기 때문에 생략 가능
```



- view 함수 작성

```py
# accounts/views.py
def profile(request, username):
    # User의 Detail 페이지
    # User를 조회
    User =  get_user_model()    # User model은 직접 참조 X
    person = User.objects.get(username=username)
    context = {
        'person': person,
    }
    return render(request,'accounts/profile.html', context)
```



- profile 템플릿 작성

```html
<!-- accounts/profile.html -->
<h1>{{ person.username }}님의 프로필</h1>
  
<hr>

<h2>작성한 게시글</h2>
{% for article in person.article_set.all %}
  <p>{{ article.title }}</p>
{% endfor %}
<hr>

<h2>작성한 댓글</h2>
{% for comment in person.comment_set.all %}
  <p>{{ comment.content }}</p>
{% endfor %}
<hr>

<h2>좋아요를 누른 게시글</h2>
{% for article in person.like_articles.all %}
  <p>{{ article.title }}</p>
{% endfor %}
```



- 프로필 페이지로 이동할 수 있는 링크 작성

```html
<!-- accounts/index.html -->
<a href="{% url "accounts:profile" request.user.username %}">내 프로필</a>

<p>작성자 : <a href="{% url "accounts:profile" article.user.username %}">{{ article.user }}</a></p>
```



### 팔로우 기능 구현

- ManyToManyField 작성

```py
# accounts/models.py
class User(AbstractUser):
    followings = models.ManyToManyField('self', symmetrical=False, related_name='followers')
```

- 참조
  - 내가 팔로우하는 사람들 (팔로잉, followings)
- 역참조
  - 상대방 입장에서 나는 팔로워 중 한 명 (팔로워, followers)
- 바뀌어도 상관없으나 관계 조회 시 생각하기 편한 방향으로 정한 것



- url 작성

```py
# accounts/urls.py
urlpatterns = [
    ...,
    path('<int:user_pk>/follow/', views.follow, name='follow'),     
    # username으로 해도 무방
]
```



- view 함수 작성

```py
# accounts/views.py
@login_required
def follow(request, user_pk):
    # follow를 하는 대상 조회
    User = get_user_model()
    you = User.objects.get(pk=user_pk)
    me = request.user

    if me != you:
        # 내가 상대방의 팔로워 목록에 있다면
        if me in you.followers.all():
        # 팔로우 취소
            you.followers.remove(me)
        # 팔로우
        else:
            you.followers.add(me)
            # me.followwings.add(tou)
    return redirect('accounts:profile', you.username)
```



- 프로필 유저의 팔로잉, 팔로워 수 & 팔로우, 언팔로우 버튼 작성

```html
<!-- accounts/profile.html -->
<div>
  <div>
    팔로잉 : {{ person.followings.all|length }} / 팔로워 {{ person.followers.all|length }}
  </div>
  {% if request.user != person %}
    <div>  
      <form action="{% url "accounts:follow" person.pk %}" method="POST">
        {% csrf_token %}
        {% if request.user in person.followers.all %}
          <input type="submit" value="Unfollow">
        {% else %}
          <input type="submit" value="Follow">
        {% endif %}
      </form>
    </div>
  {% endif %}
</div>
```



## Django Fixtures

> Django가 데이터베이스로 가져오는 방법을 알고 있는 데이터 모음

- 데이터 베이스 구조에 맞추어 작성되어 있음
- 초기 데이터 제공
  - Fixtures의 사용 목적



### 초기 데이터의 필요성

- 협업하는 유저 A, B가 있다고 생각해 보기
  1. A가 먼저 프로젝트를 작업한 후 github에 push
     - gitignore로 인해 DB는 업로드하지 않기 때문에 A가 생성한 데이터도 업로드되지 않음
  2. B가 hithub에서 A가 push한 프로젝트를 pull (혹은 clone)
     - 결과적으로 B는 DB가 없는 프로젝트를 받게 됨
- 이처럼 Django 프로젝트의 앱을 처음 설정할 때 동일하게 준비된 데이터로 데이터베이스를 미리 채우는 것이 필요한 순간이 있음
- fixtures를 사용해 앱에 초기 데이터(initial data를 제공)



### Fixtures 관련 명령어

#### dumpdata

- 데이터베이스의 모든 데이터를 추출
- 추출한 데이터는 json 형식으로 저장

```cmd
$ python manage.py dumpdata [app_name[.ModelName] [app_name[.ModelName] ...]] > filename.json
```



#### loaddata

- Fixtures 데이터를 데이터베이스로 불러오기

- Fixtures 파일 기본 경로

  - `app_name/fixtures/`
  - Django 는 설치된 모든 app의 디렉토리에서 fixtures 폴더 이후의 경로로 fixtures 파일을 찾아 load

  

- db.splite3 파일 삭제 후 migrate 진행

```py
# 해당 위치로 fixture 파일 이동

articles/
  fixtures/
    articles.json
    users.json
    comments.json
```



- load 후 데이터가 잘 입력되었는지 확인

```cmd
$ python manage.py loaddata articles.json users.json comments.json
```



- loaddata 순서 주의사항
   - 만약 loaddata를 한번에 실행하지 않고 하나씩 실행한다면 모델 관계에 따라 load 하느 순서가 중요할 수 있음
     - comment는 article에 대한 key 및 user에 대한 key가 필요
     - article은 user에 대한 key가 필요
   - 즉, 현재 모델 관계에서는 user -> article -> comment 순으로date를 넣어야 오류가 발생하지 않음



### 참고

#### 모든 모델을 한번에 dump 하기

```cmd
# 3개의 모델을 하나의 json 파일로
$ python manage.py dumpdata --indent 4 articles.artilce articles.comment accounts.user > data.json

# 모든 모델을 하나의 json 파일로
$ python manage.py dumpdata --indent 4 > data.json
```



#### loaddata 시 encoding codec 관련 에러가 발생하는 경우

- 2가지 방법 중 택 1

1. dumpdata 시 추가 옵션 작성

```cmd
$ python -Xutf8 manage.py dumpdata [생략]
```

2. 메모장 활용
   1. 메모장으로 json 파일 열기
   2. "다른 이름으로 저장" 클릭
   3. 인코딩을 UTF8로 선택 후 저장