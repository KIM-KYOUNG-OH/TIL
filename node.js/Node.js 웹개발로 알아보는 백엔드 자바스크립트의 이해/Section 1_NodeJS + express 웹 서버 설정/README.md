# node 프로젝트 생성하기
- 빈 directory 생성  
```shell
mkdir nodeDemo
```

- node 프로젝트 생성  
  - package.json 파일이 생성되고 모든 라이브러리가 여기 저장된다.  
  ```shell
  npm init
  ```

- express install
  - —save는 모든 라이브러리를 package.json에 저장한다는 의미
  - package.json에 dependencies가 생김
  - node_modules에 모듈들이 생김
  ```shell
  npm install express --save
  ```
 
 # node로 웹서버 구동하기
 - node 파일 생성하기
    - require()는 node_modules에 있는 라이브러리를 가져다 쓰겠다는 뜻
    - listen(port, function)

```shell
vi app.js
```

```javascript
var express = require('express')
var app = express()
app.listen(3000, function() {
	console.log("start! express server on port 3000");
});
```

- js 파일 노드로 실행
    - 실행후 프로그램이 동작되는 상태로 대기함
    - 브라우저로 127.0.0.1:3000 접근시 동작함
    - node는 동기적인 함수가 먼저 실행되고 비동기 함수를 실행함

```shell
node app.js
```

# nodemon을 사용해서 파일의 변경을 감지해서 알아서 node 프로젝트를 재실행하는 방법
- -g 는 global로 pc어디서든 nodemon을 사용가능하도록 함
- js 파일을 수정하고 저장하면 알아서 자동으로 node를 재실행함
```shell
npm install nodemon -g

nodemon app.js
```

# URL Routing 처리

- express의 함수를 참고하자

- app.js
    - __dirname 으로 프로젝트 공통 경로 축소
    - html url 요청에 대한 라우팅을 정의해줘야함

```javascript
var express = require('express')
var app = express()
app.listen(3000, function() {
	console.log("start! express server on port 3000");
});

app.get('/', function(req, res) {
	res.sendFile(__dirname + "/public/main.html")
})

app.get('/main', function(req, res) {
	res.sendFile(__dirname + "/public/main.html")
})
```

- public/main.html

```html
<!DOCTYPE html>
<html>
<head>
    <meta charset='utf-8'>
    <meta name='description' content=''>
    <meta name='viewport' content='width=device-width, initial-scale=1'>
    <title>Page Title</title>
</head>
<body>
    <h1>main page</h1>

    <p>Lorem ipsum dolor sit amet consectetur adipisicing elit. Error ipsam temporibus voluptatum nisi corrupti fuga tempora, quaerat illum ipsum ducimus quo facilis recusandae, asperiores et id repellat at ea laboriosam!</p>
</body>
</html>
```

# static 디렉토리 설정

- static 파일이란 javascript, css, image 파일 같은 변하지 않는 파일
- express에게 static 파일의 경로를 등록해줘야함

```javascript
app.use(express.static('public'))
```
