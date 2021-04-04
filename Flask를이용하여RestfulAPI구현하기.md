# Flask를 이용하여 Restful API 구현하기
## 1. Flask란? 
- Flask는 Python 기반의 Micro Web Framework임
- 배우기 쉽고, 간단한 코드 구현과 자유도가 높다는 장점이 있음

## 왜 Flask로 API Server를 구현하나?
- Flask는 현실적으로 서버의 Application단을 구현한다기 보다 API Server의 역할을 더 많이 함
- 요약해보면 장점은 다음과 같은 3가지가 있음
  - API Server를 가볍게 구현할 수 있음 
  - Docker나 Kubernetes를 이용해 여러 개의 컨테이너를 이용하여 스케쥴링이 가능
  - 쉽고, 코드가 짧음 

## 설치 내용
- `flask`와 `flask-restx` 를 `pip`를 이용해서 설치
~~~cmd
pip install flask
pip install flask-restx
~~~

## Flask server 만들어보기
- `app.py` 라는 명으로 해당 script를 구현
~~~python
from flask import Flask                 #- 서버 구현을 위한 Flask 객체 import
from flask_restx import Api, Resource   #- Api 구현을 위한 Api 객체 import

app = Flask(__name__)                   #- Flask 객체 선언. parameter로 app 패키지 이름을 넣어줌
api = Api(app)                          #- Api 객체에 Flask 객체 등록

@api.route('/hello')                    #- 데코레이터 이용. '/hello' 경로에 클래스 등록
class HelloWorld(Resource):
    def get(self):                      #- GET 요청시 리턴 값에 해당하는 dict를 JSON 형태로 반환
        return {"hello":"world"}

if __name__ == "__main__":
    app.run(debug = True, host = '0.0.0.0', port = 80)
~~~
- 그 다음 터미널에서 실행시켜 보자
~~~linux
python app.py
~~~
- 그 다음 여러가지 API TEST Tool을 이용할 수 있는데, 구글 크롬 확장 앱인, [Advanced REST client](https://chrome.google.com/webstore/detail/advanced-rest-client/hgmloofddffdnphfgcellkdfbfbjeloo?hl=ko)를 이용하여 테스트 가능

## 다양한 Resourceful Routing
- route의 url에 query string이 아닌 url partern을 이용할 수 있음
- url 자체에 변수를 삽입하는 방법으로 가능하며, `<타입명:변수명>` 형태로 작성하면 됨
- 그 변수는 class의 맴버 함수의 파라미터로 삽입하여 사용함
~~~python
rom flask import Flask
from flask_restx import Resource, Api

app = Flask(__name__)
api = Api(app)


@api.route('/hello/<string:name>')  # url pattern으로 name 설정
class Hello(Resource):
    def get(self, name):  # 멤버 함수의 파라미터로 name 설정
        return {"message" : "Welcome, %s!" % name}
    
if __name__ == "__main__":
    app.run(debug=True, host='0.0.0.0', port=80)
~~~

## Status Code와 Header 설정
- 반환하고자 하는 리턴 값으로 iterable하게 값을 넣으면 됨
- 순서는 다음과 같음
  - 반환 하고자하는 dict 객체
  - 반환 하고자하는 Status Code
  - 반환 하고자하는 Header 

## GET, POST, PUT, DELETE
- `get`, `post`, `put`, `delete` 맴버 함수를 오버라이딩하여 구현해 주면 됨
- body에 있는 데이터를 가져오기 위하여 취하는 방법은 간단함
- `flask` 모듈 내의 request 내의 json 객체를 이용하여 request body로 들어온 json 값을 파싱하면 됨
- json 객체는 dict 객체
~~~python 
from flask import Flask, request
from flask_restx import Resource, Api

app = Flask(__name__)
api = Api(app)

todos = {}
count = 1

@api.route('/todos')
class TodoPost(Resource):
    def post(self):
        global count
        global todos
        
        idx = count
        count += 1
        todos[idx] = request.json.get('data')
        
        return {
            'todo_id': idx,
            'data': todos[idx]
        }


@api.route('/todos/<int:todo_id>')
class TodoSimple(Resource):
    def get(self, todo_id):
        return {
            'todo_id': todo_id,
            'data': todos[todo_id]
        }

    def put(self, todo_id):
        todos[todo_id] = request.json.get('data')
        return {
            'todo_id': todo_id,
            'data': todos[todo_id]
        }
    
    def delete(self, todo_id):
        del todos[todo_id]
        return {
            "delete" : "success"
        }

if __name__ == "__main__":
    app.run(debug=True, host='0.0.0.0', port=80)
~~~


## 참고 블로그 & 문서
- https://justkode.kr/python/flask-restapi-1