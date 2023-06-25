# Django ModelForm
모델을 생성할 때, 변수의 타입과 제한(ex. max_length)을 주는 것은 사실 유효성 검사에 직접적인 영향을 주는 것이 없습니다.  
ModelForm을 사용한다면, 모델에서 설정한 타입과 제한에 맞게 데이터를 받고 유효성 검사를 한 뒤 저장할 수 있습니다.  

ModelForm은 데이터에 대한 폼을 생성할 수 있고, 검증 필터의 역할은 한다고 생각하면 좋습니다.  
ModelForm은 모델 클래스와 연동해 사용할 수 있습니다. ModelForm이 없다면, 모델에 적힌 제한들은 사실상 모두 쓸모가 없게 됩니다. 스키마를 작성하는 것은 실제로 제한을 주는 역할을 전혀 하지 않기 때문입니다.  

하나의 모델에 하나의 ModelForm이 매핑됩니다.  
이 ModelForm을 통해 폼을 생성하면 html에서 한번에 인풋창을 생성할 수 있습니다. 그리고 검증 필터의 역할을 할 수 있게 됩니다.  

<br>
<br>

## ModelForm 생성
앱 내부에 forms.py를 생성합니다.  
이 파일 안에서 ModelForm을 생성 한 뒤 모델을 연결하는 작업을 할 것입니다.  

forms.py
```python
from django import forms
from .models import Student

class StudentForm(forms.ModelForm):

    class Meta:
        model = Student
        fields = '__all__'
```

views.py 파일에서는 폼을 생성해 html의 `<input>`을 쉽게 랜더링할 수 있습니다.  
생성한 폼을 컨텍스트 딕셔너리로 넘겨서 사용하면 됩니다.  

views.py
```python
from django.shortcuts import render, redirect
from .models import Student
from .forms import StudentForm

def new(request):
    form = StudentForm()
    return render(request, 'school/new.html', {
        'form': form,
    })
```

new.html
```html
<form action="{% url 'school:create' %}" method="POST">
    {% csrf_token %}

    {{ form }}

    <button>Submit</button>
</form>
```
이렇게 연결해주면 모든 폼이 자동으로 생성됩니다.  
모든 `<input>` 요소를 `<p>`로 감싸고 싶다면, `{{ form.as_p }}`를 입력하면 됩니다.  

개발자도구로 `<input>`을 보면 인풋 타입과 maxlength 값, required도 자동으로 들어가는 것을 볼 수 있습니다.  
모델에서 설정한 것은 그 자체로만은 쓸모 없지만, ModelForm에서 이 값을 가져와 인풋을 생성할 때 제한값을 설정해 `<input>`을 생성합니다.  
제한값은 `<input>` 요소로 나타내지는 것 뿐만 아니라, 이 제한값을 바탕으로 실제 데이터가 저장될 때 유효성이 체크됩니다. **다시 말하지만, 모델폼 없이 모델에 쓰여진 제한은 유효성을 체크할 수 없습니다.**  

<br>
<br>

## ModelForm에서의 override
모델에서 작성한 제한을 그대로 사용하지 않으려면, ModelForm에서 직접 override해 설정할 수 있습니다.  
모델에서는 `min_length` 같은 설정을 줄 수 없지만, 모델 폼은 다양한 제한을 설정할 수 있는 기능을 가지고 있습니다.  
모델폼 안에서 override 하는 이유가 이것입니다.  
```python
class StudentForm(forms.ModelForm):

    # override
    age = forms.IntegerField(min_value=14, max_value=70)

    class Meta:
        model = Student
        fields = '__all__'
```

<br>
<br>

## new, create 메서드 합치기(edit, update)
html에서 form 태그에 action이 없으면 바로 직전에 요청을 보냈던 곳으로 요청을 보내게 됩니다.  
이 원리를 이용해, 같은 url, 메서드를 공유해 get, post 만 구분하여 사용할 수 있습니다.    

아래의 두 메서드를 하나의 메서드로 관리할 수 있습니다.  
새로운 폼을 불러오는 new와 폼을 저장하는 create 메서드입니다.  
```python
def new(request):
    # 새로운 html 생성용 빈 form
    form = StudentForm()
    return render(request, 'school/new.html', {
        'form': form,
    })

def create(request):
    # 사용자가 제출한 데이터 검증용 form
    form = StudentForm(request.POST)

    if form.is_valid():
        # 유효성 검사를 해야만 저장이 가능합니다.
        # 폼을 저장하면 저장된 인스턴스가 반환됩니다.
        student = form.save()
        return redirect('school:detail', student.pk)
    else:
        return render(request, 'school/new.html', {
            'form': form,
        })
```
위의 두 메서드를 아래 코드와 같이 하나의 메서드로 관리할 수 있습니다.  
```python
def create(request):
    if request.method == 'GET':
        form = StudentForm()
    
    elif request.method == 'POST':
        form = StudentForm(request.POST)

        if form.is_valid():
            student = form.save()
            return redirect('school:detail', student.pk)
    
    return render(request, 'school/form.html', {
        'form': form,
    })
```     

아래의 메서드 또한 편집할 항목을 불러오는 메서드와(edit), 실제로 데이터를 업데이트하는 메서드(update)를 하나의 메서드로 관리하는 메서드입니다.  
```python
def update(request, student_pk):
    student = Student.objects.get(pk=student_pk)

    if request.method == 'GET':
        # ModelForm에 `instance=` 속성으로 인스턴스 값을 넣어줄 수 있습니다.
        form = StudentForm(instance=student)
        return render(request, 'school/edit.html', {
            'form': form
        })
    
    elif request.method == 'POST':
        # 이미 존재하는 인스턴드를 업데이트하려면, 인스턴스 값을 넣어줘야 합니다.
        form = StudentForm(request.POST, instance=student)

        if form.is_valid():
            student = form.save()
            return redirect('school:detail', student.pk)
        else:
            return render(request, 'school/edit.html', {
                'form': form,
            })
```
더 간단하게 바꾸면 아래와 같습니다.  
```python
def update(request, student_pk):
    student = Student.objects.get(pk=student_pk)

    if request.method == 'GET':
        form = StudentForm(instance=student)
    
    elif request.method == 'POST':
        form = StudentForm(request.POST, instance=student)

        if form.is_valid():
            student = form.save()
            return redirect('school:detail', student.pk)
    
    return render(request, 'school/form.html', {
        'form': form
    })
```

위의 작업으로 create와 update는 같은 html을 공유할 수 있게 되었습니다.  
form.html
```html
{% extends 'base.html' %}

{% block content %}
    <form method="POST">
        {% csrf_token %}

        {{ form.as_p }}

        <button>Submit</button>
    </form>
{% endblock content %}
```

<br>
<br>

## 추가 개념
### 1. html 컴포넌트화
`{% include %}`를 통해 html을 컴포넌트화 하여 관리할 수 있습니다.  
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
    {% include 'navbar.html' %}
    
    {% block content %}
    {% endblock content %}
</body>
</html>
```

<br>

### 2. 함수 데코레이터를 통해 HTTP method 허용
`@require_safe`, `@require_POST`, `@require_http_methods(['GET', 'POST'])` 데코레이터를 사용하면 간편하게 HTTP method에 대한 제한을 줄 수 있습니다.  
```python
from django.views.decorators.http import require_safe, require_POST, require_http_methods

# request가 get, post 요청일때만 동작하고 나머지 요청은 무시합니다.
@require_http_methods(['GET', 'POST'])
def create_article(request):
    if request.method == 'GET':
        form = ArticleForm()
        
    elif request.method == 'POST':
        form = ArticleForm(request.POST)
        if form.is_valid():
            article = form.save()
            return redirect('board:article_detail', article.pk)
        
    return render(request, 'board/form.html', {
        'form': form,
    })

# @require_safe는 @require_http_methods(['GET', 'HEAD'])와 같습니다.
@require_safe
def article_index(request):
    articles = Article.objects.all()
    return render(request, 'board/index.html', {
        'articles': articles,
    })

# request가 post 요청일 때만 동작합니다.
@require_POST
def delete_article(request, article_pk):
    article = get_object_or_404(Article, pk=article_pk)
    article.delete()
    return redirect('board:article_index')

# ...
```

<br>

### 3. 단일 레코드 검색시, 없는 경우 Response Status Code를 '500'에서 '404'로 수정
해당 값이 없을 때 처리를 위해 `get_object_or_404()`를 사용합니다.  
`get_object_or_404()`를 사용하려면,
- 첫번째 인자로 인스턴스를 넣어주고, 두번째 인자로 조건을 넣어주면 됩니다.
- 해당 값이 있으면 반환되고, 없으면 404에러 처리를 합니다.  

```python
from django.shortcuts import render, redirect, get_object_or_404

@require_safe
def article_detail(request, article_pk):
    # article = Article.objects.get(pk=article_pk)
    # 값이 없을 경우를 대비해 아래와 같이 수정합니다.
    article = get_object_or_404(Article, pk=article_pk)
    return render(request, 'board/detail.html', {
        'article': article,
    })
```
에러를 다루기 위해서 사용하는 `HttpResponseNotFound`도 있습니다.  
`HttpResponseNotFound`는 에러 대신 응답 페이지가 전달되고, `get_object_or_404`를 사용하면 내부에 `Http404`가 불려지면서 에러가 띄워집니다.

<br>
