# 파이썬 패키지 관리툴 poetry 소개
## poetry 소개
- `peotry` 는 파이썬 의존성 관리 tool
- `peotry.lock` 을 이용해서 프로젝트의 의존성을 다른 환경에서도 동일하게 유지할 수 있도록 해줌

## 설치
### 가상환경 설치
- 우선적으로 가상환경(virtualenv)가 설치되어 있지 않으면 설치하고 activate 하자
- `poetry`를 설치하기 전에 가상환경으로 들어가야 함
- 다음 명령어를 통해 `virtualenv`를 설치하자. pip 명령어가 수행되지 않으면 파이썬에 대한 시스템 환경변수를 설정해야 함
~~~python
pip install virtualenv
~~~

- 일단 가상환경을 설치하고 가동하는 방법은 다음과 같음
~~~python
pip install virtualenv           # virtualenv 설치
mkdir C:/ProgramData1/venv_38    # 해당 디렉토리가 없다면 설치
cd    C:/ProgramData1/venv_38    # 해당 디렉토리로 이동

virtualenv env_38_1 -p C:/ProgramData1/Python/Python38/python.exe 
C:/ProgramData1/venv_38/venv_38_1/Scripts/activate.bat               # 가상환경 기동
~~~  

## poetry 설치
- 가상 환경을 `activate` 한 후에 아래 명령 설치
- 만약 1번 명령어 수행시 SSL 인증 오류가 발생한다면, 두 번째 명령어로 수행
~~~python
pip install poetry
pip install poetry --trusted-host pypi.python.org --trusted-host files.pythonhosted.org --trusted-host pypi.org
~~~

## 프로젝트 셋업
- 아래 명령어로 프로젝트를 셋업하자
~~~python
poetry new my-project
~~~
- 위의 명령으로 프로젝트를 생성하면 아래와 같은 구조의 프로젝트가 생성됨
~~~
my-project tree
.
├── README.rst
├── my_project
│   └── __init__.py
├── pyproject.toml
└── tests
    ├── __init__.py
    └── test_my_project.py
~~~
- `pyproject.toml` 파일이 의존성을 관리하는 파일
- 열어보면 아래와 같이 생김!
~~~toml
[tool.poetry]
name = "my-project"
version = "0.1.0"
description = ""
authors = ["andy.sg"]

[tool.poetry.dependencies]
python = "^3.8"

[tool.poetry.dev-dependencies]
pytest = "^5.2"

[build-system]
requires = ["poetry>=0.12"]
build-backend = "poetry.masonry.api"
~~~
- 의존성은 `tool.poetry.dependencies` 와 `tool.poetry.dev-dependencies` 에서 관리하고 있음
- 의존성을 추가하고 싶다면 `add` 서브 커맨드를 사용하면 됨.

## poetry로 패키지 설치
- `add` 명령어를 통해 패키지 설치
- 만약 error 발생한다면, `requests/adapters.py` 와 `urlib3/connectionpool.py` 수정
- 예를들어 `django` 패키지를 설치하고 싶은 경우, 다음과 같이 명령어를 수행하자
~~~
poetry add django
~~~
- 아래와 같이 출력이 나오게 됨
~~~python
Using version ^3.0.7 for django

Updating dependencies
Resolving dependencies... (7.1s)

Writing lock file


Package operations: 1 install, 9 updates, 0 removals

  - Updating pyparsing (2.4.6 -> 2.4.7)
  - Updating six (1.13.0 -> 1.15.0)
  - Installing asgiref (3.2.7)
  - Updating more-itertools (8.1.0 -> 8.3.0)
  - Updating packaging (20.1 -> 20.4)
  - Updating pytz (2019.3 -> 2020.1)
  - Updating sqlparse (0.3.0 -> 0.3.1)
  - Updating wcwidth (0.1.8 -> 0.2.3)
  - Updating django (2.2.9 -> 3.0.7)
  - Updating pytest (5.3.5 -> 5.4.3)
~~~
- `Writing lock file`에서 생성되는 파일이 바로 `poetry.lock` 파일인데, `poetry.lock` 파일이 있으면 내가 작성하고 있는 프로젝트의 의존성과 완전히 동일한 의존성을 가지도록 할 수 있음  
따라서 반드시 `poetry.lock` 파일을 꼭 저장소에 커밋하자
- `pyproject.toml` 을 보면 아래와 같이 django 의존성이 추가되어 있음
~~~python
[tool.poetry.dependencies]
python = "^3.8"
django = "^3.0.7"
~~~

### 버전 제약사항
python 의존사항을 살펴보면 `^3.8` 이라고 되어 있는데, 여기서의 의미는 `(>3.8 , < 4.0)` 의 의미

### 의존성 최신으로 업데이트 하기
- 아래의 커맨드를 입력하면 됨 `poetry update`
- 위 커맨드는 `poetry.lock` 파일을 삭제후 `poetry install` 하는 것과 동일

## 명령어들
- `new` : 새로운 프로젝트를 만들 수 있음
- `init` : `pyproject.toml` 파일을 인터렉티브 하게 만들 수 있도록 도와줌
- `install` : 현재 프로젝트의 `pyproject.toml` 파일을 읽어서 의존성 패키지를 설치 해줌. `poetry.lock` 파일이 없으면 만들어주고 있으면 해당 파일을 사용하게 됨
- `add`: 패키지 설정을 `pyproject.toml`에 추가함
- `remove` : 패키지 삭제
- `show` : 설치된 모든 패키지를 보여줌
- `check` : `pyproject.toml` 의 유효함을 체크하는 명령어입니다. 
- `search` : 패키지를 찾기 위한 커맨드
- `export` : lock 파일을 사용해서 다른 의존성 파일로 변경

## 가상 환경 관리하기
- poetry 로 가상환경(virtualenv)을 관리 가능
- 일반적으로 아래와 같이 사용
~~~python
poetry env use {파이썬경로}
~~~
- python3 이 패스에 잡혀 있는 상황이라면 모든 경로를 적어주지 않아도 됨

### 가상환경 리스트 보기
- 만들어진 가상환경의 리스트는 아래의 명령어로 확인 가능
~~~python
 poetry env list
~~~

### 가상환경 삭제하기
~~~python
 poetry env remove {python경로}
~~~