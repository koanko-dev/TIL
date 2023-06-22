# Django path 'name'

## `name` 설정으로 url 입력
url 패턴을 작성할 때 `name`을 사용해 해당 경로에 대한 이름을 설정할 수 있습니다.  
이름을 설정한다고 하는 것은 변수를 세팅한다고 생각하면 됩니다.  
이 작업을 해 놓으면, html과 views 파일에 경로를 작성할 때 하드코딩하지 않을 수 있습니다.  

앱의 urls.py 에서 아래와 같이 설정합니다.  
- path를 생성할 때 `name`을 설정해 변수명을 지정
- `app_name` 지정
```python
from django.urls import path
from . import views

app_name = 'board'

urlpatterns = [
    path('create/', views.create, name='create'),
    # ...
]
```
`app_name`을 설정하지 않고 `name`만 사용할 수도 있습니다.  
하지만 `app_name`을 지정함으로써, `name`에 대한 namespacing 이슈 또한 사라지기에 지정하는 것이 안전합니다.  
여기에서 `app_name`은 장고가 인지하는 특수한 변수 이름입니다. 따라서 반드시 변수명은 `app_name`을 사용해야 합니다.  

<br>

이제 html 파일에 경로를 설정할 때, 하드코딩하지 않고 DTL인 `url`과 함께 편하게 사용할 수 있습니다.  

(기존 코드)
```html
<a href="/board/">...</a>

<a href="/board/{{article.pk}}">...</a>
```
(변경 코드)
```html
<a href="{% url 'board:index' %}">...</a>

<a href="{% url 'board:detail' article.pk %}">...</a>
```

<br>
