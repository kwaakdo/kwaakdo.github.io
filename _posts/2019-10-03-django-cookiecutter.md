---
title: 5분만에 Django 프로젝트 만들기 - Django CookieCutter
tags: Django Python Website
---

![image](https://raw.github.com/audreyr/cookiecutter/3ac078356adf5a1a72042dfe72ebfa4a9cd5ef38/logo/cookiecutter_medium.png)

장고는 간편하지만 프로젝트를 시작할 때 마다 초기 설정에 너무 많은 시간을 쓰게 된다. 그리고 초보자들에게는 그 설정조차 난해할 수 있다. [Django CookieCutter](https://github.com/pydanny/cookiecutter-django)는 이와 같은 불편함을 덜어주기 위한 프레임워크이다. 

<!--more-->
# Django CookieCutter

## 1) 특징
* For Django 2.2
* Works with Python 3.6
* Renders Django projects with 100% starting test coverage
* Twitter Bootstrap_ v4 (`maintained Foundation fork`_ also available)
* 12-Factor_ based settings via django-environ_
* Secure by default. We believe in SSL.
* Optimized development and production settings
* Registration via django-allauth_
* Comes with custom user model ready to go
* Optional custom static build using Gulp and livereload
* Send emails via Anymail_ (using Mailgun_ by default, but switchable)
* Media storage using Amazon S3 or Google Cloud Storage
* Docker support using docker-compose_ for development and production (using Traefik_ with LetsEncrypt_ support)
* Procfile_ for deploying to Heroku
* Instructions for deploying to PythonAnywhere_
* Run tests with unittest or pytest
* Customizable PostgreSQL version

## 2) 요구사항
1. [Python 3](https://www.python.org/downloads/)이 설치되어 있어야 한다.
2. [pipenv](https://pypi.org/project/pipenv/)가 설치되어 있어야 한다.

## 3) 설치
```sh
pipenv install cookiecutter
```

### 프로젝트 생성
```sh
cookiecutter https://github.com/pydanny/cookiecutter-django
```
생성을 하게되면 아래와 같은 `options`가 나온다. 자신의 개발 환경에 맞게 선택하면 된다.
```sh
Cloning into 'cookiecutter-django'...
remote: Counting objects: 550, done.
remote: Compressing objects: 100% (310/310), done.
remote: Total 550 (delta 283), reused 479 (delta 222)
Receiving objects: 100% (550/550), 127.66 KiB | 58 KiB/s, done.
Resolving deltas: 100% (283/283), done.
project_name [Project Name]: Reddit Clone
project_slug [reddit_clone]: reddit
author_name [Daniel Roy Greenfeld]: Daniel Greenfeld
email [you@example.com]: pydanny@gmail.com
description [Behold My Awesome Project!]: A reddit clone.
domain_name [example.com]: myreddit.com
version [0.1.0]: 0.0.1
timezone [UTC]: America/Los_Angeles
use_whitenoise [n]: n
use_celery [n]: y
use_mailhog [n]: n
use_sentry [n]: y
use_pycharm [n]: y
windows [n]: n
use_docker [n]: n
use_heroku [n]: y
use_compressor [n]: y
Select postgresql_version:
1 - 11.3
2 - 10.8
3 - 9.6
4 - 9.5
5 - 9.4
Choose from 1, 2, 3, 4, 5 [1]: 1
Select js_task_runner:
1 - None
2 - Gulp
Choose from 1, 2 [1]: 1
Select cloud_provider:
1 - AWS
2 - GCP
3 - None
Choose from 1, 2, 3 [1]: 1
custom_bootstrap_compilation [n]: n
Select open_source_license:
1 - MIT
2 - BSD
3 - GPLv3
4 - Apache Software License 2.0
5 - Not open source
Choose from 1, 2, 3, 4, 5 [1]: 1
keep_local_envs_in_vcs [y]: y
debug[n]: n
```

### 설정
`local dependencies`를 설치해준다.
```sh
pip install -r requirements/local.txt
```
Django는 기본적으로 `SQLite`를 사용한다, 만약 PostgreSQL같은 DB를 현재 설치해놓지 않았다면 아래와 같은 명령어를 통해 `SQLite`를 사용하도록 만들어줘야 한다.
```sh
export DATABASE_URL="sqlite:///db.sqlite"
```

## 4) 실행
```sh
python manage.py migrate
python manage.py createsuperuser
python manage.py runserver
```

---
[Django CookieCutter](https://github.com/pydanny/cookiecutter-django)를 활용해 프로젝트를 생성하기까지 약 10분도 채 걸리지 않는다. 



> 참고 : [https://vsupalov.com/cookiecutter-django-quickstart/](https://vsupalov.com/cookiecutter-django-quickstart/)