## 가상환경 생성
* 사용할 폴더 생성
```
mkdir djangogirls
cd djangogirls
```
* 가상환경 생성
```
pyenv virtualenv <version> <env_name>
pyenv virtualenv 3.5.3 Django
```
* 가상환경 현재폴더에 적용
```
pyenv local <env_name>
pyenv local Django
```

* 가상환경 적용 확인
```
pyenv version
```

## git 생성
* 생성
```
git init
```

* 리모트 주소
```
git remote add origin <깃허브 주소>
```

* 주소확인
```
git remote -v
```

* gitignore 세팅
```
https://www.gitignore.io/
작업환경에 맞게 선택
Linux, Python, Pycharm, Django

vi .gitignore 에 내용 붙여넣기
.idea/
.swp
```

---
## 장고 설치
* pip install django
---

## 프로젝트 생성
* django-admin startproject mysite
```
└── djangogirls
    └── mysite
        ├── manage.py
        └── mysite
            ├── __init__.py
            ├── settings.py
            ├── urls.py
            └── wsgi.py
```
* 해깔리는 반복주소 변경
```
mv mysite django_app
```

* 현재경로 확인
```
pwd
```

## 파이참 연결
* 새 프로젝트
djangogirls 폴더에서 `charm .`
* 환경설정
```
File > Settings > Project: practice > project interpriter
```
```
* add Local

 /home/<유저명>/.pyenv/versions/<가상환경 명>/bin/python

```

## 블로그 어플리케이션 생성
* django_app 폴더에서
```
./manage.py startapp blog
```
* mysite/settings.py 수정
```
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'blog',
]
```
## 블로그 글 모델 만들기
* blog/models.py
```python
from django.db import models
from django.utils import timezone


class Post(models.Model):
    author = models.ForeignKey('auth.User')
    title = models.CharField(max_length=200)
    text = models.TextField()
    created_date = models.DateTimeField(
            default=timezone.now)
    published_date = models.DateTimeField(
            blank=True, null=True)

    def publish(self):
        self.published_date = timezone.now()
        self.save()

    def __str__(self):
        return self.title
```
## admin ragister 변경
* blog/admin.py
```
from django.contrib import admin
from .models import Post

admin.site.register(Post)
```

## 모델을 위한 테이블 만들기
* ./manage.py migrate
* ./manage.py makemigrations
```
우리가 변경한 내용을 DB에서 사용가능하게
```

* ./manage.py migrate
```
다시 적용하기위해 migrate 명령어 다시 실행
```

## 장고 관리자
* 서버열기
```
python manage.py runserver
```
 http://127.0.0.1:8000/admin/

 * 슈퍼유저 생성
```
./manage.py createsuperuser

Username:
Email address:
Password:
Password (again):
Superuser created successfully.
```
