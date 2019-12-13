---
title: Django 모델을 Json으로 출력하는 방법
tags: Django Python Website API
---
[Django Restful Framework](https://www.django-rest-framework.org/)를 사용하는 경우가 아니라면 프론트엔드와 데이터를 주고 받기 위해 Json Response를 직접 구현하여야 한다. 대부분의 출력은 `Django 템플릿 언어(DTL)`와 `jinja2`를 활용하면 되지만, Javascript 와의 통신과 유동적 프로그래밍을 하기에는 부족하다고 생각한다. Javascript를 HTML 내부에 직접 태깅하여 하드코딩하는 방법도 있는데 아무래도 좋은 코드라 할 수 없다.

<!--more-->
---
# HttpResponse vs. JsonResponse
Json을 출력하는 방법에는 크게 두 가지가 있다. `HttpResponse`와 `JsonResponse`다. 이 두 가지의 차이점부터 얘기하고 넘어가도록 하겠다.

우선, JsonResponse는 Automatic Serialize를 지원한다. 
``` python 
return JsonResponse({"key": "value"})
```
단순하게 위와 같이 사용해도 출력이 된다. Dict타입을 넘겨주면 Json으로 Response해주는 방식이다.

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

## 솔루션 2: JsonResponse
``` python
from django.http import JsonResponse
return JsonResponse({'foo':'bar'})
```
[JsonResponse](https://docs.djangoproject.com/en/dev/ref/request-response/#jsonresponse-objects)는 Dict타입의 데이터를 Json으로 응답해준다.

``` python
from django.http import JsonResponse

def view_name(request):
data = list(Model.objects.values())
return JsonResponse(data, safe=False)
```
data = list(Model.objects.values())
: Queryset은 Json으로 Serialize할 수 없기때문에 list타입으로 변환해준다.

# 마치며.
프로젝트를 진행하면서 필요했던 것들에 대한 포스팅을 하는 중이다. 메모가 주 기능이라 생각하지만 다른 개발자들이 좀 더 빠르게 원하는 정보를 얻게 하려는 의도도 있다. Stack Overflow나 외국 문서들을 찾아보는 것은 시간이 많이 소요될 수 있기 때문이다.
