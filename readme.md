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
.diea/
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
