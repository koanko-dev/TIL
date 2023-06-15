# Django 프로젝트 기본 설정
```bash
# 1. 프로젝트 하기 원하는 경로로 이동

# 2. git 초기화, .gitignore, README.md 파일 생성
git init
touch .gitignore, README.md

# 3. venv 생성
python -m venv venv

# 4. active
source venv/bin/activate

# 5. 프로젝트에 필요한 패키지 설치
pip install django django_extensions

# 6. 버전 정보 저장
pip freeze > requirements.txt

# 7. 현재 위치에 프로젝트 생성
django-admin startproject model .

# 8. 앱 생성
python manage.py startapp orm_test

# 9. 마스터앱의 settings.py -> INSTALLED_APPS에 생성된 앱 추가
```