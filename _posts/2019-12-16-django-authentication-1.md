---
title: Django 인증(Authentication)시스템 사용하기
tags: Django Ptyhon Website
published: false
---
## 개요
어떤 웹 사이트를 개발하던 로그인 기능은 거의 필수적이라고 할 수 있다. `Django`에 내장되어있는 인증 시스템은 로그인, 로그아웃부터 비밀번호 변경까지 간편하게 사용할 수 있다. 인증 시스템은 만들어져있지만 템플릿(Templates)는 직접 구현하여야 한다.

- 이 포스트는 [mozilla](https://developer.mozilla.org/ko/docs/Learn/Server-side/Django/Authentication)의 자료를 바탕으로 만들어졌다.


<!--more-->
# Django Authentication
## URL Pattern
프로젝트의 `urlpatterns`에 아래의 코드를 추가한다.
```python
urlpatterns += [
    path('accounts/', include('django.contrib.auth.urls')),
]
```

Django Authentication은 기본적으로 `URL Mapping`이 되어있다. 
```
accounts/ login/ [name='login']
accounts/ logout/ [name='logout']
accounts/ password_change/ [name='password_change']
accounts/ password_change/done/ [name='password_change_done']
accounts/ password_reset/ [name='password_reset']
accounts/ password_reset/done/ [name='password_reset_done']
accounts/ reset/<uidb64>/<token>/ [name='password_reset_confirm']
accounts/ reset/done/ [name='password_reset_complete']
```
매핑되어있는 URL로 접속해보면, Template Doesn't Exist 에러가 나온다. 알맞은 경로에 Template 생성을 해야한다.

## Templates 생성
템플릿(Templates)은 `settings.py`에서 경로를 지정해 줄 수 있다. 이번 글 에서는 프로젝트 디렉토리로 지정하겠다.

*settings.py*
```python
TEMPLATES = [
    {
        ...
        'DIRS': ['./templates',],
        'APP_DIRS': True,
        ...
```

### Login Template
> 이 글에 나오는 템플릿은 기본적인 기능만 하므로 커스터마이징을 해야 합니다.
> login.html 을 작성하기 이전에 *Project/templates/base_generic.html*을 만들어야합니다.

*Project/templates/base_generic.html*


*Project/templates/registration/login.html*


