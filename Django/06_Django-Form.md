# Django Form

## 개요

### HTML 'form'

- 지금까지 사용자로부터 데이터를 받기 위해 활용한 방법
- 그러나 비정상적 혹은 악의적인 요청을 필터링 할 수 없음
- 유효한 데이터인지에 대한 확인이 필요



### 유효성 검사

> 수집한 데이터가 정확하고 유효한지 확인하는 과정

- 유효성 검사를 구현하기 위해서는 입력 값, 형식, 중복, 범위, 보안 등 많은 것을 고려해야 함
- 이런 과정과 기능을 직접 개발하는 것이 아닌 Django가 제공하는 Form을 사용



## Form class

### Django Form

> 사용자 입력 데이터를 수집하고, 처리 및 유효성 검사를 수행하기 위한 도구

- HTML form의 생성, 유효성 검사를 단순화하고 자동화할 수 있는 기능을 제공



### Form class 정의

```py
from django import forms


class ArticleForm(forms.Form):
    title = forms.CharField(max_length=10)
    content = forms.CharField()
```

- max_length가 form 필드에서는 필수인자가 아님



### Form class를 적용한 new 로직

```py
# articles/views.py
from .forms import ArticleForm

def new(request):
    form = ArticleForm()
    context = {
        'form': form,
    }
    return render(request, 'articles/new.html', context)
```

```html
<!-- article/new.html -->
<form action="{% url "articles:create" %}" method="POST">
  {% csrf_token %}
  {{ form }}
  <input type="submit">
</form>
```

- 개발자 도구를 통해 input의 name이 자동으로 작성됨을 알 수 있음



### Form rendering options

- label, input 쌍을 특정 HTML 태그로 감싸는 옵션
  - https://docs.djangoproject.com/en/4.2/topics/forms/#form-rendering-options

```html
<!-- article/new.html -->
<form action="{% url "articles:create" %}" method="POST">
  {% csrf_token %}
  {{ form.as_p }}
  <input type="submit">
</form>  
```



## Widgets

> HTML 'input' element의 표현을 담당

- widget은 단순히 input 요소의 속성 및 출력되는 부분을 변경하는 것
  - https://docs.djangoproject.com/ko/4.2/ref/forms/widgets/#built-in-widgets

```py
# articles/forms.py
from django import forms


class ArticleForm(forms.Form):
    title = forms.CharField(max_length=10)
    content = forms.CharField(widget=forms.Textarea)
```



## Django ModelForm

### Form

- 사용자가 입력 데이터를 DB에 저장하지 않을 때 (ex. 로그인)



### ModelForm

> Model과 연결된 Form을 자동으로 생성해 주는 기능을 제공 (Form + Model)

- 사용자 입력 데이터를 DB에 저장해야 할 때 (ex. 게시글, 회원가입)



### ModelForm class 정의

- 기존 ArticleForm 클래스 수정

```py
# articles/forms.py
from django import forms
from .models import Article



class ArticleForm(forms.ModelForm):
    # model 등록
    class Meta:
        model = Article
        fields = '__all__'  # Article에서 정의한 필드를 모두 가져옴
```



### Meta class

> ModelForm의 정보를 작성하는 곳



### 'fields' 및 'exclude' 속성

- exclude 속성을 사용하여 모델에서 포함하지 않을 필드를 지정할 수도 있음

```py
# articles/forms.py
class ArticleForm(forms.ModelForm):
    class Meta:
        model = Article
        fields = ('title',)
        # exclude = ('title',)
```



### is_valid()

> 여러 유효성 검사를 실행하고, 데이터가 유효한지 여부를 Boolean으로 반환



### ModelForm을 적용한 create 로직

```py
# articles/view.py
from .forms import ArticleForm


def create(request):
    form = ArticleForm(request.POST)
    # 유효성 검사
    # 유효성 검사가 통과된 경우
    if form.is_valid():
        article = form.save()
        return redirect('articles:index', article.pk)
    # 통과되지 못한 경우
    context = {
        'article': article,
        'form': form,
    }
    return render(request, 'articles/new.html', context)
```

- 제목 input에 공백을 입력하면 'This field is required.'라는 에러 메시지 출력
- 공백 데이터가 유효하지 않은 이유와 에러 메시지가 출력되는 과정
  - 별도로 병시하지 않았지만 모델 필드에는 기본적으로 빈 값은 허용하지 않는 제약조건이 설정되어 있음
  - 빈 값은 is_valid()에 의해 False로 평가되고 form 객체에는 그에 맞는 에러 메시지가 포함되어 다음 코드로 진행됨
- 개발자 도구에서 max_length의 값을 변경한 후 10자 이상 입력하면 'Ensure this value has at most 10 characters (it has 20).'라는 에러 메시지가 출력됨



### ModelForm을 적용한 edit 로직

- ArticleForm에 **instance 키워드 인자**를 추가하여 수정할 내용 출력

```py
# articles/views.py
def edit(request, pk):
    article = Article.objects.get(pk=pk)
    form = ArticleForm(instance=article)
    context = {
        'article': article,
        'form' : form,
    }
    return render(request, 'articles/edit.html', context)
```

```html
<!-- articles/edit.hrml -->
<h1>Edit</h1>
<form action="{% url "articles:update" article.pk %}" method="POST">
  {% csrf_token %}
  {{ form.as_p }}
  <input type="submit">
</form>
```



### ModelForm을 적용한 update 로직

```py
# articles/views.py
def update(request, pk):
    article = Article.objects.get(pk=pk)
    form = ArticleForm(request.POST, instance=article)
    if form.is_valid():
        form.save()
        return redirect('articles:detail', article.pk)
    context = {
        'form': form,
    }
    return render(request, 'articles/edit.html', context)
```



**save() 메서드가 생성과 수정을 구분하는 법**

- 키워드 인자 **instance** 여부를 통해 생성할지, 수정할지를 결정

```py
# CREATE
form = ArticleForm(request.POST)
form.save()

#UPDATE
form = ArticleForm(request.POST, instance=article)
form.save()
```



## 참고

### Widget 응용

```py
class ArticleForm(forms.ModelForm):
    title = forms.CharField(
        label='제목',
        widget=forms.TextInput(
            attrs={
                'class': 'my-title',
                'placeholder': 'Enter the title',
                'maxlength': 10,
            }
        )
    )
    content = forms.CharField(
        label='내용',
        widget=forms.Textarea(
            attrs={
                'class': 'my-content',
                'placeholder': 'Enter the content',
                'rows': 5,
                'cols': 50,
            }
        ),
        error_messages={
            'required': '내용을 입력해주세요.'
        }
    )

    class Meta:
        model = Article
        fields = '__all__'
```



## Handling HTTP requests

### view 함수 구조 변화

#### new & create view 함수간 공통점과 차이점 

**공통점**

- 데이터 생성을 구현하기 위함

**차이점**

- new는 GET method 요청만을, create는 POST method 요청만을 처리



#### request method에 따른 요청의 변화

- (GET) articles/create/	게시글 생성 문서를 줘!
- (POST) articles/create/      게시글을 생성해 줘!



#### 새로운 create view 함수

- new와 create view 함수의 공통점과 차이점을 기반으로 하나의 함수로 결합
- 이후에 기존 new 관련 코드 수정

```py
def create(request):
    # 요청의 메서드가 POST라면 (create)
    if request.method == 'POST':
        form = ArticleForm(request.POST)
        if form.is_valid():
            article = form.save()
            return redirect('articles:index', article.pk)
    
    # 요청의 메서드가 POST가 아니라면 (new) (GET인지 확인하는 것이 아님)
    else:
        form = ArticleForm()

    context = {
        'form': form,
    }
    return render(request, 'articles/new.html', context)
```

- 요청의 메서드가 GET이 아니라 POST인지 확인하는 이유
  - POST일 때의 로직은 오로지 POST일 때만 수행되어야 하기 때문



### 새로운 update view 함수

- 기존 edit과 update view 함수 결합
- 이후에 기존 edit 관련 코드 수정

```py
# articles/views.py

def update(request, pk):
    # 요청의 메서드가 POST라면 (update)
    article = Article.objects.get(pk=pk)
    if request.method == 'POST':
        form = ArticleForm(request.POST, instance=article)
        if form.is_valid():
            form.save()
            return redirect('articles:detail', article.pk)
    # 요청의 메서드가 POST가 아니라면 (edit)
    else:
        form = ArticleForm(instance=article)
    context = {
        'article': article,
        'form' : form,
    }
    return render(request, 'articles/update.html', context)
```

