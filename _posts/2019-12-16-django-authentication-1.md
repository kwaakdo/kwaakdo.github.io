---
title: Django 인증(Authentication)시스템 사용하기
tags: Django Ptyhon Website
---
# 개요
어떤 웹 사이트를 개발하던 로그인 기능은 거의 필수적이라고 할 수 있다. `Django`에 내장되어있는 인증 시스템은 로그인, 로그아웃부터 비밀번호 변경까지 간편하게 사용할 수 있다. 인증 시스템은 만들어져있지만 템플릿(Templates)는 직접 구현하여야 한다.

이 포스트는 [mozilla](https://developer.mozilla.org/ko/docs/Learn/Server-side/Django/Authentication)의 자료를 바탕으로 만들어졌습니다.{:info }

# Django Authentication
## URL Pattern
프로젝트의 `urlpatterns`에 아래의 코드를 추가한다.
```python
urlpatterns += [
    path('accounts/', include('django.contrib.auth.urls')),
]
```