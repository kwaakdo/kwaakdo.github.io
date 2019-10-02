---
title: Django에서의 Markdown 렌더링과 Markdown 에디터
tags: Django Python Website Markdown
---
장고에서 사용할 수 있는 마크다운 라이브러리에는 [Django Markup](https://pypi.org/project/django-markup/), [Markdown_Deux](https://github.com/trentm/django-markdown-deux) 등등이 존재한다. 개인적으로 markdown_deux의 document가 깔끔하게 만들어져있고, `settings.py`에서의 커스터마이징이 자유롭다고 생각하기 때문에 markdown_deux를 채택했다.

물론, Github Flavored Markdown도 있지만 API 요청의 한계가 있기 때문에 사용하지 않기로 했다.

# markdown_deux
`markdown_deux`는 [python_markdown2](https://github.com/trentm/python-markdown2)을 기반으로 한다. 
## 1) installation
pypi로 최신버전을 설치한다.
```
pip install django-markdown-deux
```

## 2) set up
settings.py의 `INSTALLED_APP`에 `markdown_deux`를 추가한다.

## 3) usage
markdown_deux는 settings.py에서 style을 설정할 수 있다.
### markdown 템플릿 필터
```
{% raw %}

{% load markdown_deux_tags %}
...
{{ myvar|markdown:"STYLE" }}      {# convert `myvar` to HTML using the "STYLE" style #}
{{ myvar|markdown }}              {# same as `{{ myvar|markdown:"default"}}` #}
{% endraw %}
```
아래와 같이 template block 도 가능하다
```
{% raw %}
{% markdown STYLE %}        {# can omit "STYLE" to use the "default" style #}
This is some **cool**
[Markdown](http://daringfireball.net/projects/markdown/)
text here.
{% endmarkdown %}
{% endraw %}
```

### markdown_cheatsheet
```
{% raw %}
{% markdown_cheatsheet %}
{% endraw %}
```

