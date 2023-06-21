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
    # ...
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
