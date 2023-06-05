# 코딩 스타일

## 1. 읽기 쉬운 코드
- 축약적이거나 함축적인 변수명은 피한다.
- 함수 인자의 이름들은 꼭 써 준다.
- 클래스와 매서드를 문서화 한다.
- 코드에 주석은 꼭 달도록 한다.
- 재사용 가능한 함수 또는 매서드 안에서 반복되는 코드들은 리팩터링을 해 둔다.
- 함수와 매서드는 가능한 작게, 대충 스크롤 없이 읽을 수 있는 길이가 최고다.

> Ex) 함축적이고 난해한 함수 이름 피할것. 
<br> 
&nbsp; &nbsp; &nbsp; &nbsp; ⛔ bal_s_d 
<br> 
&nbsp;&nbsp; &nbsp; &nbsp; &nbsp;✅ balance_sheet_decrease

---
<br>

## 2. PEP8 스타일
https://peps.python.org/pep-0008/

- 들여쓰기는 스페이스 4개
- 최사우이 함수와 클래스 선어 사이를 구분짓기 위해 두 줄 띄운다
- 클래스 안에서 메서드들을 나누기 위해 한 줄 띄운다. 

> 🤟 Tip) vscode의 경우 black formatter, flake8를 이용하면 쉽게 코드를 관리 할 수 있다.
<br>

### 2.1 79칼럼의 제약

pep8에 따르면 한 줄당 텍스트는 79자를 넘으면 안된다.
이해도를 떨어뜨리지 않는 수준의 줄 길이이기 때문이다.

- 오픈 소스 프로젝트는 79칼럼 제약을 **반드시** 지킨다.
- 프라이빗 프로젝트에 한해서는 99까지 확장하여 최신 모니터의 장점을 살려도 된다.

참고. https://peps.python.org/pep-0008/#maximum-line-length

---
<br>

## 3. Import 규칙

import의 순서는 다음과 같이 그룹을 지을것을 제안한다.
1. 표준 라이브러리 
2. 연관 외부 라이브러리
3. 로컬 애플리케이션 또는 라이브러리에 한정된 임포트

EX)
```
# Stdlib imports
from math import sqrt
from os.path import abspath

# Core Django imports
from django.db import models
from django.utils.translation import gettext_lazy as _

# Third-party app imports
from django_extensions.db.models import TimeStampedModel

# Imports from your apps
from splits.models import BananaSplit
```
✅ 임포트에 대해 주석을 달 필요는 없다.

----
<br>

장고 프로젝트 임포는 순서는 다음과 같다.

1. 표준 라이브러리 
2 코어 장고 
3 장고와 무관한 외부 앱
4. 프로젝트 앱

---
<br>

## 4. 명시적 성격의 상대 import 이용하기

파이썬은 명시적 성격의 상대 import(explicit relative import)를 통해 모듈의 패키지를 하드 코딩하거나 구조적으로 종속된 모듈을 어렵게 분리해야 하는 경우들을 피해 갈 수 있다.

```
# cones/views.py
from django.views.generic import CreateView

#✅ Relative imports of the 'cones' package
from .models import WaffleCone
from .forms import WaffleConeForm

#❌ Bad case Too much hard coding.
from cones.models import WaffleCone 

# absolute import from the 'core' package
from core.views import FoodMixin

class WaffleConeCreateView(FoodMixin, CreateView):
    model = WaffleCone
    form_class = WaffleConeForm
```



|코드|import 타입|용도|
|:------:|:---:|:------:|
|from core.views import FoodMixin|절대 import|외부에서 현재 앱에 이용할 때|
|from .models import WaffleCone|명시적 상대|다른 모듈에서 임포트해서 현재 앱에 이용할 때|
|from models import WaffleCone|암묵적 상대|다른모듈에서 임포트해서 현재앱에 이용 좋은 방법 아님|

---
<br>

## 5. Import *는 피할것

절대 쓰지 말 것<br>
❌ from django.forms import *

이렇게 쓸 것<br>
✅ from django import form

다른 파이썬 모듈의 이름공간들이 현재 작업하는 모듈의 이름공간에 추가로 로딩 되거나
기존 것 위에 덮여 로딩되는 일을 막기 위해 import *을 금지한다.

----
<br>

## 6. 장고 코딩 스타일

장고 공식은 아니지만 일반적으로 널리 통용되는 코딩 스타일
<br>

### 6.1 장고 코딩
공식 코딩 스타일
https://docs.djangoproject.com/en/1.8/internals/contributing/writing-code/coding-style/

<br>

### 6.2 URL 패턴 이름에는 대시(-) 대신 밑줄(_)이용
❌ Bad Case
```
patterns = [
    path(route='add/',
        view=views.add_topping,
        name='add-topping'),
    ]
```
<br>
✅ Good Case

```
patterns = [
    path(route='add/',
        view=views.add_topping,
        name='toppings:add_topping'),
    ]
```






