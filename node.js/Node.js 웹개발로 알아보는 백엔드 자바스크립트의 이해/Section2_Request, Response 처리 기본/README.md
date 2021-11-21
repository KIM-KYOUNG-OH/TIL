# Request, Response 처리 기본

# POST 요청 처리
- post 방식은 get처럼 url에 정보를 담는게 아니다.
- post 방식은 get방식 처럼 req.param()으로 파라미터를 받지 못하고 body-parser라는 모듈을 이용한다.
    
```shell
npm install body-parser --save
```
    

```javascript
app.post('/email_post', function(req, res){
	// get : req.param('email')
	console.log(req.body.email)
	res.send("<h1>welcome " + req.body.email + "!</h1>" )
})
```

- form.html

```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8">
    <title>email form</title>
</head>
<body>
    <form action="/email_post" method="post">
        email : <input type="text" name="email"><br/>
        <input type="submit">
    </form>
</body>
```
<img width="600" alt="스크린샷 2021-11-22 오전 1 04 57" src="https://user-images.githubusercontent.com/66231761/142769534-d6bb23eb-b6a7-41f3-8cd6-c4a2c6cc0993.png">

<img width="600" alt="스크린샷 2021-11-22 오전 1 05 17" src="https://user-images.githubusercontent.com/66231761/142769561-4a78af92-a948-4356-aaf0-a82431c5aa91.png">

# ejs 모듈을 이용하여 서버에서 데이터를 섞어서 html 코드를 클라이언트에 전송하기
- ejs 설치
```shell
npm install ejs --save
```

- email.ejs
```ejs
<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8">
    <title>email form</title>
</head>
<body>
    <form action="/email_post" method="post">
        email : <input type="text" name="email"><br/>
        <input type="submit">
    </form>
</body>
```

- app.js
```javascript
app.post('/email_post', function(req, res){
	// get : req.param('email')
	console.log(req.body.email)
	res.render('email.ejs', {email: req.body.email})
})
```

<img width="600" alt="스크린샷 2021-11-22 오전 1 05 40" src="https://user-images.githubusercontent.com/66231761/142769571-2ccd7f5d-b04d-4cd6-bd3d-7f43021df5d3.png">

# Json을 활용한 Ajax 처리
- Ajax는 브라우저 새로고침 없이 서버에 JSON데이터를 보내고 JSON응답을 받을 수 있다
- form.html
```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8">
    <title>email form</title>
</head>
<body>
    <form action="/email_post" method="post">
        email : <input type="text" name="email"><br/>
        <input type="submit">
    </form>

    <button class="ajaxsend">ajaxsend</button>

    <div class="result"></div>

    <script>
        document.querySelector('.ajaxsend').addEventListener('click', function(){
            var inputdata = document.forms[0].elements[0].value
            sendAjax('http://127.0.0.1:3000/ajax_send_email', inputdata);
        })

        function sendAjax(url, data){
            var data = {'email' : data};
            data = JSON.stringify(data);

            var xhr = new XMLHttpRequest();
            xhr.open('POST', url);
            xhr.setRequestHeader('Content-Type', "application/json");
            xhr.send(data);

            xhr.addEventListener('load', function() {
                var result = JSON.parse(xhr.responseText);
                if(result.result != "ok") return;
                document.querySelector(".result").innerHTML = result.email;
            })
        }
    </script>
</body>
```

- app.js
```javascript
const { response } = require('express');
var express = require('express')
var app = express()
var bodyParser = require('body-parser')

app.listen(3000, function() {
	console.log("start! express server on port 3000");
});

app.use(express.static('public'))
app.use(bodyParser.json())
app.use(bodyParser.urlencoded({extended: true}))
app.set('view engine', 'ejs')

//url routing
app.get('/', function(req, res) {
	res.sendFile(__dirname + "/public/main.html")
})

app.get('/main', function(req, res) {
	res.sendFile(__dirname + "/public/main.html")
})

app.post('/email_post', function(req, res){
	// get : req.param('email')
	console.log(req.body.email)
	res.render('email.ejs', {email: req.body.email})
})

app.post('/ajax_send_email', function(req, res) {
	console.log(req.body.email);
	// check validation about input value => select db
	var responseData = {'result': 'ok', 'email': req.body.email}
	res.json(responseData);
})
```
<img width="600" alt="스크린샷 2021-11-22 오전 1 06 07" src="https://user-images.githubusercontent.com/66231761/142769587-10ca32fa-cd33-48d5-b8f0-eff4beeed145.png">
