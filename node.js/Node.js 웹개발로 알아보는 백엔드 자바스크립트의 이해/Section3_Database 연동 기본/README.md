# MySQL 연동

- mysql 로컬 서버 접속
  - -u: user
  - -p: password

```shell
$ mysql -u root -p
```

- database 보기

```shell
$ show databases;
```

- database 만들기

```shell
$ create database jsman;
```

- table 생성

```shell
$ create table user(
	  email varchar(30),
	  name varchar(30),
	  pw varchar(30)
	)

```

- 데이터 삽입

```shell
$ insert into user (email, name, pw) values
('crong@naver.com', 'crong', 'asdf');
```

- 데이터 조회

```shell
$ select * from user;
```

- express에서 mysql 모듈 의존성 관리하기

```shell
> npm install mysql --save
```

- app.js

```javascript
var mysql = require('mysql')

var connection = mysql.createConnection({
    host: 'localhost',
    user: 'root',
    password: '1234',
    database: 'jsman'
});
```

# Query를 이용하여 클라이언트에서 JSON 응답 받기

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
                var resultDiv = document.querySelector(".result");
                if(result.result != "ok") resultDiv.innerHTML = 'your email is not found'
                else resultDiv.innerHTML = result.name;
            })
        }
    </script>
</body>
```

- app.js

```javascript
app.post('/ajax_send_email', function(req, res) {
    var email = req.body.email;
    var responseData = {};

    var query = connection.query('select name from user where email = "' + email + '"', function(err, rows){
        if (err) {
			throw err;
		}
        if (rows[0]) {
            console.log(rows[0].name);
            responseData.result = 'ok';
            responseData.name = rows[0].name;
        }else {
            console.log('none : ' + rows[0]);
			responseData.result = 'none';
            responseData.name = '';
        }
		res.json(responseData);
    })
})
```

- DB에 없는 email 검색시
<img width="600" alt="스크린샷 2021-11-22 오후 9 51 36" src="https://user-images.githubusercontent.com/66231761/142864953-1a778134-3dab-47ce-a202-c3769c2d0de6.png">

- DB에 있는 email 검색시
<img width="600" alt="스크린샷 2021-11-22 오후 9 52 13" src="https://user-images.githubusercontent.com/66231761/142865039-af218a14-6c1a-4539-b07c-278e45ba8c4d.png">


