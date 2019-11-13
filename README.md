# Python
![Python기본그림](image/python.png)
## 개발 환경 셋팅
---
### 설치
http://www.python.org/downloads 에서 다운로드  
현재(2019.10.30 기준) 3.8.0 (stable)까지 나와 있음  
![Python설치그림](image/python_install01.png)

> PATH 잡아서 설치 하기
> ![Python설치그림](image/python_install02.jpg)
---

### pip
Python Package Index(PyPI)에서 제공하는 Python Package를 설치, 관리하는 시스템

```
windows: python -m pip install -U pip 혹은 https://www.lfd.uci.edu/~gohlke/pythonlibs/#pip/
centos: yum install pip
ubuntu: apt-get install python-pip
```
#### 사용법

```bash
# 사용법
pip <pip arguments> 
python -m pip <pip arguments>
```
##### pip 간단한 옵션들
``` bash
# 패키지 설치
pip install <package>
## windows에서 다운로드가 잘 안될 경우
## pip --trusted-host pypi.org --trusted-host files.pythonhosted.org install <package>

# 설치된 패키지 리스트 보기
pip list

# 설치된 패키지를 requirements format으로 출력하기
pip freeze > requirements.txt
# requirements에 있는 패키지 설치하기
pip install -r requirements.txt

# 설치된 패키지의 구체적인 정보 보기
pip show <package>

# 특정 패키지 찾기
pip search <package>

# 패키지 삭제
pip uninstall <package>

# 패키지 dependency check
pip check

# Online 되는 곳에서 해당 패키지를 다운 로드 받음
pip download <package>
# Offline 인 곳에 다운로드 받은 패키지를 옮겨 아래와 같이 실행하여 설치
pip install --no-index --find-links . <package>
```
> https://pip.pypa.io/en/stable/

---

### 가상 환경 구성
virtualenv 혹은 venv(3.4 이후는 기본 설치) 사용  
구동 되었을 때 앞이 *(venv)* 로 변경됨  
> OS와 응용프로그램간의 python version 및 package를 다르게 가져갈 수 있음  
>
|OS의 python version 및 package  |응용프로그램의 python version 및 package |
|---|---|
|![OS의 환경](image/python_osenv.JPG)  |![App의 환경](image/python_appenv.JPG)  |  

* 가상 환경을 구축하여 서로 다른 응용 프로그램간의 영향을 주지 않게 할 수 있음  
(해당 용용 프로그램에 필요한 파이썬이나 패키지를 설치한 데이터 트리를 구성하는 방법)  

> https://virtualenv.pypa.io/en/latest/  
> https://docs.python.org/3/library/venv.html#


#### 설치 (3.5 이상 설치 불필요)
```bash
pip install virtualenv
```
#### 사용법
```bash
# Linux
## 환경 구성
virtualenv ENV
python -m venv .venv
## 동작
source .venv/bin/activate
## 종료
deactivate
```
```powershell
# Windows
## 환경 구성 
python -m venv .venv
## 동작
. .venv\Scripts\activate
## 종료
deactivate
```


---

## Flask를 이용한 API 제작  

Python 기반 Web Framework 로 간단한 API, Web 서비스 제작에 사용  
최신버전: 1.1.1(11/7)  
  
> Flask 공식 Document: https://flask.palletsprojects.com/en/1.1.x/  
> python web framework Rank: https://hotframeworks.com/languages/python  
> |웹 프레임워크|설명|
> |---|---|
> |Django| 복잡하고 큰 Web Servicing에도 적합, 검증된 framework  |
> |flask| 간단한 API 제작에 적합한 framework  |
> |Aiohttp|대규모 처리에 빠른 속도를 보여 최근 뜨고 있는 framework  |
---  

### Flask 설치  

```bash
pip install flask flask-api
```

---

### 기본 사용법  
  
```python
from flask import Flask     # 1. Flask 모듈 import
app = Flask(__name__)       # 2. app에 Flask 객체 할당

@app.route("/")             # 3. app 객체를 이용하여 routing
def hello():                # 4. 해당 routing 경로, 요청에 맞는 함수 작성
    return "Hello World!"

if __name__ == "__main__":
    app.run(host="127.0.0.1", port="8080")  # 5. Flask 서버 구동을 위한 설정
```
```bash
> python -m flask run
```
  
> 
| Hello world | Server Log |
|---|---|
|![flaks 01](image/python_flask01.JPG)  |![flaks 02](image/python_flask02.JPG)  |

---
  
  
### 라우팅: 다양한 URL을 연결 하는 방법  
  
```python
from flask import Flask

app = Flask(__name__)

@app.route("/")
@app.route("/hello")        # 여러 디렉토리에 같은 동작도 가능하게함
def hello():
    return "<h1>Hello World!</h1>"

@app.route("/first")        # route 안에 디렉토리를 적어 해당 디렉토리 호출에 맞는 함수 작성
def first():
    return "<h3>First!</h3>"

@app.route("/second")
def second():
    return "Hello Second!"

if __name__ == "__main__":
    app.run(host="127.0.0.1", port="8080")
```

> ![flask 03](image/python_flask03.JPG)  
> ![flask 04](image/python_flask04.JPG)  
---

### URL을 변수로 활용하기  
  
```python
from flask import Flask

app = Flask(__name__)

@app.route("/")
def hello():
    return "<h1>Hello World!</h1>"

@app.route("/<username>")       # <>안에 값을 받을 변수를 넣어 URL을 통해 값을 받을 수 있음
def user_profile(username):
    return "user_profile: " + username

@app.route("/second/<string:username>") # <타입:변수명> 을 통해 원하는 타입의 변수에 저장 가능 
def second_profile(username):
    return "<h3>Hello" + username + "!</h3>"

if __name__ == "__main__":
    app.run(host="127.0.0.1", port="8080")
```
> 가능한 타입: stirng, int, float, path, uuid

> ![flask 06](image/python_flask06.JPG)  
> ![flask 07](image/python_flask07.JPG)  
---

### method, code 에 따른 처리 방법  

Flask API를 통해 좀 더 유연하게 API를 제작 가능  
```python
from flask import Flask, request, render_template

app = Flask(__name__)

@app.route("/<string:app>", methods=['PUT','POST']) # request method에 따른 처리
def put(app)
    if request.method == "POST":
        return ~~~
    else:
        return ~~~

@app.route("/<string:app>", methods=['GET'])
def get(app)
    return ~~~~

@app.errorhandler(404)          # 에러 핸들러를 통한 에러처리
def page_not_found(error):
    return render_template('page_not_found.html'), 404

```
  
---
### Web Servicing  
  
실제 production 환경에서는 gunicorn, apache 등의 webservicing application을 이용할 것  
  
#### gunicorn 설치  
  
```bash
pip install gunicorn

gunicorn -b :port번호 --reload 파이썬파일 이름:app 
```
  
## Mysql 연결  


