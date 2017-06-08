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

## 추가 설치

* pip install django_extensions
* pip install django-shell-plus
* pip install ipython

## 가상환경 현재 상태 남겨두기
```
pip freeze > requirements.txt
```

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
* 소스루트 설정
django_app 우클릭 mark directory -> 소스루트

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

    'app 이름',
    'django_extensions'   // shell에서 자동 import 도와주는
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

## admin 화면 커스터마이징
* blog/admin
```py
from .models import '클래스명'
admin.site.register('클래스명')
// 클래스명 입력후 alt + enter 로 import 가능
```
* blog/models
```py
from django.db import models
from django.utils import timezone

class Post(models.Model):
    author = models.ForeignKey('auth.User')
    title = models.CharField(max_length=200)  // 글상자 200자 넘어가면 오류
    text = models.TextField()
    created_date = models.DateTimeField(
        default=timezone.now
    )
```
//모델의 내용이 변경되면 마이그레이트 해주는게 좋다
*
```py
def __str__(self):
    return self.title   << 관리창에서 뜨는 제목란
```

## url / views
* blog/views
```py
def main_view(request):
    return HttpResponse("보여질 내용")
```

* mysite/urls
```py
from blog import views   // views를 임포트

urlpatterns = [
    url(r'^admin/', admin.site.urls),
    url(r'^$', views.main_view),  // 지정한 함수를 입력
]
```
* mysite/settings - 템플릿 위치 설정(현재 app폴더랑 같은위치)
os.path.dirname // 상위폴더로 가기
```py
BASE_DIR = os.path.dirname(os.path.dirname(os.path.abspath(__file__)))
print(BASE_DIR)
ROOT_DIR = os.path.dirname(BASE_DIR)
print('루트 위치', ROOT_DIR)
TEMPLATES_DIR = os.path.join(ROOT_DIR, 'templates')  // .join 들어가기
print('템플릿 위치', TEMPLATES_DIR)
```

* 스테틱 설정
```py
STATIC_DIR = os.path.join(ROOT_DIR, 'static')
...

STATIC_URL = '/static/'

STATICFILES_DIRS = (
    STATIC_DIR,
)
```

## html 화면 연결

* settings
```
TEMPLATES = [
    {
        'BACKEND': 'django.template.backends.django.DjangoTemplates',
        'DIRS': [
            TEMPLATES_DIR,
            ],
```

* blog/views
```py
def main_view(request):
    return render(request, 'base/base.html')
```
```py
def main_view(request):
    post = Post.objects.all()
    context = {
        'posts': post    // posts라는 키값을 가지는 post
    }
    return render(request, 'base/base.html', context)  // 컨텍스트 추가
           그리다              도화지             내용물
```
키값으로 접근!!!
## block content
* base/base   block 위 아래를 상속
```python
<!doctype html>
<html lang="ko">
<head>
    <meta charset="UTF-8">
    <meta name="viewport"
          content="width=device-width, user-scalable=no, initial-scale=1.0, maximum-scale=1.0, minimum-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>It's my blog</title>
</head>
<body>
{% block content %}

{% endblock %}
</body>
</html>
```

* post/post-add
```python
{% extends 'base/base.html' %}
<h1>다섯 여섯 일곱 여덟</h1>   //내용 안보임
{% block content %}
<h1>하나 둘 셋 넷</h1>  // 내용 보임
{% endblock %}
```

* urls 주소추가
```
url(r'^postadd/', views.post_add_view,)
```

## forms
required=True << 빈값이 있으면 안된다! 는 설정
```
class PostCreationForm(forms.Form):
    title = forms.CharField(max_length=20, required=True)
    text = forms.TextInput()
```

* blog/view
```
def post_add_view(request):
    if request.method == 'GET':

    return render(request, 'post/post-add.html')
```


elif request.method == 'POST':
    form = PostCreationForm(request.POST)

    if form.is_valid():
        author = User.objects.first()
        title = form.cleaned_data['title']  // 딕셔너리로 반환
        text = form.cleaned_data['text']
        Post.objects.create(
            author=author,
            title=title,
            text=text,
        )
    return redirect('post_main')


* post-add
<form action="" method="POST">{% csrf_token %}
{{ forms }}
    <button type="submit">생성</button>
</form>
