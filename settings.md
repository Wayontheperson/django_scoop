# Settings

- VC(version control)으로 모든 설정 파일을 관리 해야 한다.
    - 특히 날짜 시간 등 세팅 변화에 대한 기록이 반드시 문서화 되어야 한다.

- 반복되는 설정들을 없애야 한다.
    - 이곳저곳에 복붙하기보단 기본 세팅 파일로부터 상속을 통해 이용한다.
- 암호및 비밀 키 등은 안전하게 보관한다.
    -   VC에 들어가면 안된다.

> 📌 Warning <br>
&nbsp; &nbsp; &nbsp; &nbsp; SECRET_KEY 세팅은 무조건 감추어야 한다.

<br>
<br>

## 1. 어떻게 settings.py를 관리할까?
보안, 개발, 스테이징, 테스트 등을 위해 여러 settings파일을 이용하자

settings/ 디렉토리를 만들어 여러개의 셋업파일을 구성하여 이용한다.
```
settings/
    __init.py__.py
    base.py
    local.py
    staging.py
    test.py
    production.py
```


✅ How to use settings
base.py
```
(... 생략 ...)
BASE_DIR = Path(__file__).resolve().parent.parent.parent
(... 생략 ...)
```

✅ others (local.py, test.py etc...)
```
from .base import *

ALLOWED_HOSTS = []
```

✅ settings file 적용방법
```
# runserver 할떄
python manage.py runserver --settings=[project_path].settings.local

# shell 이용 시
python manage.py shell --settings=[project_path].settings.local
```

<br>
<br>

## 2. 코드에서 설정 분리하기

환경변수에 비밀키 등을 넣어 관리한다. <br>
> ❗ TIP <br>
&nbsp; &nbsp; &nbsp; &nbsp; 환경변수를 아파치와 같이 이용하면 안된다. 아파치는 스스로 독립적인 환경 변수 시스템을 가지고 있기 때문이다.

<br>
<br>

## 3.로컬 환경에서 환경 변수 셋팅하기

bash를 이용하는 맥과 리눅스의 경우 다음 구문을 bashrc, bash_profile, .profile의 뒷부분에 추가하면 된다.
<br>
또는 같은 API를 이용하는 여러개의 프로젝트를 서로 다른 API키를 이용하여 작업한다고 하면 다음 구문을 가상혼경의 /bin/activate 스크립트의 맨 마지막 부분에 넣어준다.

```
export SOME_SECRET_KEY=1c3-cr3am-15-yummy
export AUDREY_FREEZER_KEY=y34h-r1ght-d0nt-t0uch-my-1c3-cr34m
```

윈도우의 경우
```
set SOME_SECRET_KEY 1c3-cr3am-15-yummy
```

또는 bin/activate.bat 스크립트 맨 마지막에 넣어준다.

<br>
<br>

## 4. 운영 환경에서 환경 변수를 셋팅하기

서버에서 이용하고 있는 배포 도구에 따라 달라 진다.
만약 PaaS를 기반으로 배포된다면 다음과 같이 할 수 있다.

```
# heroku 예시
heroku config:set SOME_SECRET_KEY=
```

파이썬 내부 모듈에서 환경 변수를 읽어 오는 방법

```
import os
SOME_SECRET_KEY = os.environ['SOME_SECRET_KEY']
```


## 5. 환경 변수를 사용하지 않는 방법

아파치 웹서버의 경우 환경 변수를 이용할수 없으며, nginx 기반 환경에서도 특정경우에 한해 환경 변수를 이용할 수 없다.
이럴 경우 **비밀 파일 패턴(secrets file pattern)**을 이용한다.

### 5.1 비밀 파일 패턴 구현
1.  JSON, Config, YAML, XML 중 한 가지 포맷을 선택하여 비밀 파일을 생성한다.
2. 비밀 파일을 관리하기 위한 비밀 파일 로더(ex. JSON)를 간단하게 추가한다.
3. 비밀파일의 이름을 반드시 .gitignore 또는 .hgignore에 추가한다.

<br>

✨ 비밀파일 로더 예시
```
# secrets.json

{
  "FILENAME": "secrets.json",
  "SECRET_KEY": "I've got a secret!",
  "DATABASES_HOST": "127.0.0.1",
  "PORT": "5432"
}
```
위 파일을 settings에서 읽도록 한다.

```
# settings/base.py
# 일반적으로 setting파일에는 절대 다른 것을 import 하면 안된다.
# 단, ImproperlyConfigured는 예외적으로 허용한다.

import json

from django.core.exceptions import ImproperlyConfigured

# JSON-based secrets module
with open('secrets.json') as f:
    secrets = json.load(f)

def get_secret(setting, secrets=secrets):
    '''Get the secret variable or return explicit exception.'''
    try:
        return secrets[setting]
    except KeyError:
        error_msg = 'Set the {0} environment variable'.format(setting)
        raise ImproperlyConfigured(error_msg)

SECRET_KEY = get_secret('SECRET_KEY')
```


>🤟 TIP <br>
&nbsp; &nbsp; &nbsp; &nbsp;  **절대 setting 파일에 절대경로로 하드 코딩하지 말것❗**

❌ 나쁜 예

```
# settings/base.py

# Configuring MEDIA_ROOT
# DON’T DO THIS! Hardcoded to just one user's preferences
MEDIA_ROOT = '/Users/pydanny/twoscoops_project/media'

# Configuring STATIC_ROOT
# DON’T DO THIS! Hardcoded to just one user's preferences
STATIC_ROOT = '/Users/pydanny/twoscoops_project/collected_static'

# Configuring STATICFILES_DIRS
# DON’T DO THIS! Hardcoded to just one user's preferences
STATICFILES_DIRS = ['/Users/pydanny/twoscoops_project/static']

# Configuring TEMPLATES
# DON’T DO THIS! Hardcoded to just one user's preferences
TEMPLATES = [
    {
        'BACKEND': 'django.template.backends.django.DjangoTemplates',
        DIRS: ['/Users/pydanny/twoscoops_project/templates',]
    },
]
```

✅ 좋은 예

```
# At the top of settings/base.py
from pathlib import Path

BASE_DIR = Path(__file__).resolve().parent.parent.parent
MEDIA_ROOT = BASE_DIR / 'media'
STATIC_ROOT = BASE_DIR / 'static_root'
STATICFILES_DIRS = [BASE_DIR / 'static']
TEMPLATES = [
    {
        'BACKEND': 'django.template.backends.django.DjangoTemplates',
        'DIRS': [BASE_DIR / 'templates']
    },
]
```









