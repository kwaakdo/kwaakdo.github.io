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
단순하게 위와 같이 사용해도 출력이 된다. 굳이 쿼리셋을 Json 형식으로 [Serialize](https://ko.wikipedia.org/wiki/%EC%A7%81%EB%A0%AC%ED%99%94)하는 과정을 거치지 않아도 작동한다. 

반면 HttpResponse는 아래와 같이 `Content-Type`설정에 용이하다는 장점이 있다.
```python
return HttpResponse("Text only, please.", content_type="text/plain")
```



## 솔루션 1: HttpResponse

``` python
values = PCT.objects.filter(code__startswith='a').values()
return HttpResponse(json.dumps(values), content_type='application/json')
```

