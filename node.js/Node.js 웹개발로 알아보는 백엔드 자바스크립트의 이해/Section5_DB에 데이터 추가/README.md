# DB에 데이터 Insert하기

- index.js

```javascript
// Routing
router.post('/', function(req, res){
    var body = req.body;
    var email = body.email;
    var name = body.name;
    var password = body.password;

    var query = connection.query('insert into user(email, name, pw) values("' + email + '", "' + name + '", "' + password + '")', function(err, rows){
        if (err) throw err;
        console.log('ok db insert');
    })
})
```

<img width="600" alt="스크린샷 2021-11-22 오후 9 57 41" src="https://user-images.githubusercontent.com/66231761/142865803-59e91c8c-57d2-4a0c-9e0b-00d049457521.png">
<img width="885" alt="스크린샷 2021-11-22 오후 9 58 08" src="https://user-images.githubusercontent.com/66231761/142865865-4762dc3d-a94c-4992-9962-31e4ec377eb4.png">

# mysqljs로 query 편하게 선언하기

- legacy

```jsx
var query = connection.query('insert into user(email, name, pw) values("' + email + '", "' + name + '", "' + password + '")', function(err, rows){

})
```

- to-be

```jsx
var sql = {email: email, name: name, pw: password};
var query = connection.query('insert into user set ?', sql, function(err, rows){

})
```

# insert 성공후 클라이언트에 데이터 추가 성공 화면 랜더링하기

- index.js
    - rows엔 아래의 정보가 들어있다.
        
        ```shell
        OkPacket {
          fieldCount: 0,
          affectedRows: 1,
          insertId: 14,
          serverStatus: 2,
          warningCount: 0,
          message: '',
          protocol41: true,
          changedRows: 0
        }
        ```
        
    - render()는 template engine(ejs)을 사용하여, 지정한 .ejs파일에 parameter로 보낸 값을 합쳐서 html 코드를 만들어서 클라이언트에게 보냄

```javascript
// Routing
router.post('/', function(req, res){
    var body = req.body;
    var email = body.email;
    var name = body.name;
    var password = body.password;

    var sql = {email: email, name: name, pw: password};
    var query = connection.query('insert into user set ?', sql, function(err, rows){
        console.log(rows);
        if (err) throw err;
        else res.render('welcome.ejs', {'name': name, 'id': rows.insertId});
    })
})
```

- views/welcome.ejs

```html
<!DOCTYPE html>
<html>
<head>
<meta charset="utf-8">
<title>Welcome</title>
</head>
<body>
    <h1>Welcome !! <%= name %> </h1>

    <p><%= id %>번째 가입자시네요~~</p>
</body>
</html>
```
<img width="887" alt="스크린샷 2021-11-22 오후 9 58 48" src="https://user-images.githubusercontent.com/66231761/142865974-6256dd29-76f3-4c1d-a482-8219520057a1.png">

