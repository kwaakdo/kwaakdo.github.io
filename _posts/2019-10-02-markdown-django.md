---
title: Django에서의 Markdown 렌더링과 Markdown 에디터
tags: Django Python Website Markdown
---


장고에서 사용할 수 있는 마크다운 라이브러리에는 [Django Markup](https://pypi.org/project/django-markup/), [Markdown_Deux](https://github.com/trentm/django-markdown-deux) 등등이 있다. 경험에 의하면 markdown_deux의 document가 깔끔하게 만들어져있고, `settings.py`에서의 커스터마이징이 자유롭다고 생각하기 때문에 markdown_deux를 채택했다.

<!--more-->

물론, Github Flavored Markdown도 있지만 API 요청 횟수에 한계가 있기 때문에 사용하지 않기로 했다.

---

# markdown_deux
`markdown_deux`는 [python_markdown2](https://github.com/trentm/python-markdown2)을 기반으로 한다. 
## 1) 설치
pypi로 최신버전을 설치한다.
```
pip install django-markdown-deux
```

## 2) 설정
settings.py의 `INSTALLED_APP`에 `markdown_deux`를 추가한다.

## 3) 사용법
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
`cheatsheet`태그를 사용하면 템플릿에 마크다운 미리보기를 모두 출력한다. 프론트엔드 디자인 시 유용하게 사용할 수 있다.
```
{% raw %}
{% markdown_cheatsheet %}
{% endraw %}
```

### 익스텐션
`settings.py`에서 아래를 참고하여 여러가지 customize가 가능하다. 
```python
# settings.py
from markdown_deux.conf.settings import MARKDOWN_DEUX_DEFAULT_STYLE

MARKDOWN_DEUX_STYLES = {
    "default": MARKDOWN_DEUX_DEFAULT_STYLE,
    "trusted": {
        "extras": {
            "code-friendly": None,
        },
        # Allow raw HTML (WARNING: don't use this for user-generated
        # Markdown for your site!).
        "safe_mode": False,
    }
    # Here is what http://code.activestate.com/recipes/ currently uses.
    "recipe": {
        "extras": {
            "code-friendly": None,
        },
        "safe_mode": "escape",
        "link_patterns": [
            # Transform "Recipe 123" in a link.
            (re.compile(r"recipe\s+#?(\d+)\b", re.I),
             r"http://code.activestate.com/recipes/\1/"),
        ],
        "extras": {
            "code-friendly": None,
            "pyshell": None,
            "demote-headers": 3,
            "link-patterns": None,
            # `class` attribute put on `pre` tags to enable using
            # <http://code.google.com/p/google-code-prettify/> for syntax
            # highlighting.
            "html-classes": {"pre": "prettyprint"},
            "cuddled-lists": None,
            "footnotes": None,
            "header-ids": None,
        },
        "safe_mode": "escape",
    }
}
```

---

마크다운 에디터는 [simpleMDE](https://github.com/sparksuite/simplemde-markdown-editor)를 추천한다. 위지윅([WYSIWYG](https://ko.wikipedia.org/wiki/%EC%9C%84%EC%A7%80%EC%9C%84%EA%B7%B8)) 방식을 채택하여 사용자 입장에서 편리하다.
![image](https://camo.githubusercontent.com/dd1a40dd1efd202fd3862995b3ecc699282ee540/687474703a2f2f692e696d6775722e636f6d2f7a7157664a774f2e706e67) 
마크다운을 입력하는 즉시 스타일이 적용되서, 직관적이다.

# simpleMDE
설치 방법에는 여러가지가 있다.

## 1) 설치
### npm
```sh
npm install simplemde --save
```

### bower
```sh
bower install simplemde --save
```

### jsDeliver
```html
<link rel="stylesheet" href="https://cdn.jsdelivr.net/simplemde/latest/simplemde.min.css">
<script src="https://cdn.jsdelivr.net/simplemde/latest/simplemde.min.js"></script>
```

## 2) 사용법
Textarea의 ID를 사용하면 된다.
```javascript
<script>
var simplemde = new SimpleMDE({ element: document.getElementById("MyID") });
</script>
```

`Javascript`상에서 Textarea의 값을 설정하거나 불러올 수 있다.
```javascript
simplemde.value();
simplemde.value("This text will appear in the editor");
```

## 3) 커스터마이징
[simpleMDE](https://github.com/sparksuite/simplemde-markdown-editor)를 참고하면 된다.
---

위 두 가지 라이브러리를 사용하면 Markdown 렌더링부터 에디터까지 빠르게 구현이 가능하다.

