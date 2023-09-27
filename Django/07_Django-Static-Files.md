# Django Static files

## Static Files(정적 파일)

> 서버 측에서 변경되지 않고 고정적으로 제공되는 파일 (이미지, JS, CSS 파일 등)

- **정적 파일을 제공하려면 요청에 응답하기 위한 URL이 필요**



### 웹 서버와 정적 파일

- 웹 서버의 기본 동작은 특정 위치(URL)에 있는 자원을 요청(HTTP request) 받아서 응답(HTTP response)을 처리하고 제공(serving)하는 것
- 이는 "자원에 접근 가능한 주소가 있다."라는 의미
- 웹 서버는 요청 받은 URL로 서버에 존재하는 정적 자원을 제공함
- 정적 파일을 제공하기 위한 경로(URL)가 있어야 함



### STATIC_URL

> 기본 경로 및 추가 경로에 위치한 정적 파일을 참조하기 위한 URL

- 실제 파일이나 디렉토리가 아니며, URL로만 존재
- **URL + STATIC_URL + 정적파일 경로**
  - http://127.0.0.1:8000/static/articles/sample-1.png

```py
# settings.py

STATIC_URL = 'static/'
```



### Static files 제공하기

1. 기본 경로에서 제공하기
2. 추가 경로에서 제공하기



### 기본 경로에서 제공하기

- 기본 경로
  - app폴더/static/

- articles/static/articles/ 경로에 이미지 파일 배치

- static tag를 사용해 이미지 파일에 대한 url 제공
  - 상단에 `{% load static %}`를 작성해야 함 (import 같은 의미)

```html
<!-- articles/index.html -->
{% load static %}

<img src="{%  static "articles/sample-1.png" %}" alt="sample1 img">
```

- 개발자 도구를 통해 STATIC_URL 확인



### 추가 경로에서 제공하기

- STATICFILES_DIRS에 문자열 값으로 추가 경로 설정

```py
# settings.py
STATICFILES_DIRS = [
    BASE_DIR / 'static',
]
```

- 추가 경로에 이미지 파일 배치
  - 최상단에 static 폴더 생성 후 이미지 파일 배치
- static tag를 사용해 이미지 파일에 대한 url 제공

```html
<!-- articles/index.html -->
<img src="{% static "sample-2.png" %}" alt="sample2 img">
```



#### STATICFILES_DIRS

> 정적 파일의 기본 경로 외에 추가적인 경로 목록을 정의하는 리스트



## 이미지 업로드

### Media Files

> **사용자**가 웹에서 업로드하는 정적 파일 (user-uploaded)

- 서버가 미리 준비한 파일이 아님



### ImageField()

> 이미지 업로드에 사용하는 모델 필드

- 이미지 객체가 직접 저장되는 것이 아닌 '**이미지 파일의 경로**'가 문자열로 DB에 저장



### 미디어 파일을 제공하기 전 준비

1. settings.py에 MEDIA_ROOT, MEDIA_URL 설정
2. 작성한 MEDIA_ROOT와 MEDIA_URL에 대한 url 지정



#### MEDIA_ROOT

> 미디어 파일들이 위치하는 디렉토리의 절대 경로

```py
# settings.py
MEDIA_ROOT = BASE_DIR / 'media'
```



#### MEDIA_URL

> MEDIA_ROOT에서 제공되는 미디어 파일에 대한 주소를 생성 (STATIC_URL과 동일한 역할)

```py
# settings.py
MEDIA_URL = 'media/'
```



#### MEDIA_ROOT와 MEDIA_URL에 대한 url 지정

- 업로드 된 파일의 URL == settings.MEDIA_URL
- 위 URL을 통해 참조하는 파일의 실제 위치 == settings.MEDIA_ROOT

```py
# crub/urls.py
from django.conf import settings
from django.conf.urls.static import static

urlpatterns = [
    # ... the rest of your URLconf goes here ...
] + static(settings.MEDIA_URL, document_root=settings.MEDIA_ROOT)
```

- https://docs.djangoproject.com/en/4.2/howto/static-files/



### 이미지 업로드

- blank=True 속성을 작성해 빈 문자열이 저장될 수 있도록 제약 조건 설정
- 기존 필드 사이에 작성해도 실제 테이블 생성 시에는 가장 우측(뒤)에 추가됨

```py
# articles/models.py
class Article(models.Model):
    title = models.CharField(max_length=10)
    content = models.TextField()
    image = models.ImageField(blank=True)	# 이미지 업로드
    created_at = models.DateTimeField(auto_now_add=True)
    updated_at = models.DateTimeField(auto_now=True)
```

- migration 진행
  - ImageField를 사용하려면 반드시 Pillow 라이브러리가 필요

```cmd
$ pip install Pillow

$ python manage.py makemigrations
$ python manage.py migrate

$ pip freeze > requirements.txt
```

- form 요소의 entype 속성 추가
  - `enctype="multipart/form-data"`
  - https://developer.mozilla.org/en-US/docs/Web/HTML/Element/form

```html
<!-- articles/create.html -->
<h1>CREATE</h1>
<form action="{% url "articles:create" %}" method="POST" enctype="multipart/form-data">
  {% csrf_token %}
  {{ form.as_p }}
  <input type="submit">
</form>
```

- view 함수에서 업로드 파일에 대한 추가 코드 작성
  - ` request.FILES`

```py
# article/views.py
def create(request):
    if request.method == 'POST':
        form = ArticleForm(request.POST, request.FILES)
...
```

- 이미지 입력 양식 및 업로드 결과 확인

  - midia 폴더 생성됨

  - DB에는 파일 자체가 아닌 **파일 경로**가 문자열로 저장됨
    - 성능 및 DB 최적화
      - 직접 파일을 저장하면 DB 크기가 급격하게 증가 및 성능 저하 발생
      - 파일 자체는 파일 시스템에 별도로 저장
    - 유지 보수 관점
      - 만약 DB에 직접 파일을 저장해 버리면  파일을 변경하거나 업데이트할 때 DB를 직접 조작해야 함
      - 그런데 DB에 경로만 저장되어 있다면, 파일 시스템에서만 파일을 수정하면 됨



### 업로드 이미지 제공하기

- 'url' 속성을 통해 업로드 파일의 경로 값을 얻을 수 있음
- `article.image.url` : 업로드 파일의 경로
- `article.image` : 업로드 파일의 파일 이름

```html
<!-- articles/detail.html -->
<img src="{{ article.image.url }}" alt="#">
```

- 이미지 업로드하지 않은 게시물은 detail 템플릿을 렌더링 할 수 없음
- 이미지 데이터가 있는 경우에만 이미지를 출력할 수 있도록 처리

```html
<!-- articles/detail.html -->
{% if article.image %}
  <img src="{{ article.image.url }}" alt="#">
{% endif %}
```



### 업로드 이미지 수정

- 수정 페이지 form 요소에 enctype 속성 추가

```html
<!-- articles/update.html -->
<h1>Update</h1>
<form action="{% url "articles:update" article.pk %}" method="POST" enctype="multipart/form-data">
  {% csrf_token %}
  {{ form.as_p }}
  <input type="submit">
</form>
```

- update view 함수에서 업로드 파일에 대한 추가 코드 작성
  - ` request.FILES`

```py
# articles/views.py
def update(request, pk):
    article = Article.objects.get(pk=pk)
    if request.method == 'POST':
        form = ArticleForm(request.POST, request.FILES, instance=article)
```



## 참고

### 'upload_to' argument

- imageField()의 upload_to 인자를 사용해 미디어 파일 추가 경로 설정

```py
# articles/models.py
# 1
image = models.ImageField(blank=True, upload_to='images/')

# 2
image = models.ImageField(blank=True, upload_to='%Y/%m/%d/')

# 3
def articles_image_path(instance, filename):
    return f'images/{instance.user.username}/{filename}'

image = models.ImageField(blank=True, upload_to=articles_image_path)
```



### request.FILES가 두 번째 위치 인자인 이유

- ModelForm 상위 클래스 BaseModelForm의 생성자 함수 키워드 인자



### Django imagekit

https://django-imagekit.readthedocs.io/en/latest/