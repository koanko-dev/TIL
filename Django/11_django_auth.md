# Django Auth
프레임워크들은 회원관리 기능까지 내장하고 있는 경우가 별로 없습니다. 반면, 장고는 회원관리 기능을 내장하고 있습니다. 이는 장고가 신문사에서부터 탄생한 배경과 관련이 있습니다.  

회원관리 즉, Auth가 의미하는 바는 크게 두가지로 나뉩니다.  
- Authentication: 확인, 인증 => sign in  
- Authorization: 등록, 권한 부여 => sign up  

보통 장고는 `accounts` 라는 이름의 앱으로 auth를 관리하는 게 컨벤션입니다.

장고 프로젝트를 생성하면, settings.py의 `INSTALLED_APPS`에 자동으로 작성되어 있는 앱들이 있습니다.  
그 안에 `django.contrib.auth`가 있는 것을 확인할 수 있습니다. 이는 장고가 제공하는 auth 입니다.  
`INSTALLED_APPS`에 적힌 모든 앱들의 모델을 대상으로 migrations를 생성하는 `python manage.py makemigrations`를 입력하면, `django.contrib.auth` 를 포함한 장고가 제공하는 기본 앱들(admin, sessions 등등)에 대한 migrations가 생성됩니다.  

auth에 대한 migrations를 생성하기 전에 해야 할 일이 있습니다.  
`django.contrib.auth`를 그대로 가져다 쓰면 확장성에 문제가 있기 때문에, accounts 앱을 생성해 상속한 클래스로 작업을 해야 합니다.  

<br>
<br>

## accounts 앱 생성 및 설정
상속된 클래스로 스키마를 작성하기 위해 `AbstractUser`를 사용할 수 있습니다.
models.py(accounts app)
```python
from django.db import models
from django.contrib.auth.models import AbstractUser

class User(AbstractUser):
    pass
```
이 모델을 프로젝트의 총괄 회원관리 모델로 설정하기 위해 settings 파일에서 아래와 같이 코드를 추가합니다.  
settings.py
```python
AUTH_USER_MODEL = 'accounts.User'
```
총괄 회원관리 모델은 accounts 앱의 User모델이라고 선언했습니다.  
장고 프로젝트를 시작하면 settings 파일에서 AUTH_USER_MODEL을 찾아볼 수 없습니다. settings 파일에서 볼 수는 없지만, 장고 코드 내부에 `AUTH_USER_MODEL = "auth.User"`로 설정되어 있기 때문에 settings 파일에 따로 설정하지 않은 상태라면 장고가 제공하는 그대로의 auth.User를 사용하게 됩니다.  

**장고는 장고가 기본적으로 제공하는 auth가 당장 사용하기에 충분하더라도, 확장성을 위해 AbstractUser을 사용해 따로 관리하는 것을 강하게 추천하고 있습니다.**  
따라서, 제공되는 auth를 그대로 사용하는 것은 좋지 않은 습관이 될 수 있습니다.  

<br>
<br>

## User 모델 폼 생성
User 모델 폼은 다른 모델 폼을 생성하는 방식과는 조금 다릅니다.  
UserCreationForm을 가져와 상속받아 사용합니다.
```python
from django.contrib.auth.forms import UserCreationForm
from django.contrib.auth import get_user_model

class CustomUserCreationForm(UserCreationForm):

    class Meta:
        model = get_user_model()
        fields = ('username',)
```
모델을 지정할 때도 직접 모델명을 작성하지 않고 get_user_model을 사용합니다.  
`get_user_model`은 알아서 settings의 `AUTH_USER_MODEL`에서 설정된 모델을 가져옵니다. 위에 작성한 것처럼, settings에서 `AUTH_USER_MODEL = 'accounts.User'`을 설정했던 이유가 이것입니다.  
모델을 직접 입력하는 것보다 `get_user_model`을 사용하는 것을 장고는 권장합니다. 후에 변경될 가능성이 있기 때문입니다.  

<br>
<br>

## Session / Cookie 인증 방식
브라우저의 개발자도구 Aplication -> Cookies 에 들어가면 쿠키들을 볼 수 있습니다.  
세션 id가 발급되면 유저의 브라우저 쿠키 자리에 세션 id가 저장됩니다. 이 브라우저를 a 라고 합시다. 유저가 a 브라우저에서 이 쿠키를 삭제하면 로그아웃이 되고, 만약 다른 브라우저인 b에서도 로그인 되어 있는 상태였다면 b 브라우저에서도 자동으로 로그아웃 될 것입니다.  
로그아웃 하면서 세션이 삭제되는 것이기 때문에, b 브라우저에서 여전히 세션 id를 담은 쿠키를 들고 있어도 유효하지 않게 되는 것입니다.  

세션은 django_session 테이블에서 볼 수 있습니다.  
django_session 테이블에는 로그인 되어 있는 유저 세션들이 들어가게 됩니다. 이 테이블은 `session_key`, `session_data`, `expire_date`로 구성됩니다.  
`session_key`는 사용자에게 보내져 쿠키 안에 담게 함으로써, 요청이 있을 때마다 확인하는 역할로 쓰입니다.  
`session_data`는 디코드하면 아래와 같은 값이 나옵니다.  
```python
{
    '_auth_user_id': '4',
    '_auth_user_backend': 'django.contrib.auth.backends.ModelBackend',
    '_auth_user_hash': '5a962beac7b9efe02ea994262e7c1aec97762305cd6d4e316845867889afc46b'
}
```
`_auth_user_id`는 user id를 나타내며, `_auth_user_hash`는 user password를 해시한 값입니다.  
`_auth_user_hash` 값은 유저가 비밀번호를 변경하면 따라 변경됩니다.  
따라서 변경 전의 `session_data`를 가지고 있는 경우, 해당 세션은 유효하지 않게 됩니다.  

<br>
<br>

## views 에서 User를 다루는 방법
회원가입에서는 생성한 `CustomUserCreationForm`을 사용해 기존에 모델폼으로 폼을 생성하듯 화면을 생성하고 유저 정보를 저장하면 됩니다.

로그인에서는 장고가 제공하는 `AuthenticationForm`을 사용합니다.

```python
from django.shortcuts import render, redirect
from django.views.decorators.http import require_http_methods, require_POST

# auth 관련
from django.contrib.auth.forms import AuthenticationForm
from django.contrib.auth import login as auth_login, logout as auth_logout

from .forms import CustomUserCreationForm

@require_http_methods(['GET', 'POST'])
def signup(request):
    if request.method == 'GET':
        form = CustomUserCreationForm()

    else:
        form = CustomUserCreationForm(request.POST)
        if form.is_valid():
            user = form.save()
            auth_login(request, user)

            return redirect('home')
    
    return render(request, 'accounts/signup.html', {
        'form': form,
    })

@require_http_methods(['GET', 'POST'])
def login(request):
    if request.method == 'GET':
        # 빈 폼을 생성합니다.
        form = AuthenticationForm()
    else:
        # 유저가 입력한 auth 정보를 담은 AuthenticationForm을 생성합니다.
        form = AuthenticationForm(request, data=request.POST)
        if form.is_valid():
            # AuthenticationForm은 get_user 메서드가 있습니다.
            # 이것으로 폼의 정보와 일치하는 사용자를 가져올 수 있습니다.
            user = form.get_user()
            auth_login(request, user)

            return redirect(request.GET.get('next') or 'home')

    return render(request, 'accounts/login.html', {
        'form': form,
    })

# 로그아웃은 하면 할수록 보안에 좋기 때문에, 아무 데코레이터를 달지 않는 경우가 많습니다.
def logout(request):
    auth_logout(request)
    return redirect('home')
```

<br>
<br>

## 모델 스키마 작성시 user
유저의 경우, ForeignKey를 조금 다르게 사용합니다.  
모델끼리의 관계를 설정할 때, 예를 들어 아티클의 저자 정보를 저장할 때 모델을 그대로 가져오는 방법이나 `get_user_model`을 쓰지 않고 `settings.AUTH_USER_MODEL`을 사용해야 합니다.  
```python
class Article(models.Model):
    user = models.ForeignKey(settings.AUTH_USER_MODEL, on_delete=models.CASCADE)
    # ...
```
정리하면, User 클래스를 사용할 경우
1. 모델간 관계설정시에는 `settings.AUTH_USER_MODEL` 을 사용합니다.(위 코드)  
2. 그 외(form 생성등)에는 `get_user_model()` 함수를 사용합니다.  

<br>
<br>

## DB 초기화
DB 초기화하기에 가장 심플하고 강력한 방법은 `db.sqlite3`을 지우는 것입니다.  
이 방법은 sqlite를 사용할때만 가능합니다. sqlite3는 파일로 DB를 가지고 있기 때문입니다.  
```bash
rm db.sqlite3
rm <app-name>/migrations/0*  # 앱 내부에 0으로 시작하는 파일은 모두 지웁니다.
```
그다음 모델 재정비가 완료되면 -> makemigrations -> migrate 순서로 진행합니다.  

<br>
<br>

## 로그인 상태에 따른 ui 변경
settings 파일의 `TEMPLATES` 설정에 `context_processors`를 보면, `debug`, `request`, `auth`, `messages`가 작성되어 있는데, 이것들은 컨텍스트로 실어보내지 않아도 자동으로 프로세싱되는 것들을 나타냅니다.  
따라서 html에서 DTL로 바로 사용할 수 있습니다.  

그 외에 `user` 키워드도 가능합니다. `user`는 지금 요청을 보내는 사용자의 정보를 보여줍니다.  
`user`를 사용해 html에서 로그인 된 경우와 그렇지 않은 경우에 따라 다른 버튼을 보여줄 수 있습니다.  
DTL의 `if`를 사용하여 조건에 따라 다르게 출력되도록 설정합니다.
```html
<nav>
  <ul>
    <li>
      <a href="{% url 'home' %}">Home</a>
    </li>

    {% if user.is_authenticated %}
        <li>
        <a href="{% url 'board:create_article' %}">New</a>
        </li>
        <li>
        <a href="{% url 'accounts:logout' %}">Logout</a>
        </li>
    {% else %}
        <li>
        <a href="{% url 'accounts:signup' %}">Signup</a>
        </li>
        <li>
        <a href="{% url 'accounts:login' %}">Login</a>
        </li>
    {% endif %}
  </ul>
</nav>
```

<br>
<br>

## 로그인 상태에 따른 기능 제한
로그인 되어있는 유저만 아티클을 작성하게 하고 싶다면,
```python
@require_http_methods(['GET', 'POST'])
def create_article(request):
    # 인증된 사용자가 아니면 home으로 보냅니다.
    if not request.user.is_authenticated:
        return redirect('home')
    # ...
```
위와 같이 할 수 있지만, 장고에서는 아래와 같은 데코레이터를 제공합니다.
```python
# ...
from django.contrib.auth.decorators import login_required

@require_http_methods(['GET', 'POST'])
@login_required
def create_article(request):
    # ...
```
이 `@login_required` 데코레이터를 사용하면, 로그인 안된 사용자를 로그인 창으로 넘기고 로그인이 완료되면 이전 페이지인 아티클을 생성하는 페이지로 사용자를 바로 넘겨줄 것입니다.  
이것이 가능한 이유는 쿼리파라미터로 `?next=<아티클_생성_경로>`를 보내주기 때문입니다.  
그럼 login 기능에서 이 정보를 활용해 redirect 해줄 수 있습니다.
```python
@require_http_methods(['GET', 'POST'])
def login(request):
    if request.method == 'GET':
        form = AuthenticationForm()
    else:
        form = AuthenticationForm(request, data=request.POST)
        if form.is_valid():
            user = form.get_user()
            auth_login(request, user)

            # 전달받은 퀴리파라미터에 담긴 'next'의 값으로 redirect합니다.
            # 퀴리파라미터에 'next'가 없다면, 'home'으로 redirect합니다.
            return redirect(request.GET.get('next') or 'home')

    return render(request, 'accounts/login.html', {
        'form': form,
    })
```

<br>
