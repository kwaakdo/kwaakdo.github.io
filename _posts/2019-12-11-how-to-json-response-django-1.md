---
title: Django 모델을 Json으로 출력하는 방법
tags: Django Json Serialize
---
[Django Restful Framework](https://www.django-rest-framework.org/)를 사용하는 경우가 아니라면 프론트엔드와 데이터를 주고 받기 위해 Json Response를 직접 구현하여야 한다. 대부분의 출력은 `Django 템플릿 언어(DTL)`와 `jinja2`를 활용하면 되지만, Javascript 와의 통신과 유동적 프로그래밍을 하기에는 부족하다고 생각한다. Javascript도 HTML에 태그하는 방식으로 작성하여 하드코딩하면 되지만, 좋은 코드라 할 수 없다.

<!--more-->
---
# HttpResponse vs. JsonResponse
Json을 출력하는 방법에는 크게 두 가지가 있다. `HttpResponse`와 `JsonResponse`다. 이 두 가지의 차이점부터 얘기하고 넘어가도록 하겠다.

우선, JsonResponse는 Automatic Serialize를 지원한다. 
``` python 
return JsonResponse({"key": "value"})
```
단순하게 위와 같이 사용해도 출력이 된다. 굳이 모델을 Json 형식으로 [Serialize](https://ko.wikipedia.org/wiki/%EC%A7%81%EB%A0%AC%ED%99%94)하는 과정을 거치지 않아도 작동한다. 

HttpResponse의 기본 `header`값은 `Content-Type: text/html; charset=utf-8` 로 되어있다. 아래와 같이 `content_type`을 설정해줄 수 있다.
```python
return HttpResponse("Text only, please.", content_type="text/plain")
```
반면에, JsonResponse는 기본 `header`값이 `Content-Type: application/json`이다.

## 솔루션 1: HttpResponse
``` python
from django.core import serializers
from django.http import HttpResponse

def view_name(request):
    queryset = SomeModel.objects.all()
    queryset_json = serializers.serialize('json', queryset)
    return HttpResponse(queryset_json, content_type='application/json')
```
Django에 내장되어있는 `serializers` 모듈을 사용하여 쿼리셋을 Json으로 `Serialize`한다. 그리고 위에서 언급했듯이 HttpResponse는 기본적으로 `content_type`이 `text/html;`로 설정되어 있기 때문에, `application/json`으로 설정해줘야한다.
