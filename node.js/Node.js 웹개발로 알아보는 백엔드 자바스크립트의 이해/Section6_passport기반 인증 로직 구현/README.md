# passport 환경 구축

- 서버는 사용자의 로그인 여부에 따라 보여줘야할 정보가 다르다.
- 따라서 서버에선 사용자의 로그인 상태값을 계속 확인하고 유지해야할 필요가 있다.
- 상태값을 유지하지 않는 Token방식도 존재함
- passport는 인증 관련된 처리를 하는 모듈

- passport 모듈 설치
    - github/passport-local이나 passportjs.org에서 자세한 내용 참고
    - passport-local: 네이버, 카카오, 구글 로그인 말고 db에서 따로 관리하는 일반 로그인 처리
    - express-session: 세션 관련된 처리
    - connect-flase: 에러 리다이렉트 처리

```bash
npm install passport passport-local express-session connect-flash --save-dev
```

- app.js
    - 자세한 내용은 github 참고

```jsx
// include
var passport = require('passport')
var LocalStrategy = require('passport-local').Strategy
var session = require('express-session')
var flash = require('connect-flash')
```

# Middleware, strategy 설정

- [https://github.com/expressjs/session](https://github.com/expressjs/session)
- app.js

```jsx
// middleware
app.use(session({
	secret: 'keyboard cat',
	resave: false,
	saveUninitialized: true
}))
app.use(passport.initialize())
app.use(passport.session())
app.use(flash()
```

- router/join/index.js
    - 'local-join'이라는 strategy를 passport.use()로 등록

```jsx
passport.use('local-join', new LocalStrategy({
    usernameField: 'email',
    passwordField: 'passwd',
    passReqToCallback: true
}, function(req, email, password) {
    console.log('local-join callback called');
}))

router.post('/', passport.authenticate('local-join', {
    successRedirect: '/main',
    failureRedirect: '/join',
    failureFlash: true
}))
```

# local-strategy 콜백 완성

- router/join/index.js
    - 입력한 email 정보가 이미 존재하면 errMsg를 랜더링함

```jsx
// Routing
router.get('/', function(req, res){
    var msg;
    var errMsg = req.flash('error');
    if(errMsg) msg = errMsg;
	res.render('join.ejs', {'message' : msg});
})

passport.use('local-join', new LocalStrategy({
    usernameField: 'email',
    passwordField: 'password',
    passReqToCallback: true
}, function(req, email, password, done) {
    var query = connection.query('select * from user where email=?', [email], function(err, rows){
        if(err) {
            return done(err);
        }
        if(rows.length) {
            console.log('existed user');
            return done(null, false, {message: 'your email is already used'});
        } else{
            var sql = {email: email, pw: password};
            var query = connection.query('insert into user set ?', sql, function(err, rows){
                if(err) throw err;
                return done(null, {'email': email, 'id': rows.insertId});
            })
        }
    })
}))

router.post('/', passport.authenticate('local-join', {
    successRedirect: '/main',
    failureRedirect: '/join',
    failureFlash: true
}))
```
![스크린샷 2021-11-30 오후 9 30 55](https://user-images.githubusercontent.com/66231761/144047703-e03c1b37-8105-4090-bbf5-a45059a83f6c.png)
![스크린샷 2021-11-30 오후 9 31 11](https://user-images.githubusercontent.com/66231761/144047738-8e253915-8eda-4f4f-95a6-b3ad23992052.png)

# passportjs 기반 세션처리

- router/join/index.js
    - serializeUser는 session에 값을 저장
    - deserializeUser는 session에서 값을 꺼내옴
    - 'local-join'이라는 strategy를 생성
    - 'local-join'의 성공, 실패 여부에 따라 페이지 redirect

```jsx
passport.serializeUser(function(user, done){
    console.log('passport session save: ', user.id);
    done(null, user.id);
})

passport.deserializeUser(function(id, done) {
    console.log('passport session get id: ', id);
    done(null, id);
});

passport.use('local-join', new LocalStrategy({
    usernameField: 'email',
    passwordField: 'password',
    passReqToCallback: true
}, function(req, email, password, done) {
    var query = connection.query('select * from user where email=?', [email], function(err, rows){
        if(err) {
            return done(err);
        }
        if(rows.length) {
            console.log('existed user');
            return done(null, false, {message: 'your email is already used'});
        } else{
            var sql = {email: email, pw: password};
            var query = connection.query('insert into user set ?', sql, function(err, rows){
                if(err) throw err;
                return done(null, {'email': email, 'id': rows.insertId});
            })
        }
    })
}))

router.post('/', passport.authenticate('local-join', {
    successRedirect: '/main',
    failureRedirect: '/join',
    failureFlash: true
}))
```

- views/main.ejs

```jsx
<!DOCTYPE html>
<html>
<head>
<meta charset='utf-8'>
<title>main</title>
</head>
<body>
<h1>main page</h1>
<h3>welcome, <%= id %> 번째 가입고객님!!!</h3>
<p>Lorem, ipsum dolor sit amet consectetur adipisicing elit. Ab vero eos nemo aperiam quibusdam eveniet ipsam libero a sed sunt dolorum possimus ex culpa iste officia maiores, assumenda nobis sequi.</p>

</body>
</html>
```

![스크린샷 2021-11-30 오후 9 31 35](https://user-images.githubusercontent.com/66231761/144047795-89e5cf66-a79f-4795-ac16-4bc907015f27.png)

# Ajax 기반의 passport 인증 처리

- router/login/index.js

```jsx
var express = require('express')
var path = require('path')
var app = express()
var router = express.Router()
var mysql = require('mysql')
const passport = require('passport')
var LocalStrategy = require('passport-local').Strategy
const { doesNotMatch } = require('assert')

// Database Setting
var connection = mysql.createConnection({
    host: 'localhost',
    user: 'root',
    password: '1234',
    database: 'jsman'
});

// Routing
router.get('/', function(req, res){
    var msg;
    var errMsg = req.flash('error');
    if(errMsg) msg = errMsg;
	res.render('login.ejs', {'message' : msg});
})

passport.serializeUser(function(user, done){
    done(null, user);
    console.log('passport session save: ', user.id);
})

passport.deserializeUser(function(id, done) {
    console.log('passport session get id: ', id);
    done(err, id);
});

// register strategy
passport.use('local-login', new LocalStrategy({
    usernameField: 'email',
    passwordField: 'password',
    passReqToCallback: true
}, function(req, email, password, done) {
    var query = connection.query('select * from user where email=?', [email], function(err, rows){
        if(err) {
            return done(err);
        }
        if(rows.length) {
            return done(null, {'email': email, 'id': rows[0].user_id});
        } else{
            return done(null, false, {'message': 'your login info is not found'});
        }
    })
}))

// Routing & call local strategy
router.post('/', function(req, res, next){
    passport.authenticate('local-login', function(err, user, info){
        if (err) return res.status(500).json(err);
        if (!user) { 
            return res.status(401).json(info.message)
        }

        req.logIn(user, function(err) {
            if (err) { return next(err); }
            return res.json(user);
        });
    })(req, res, next);
})

module.exports = router;
```

- views/login.ejs

```jsx
<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8">
    <title>email form</title>
</head>
<body>
    <form action="/login" method="post">
        email : <input type="text" name="email"><br/>
        password : <input type="text" name="password"><br/>
    </form>

    <button class="ajaxsend">login</button>

    <div class="result"></div>

    <script>
        document.querySelector('.ajaxsend').addEventListener('click', function(){
            var email = document.getElementsByName('email')[0].value
            var password = document.getElementsByName('password')[0].value
            sendAjax('http://127.0.0.1:3000/login', {'email': email, 'password': password});
        })

        function sendAjax(url, data){
            data = JSON.stringify(data);

            var xhr = new XMLHttpRequest();
            xhr.open('POST', url);
            xhr.setRequestHeader('Content-Type', "application/json");
            xhr.send(data);

            xhr.addEventListener('load', function() {
                var result = JSON.parse(xhr.responseText);
                var resultDiv = document.querySelector(".result");
                if(result.email) resultDiv.innerHTML = 'welcome, ' + result.email + '!!';
                else resultDiv.innerHTML = result;
            })
        }
    </script>
</body>
```

![스크린샷 2021-11-30 오후 9 32 01](https://user-images.githubusercontent.com/66231761/144047862-cedff4e6-57bb-4daa-a800-4587cd6603d5.png)

- DB user table에 있는 정보를 요청했을 때
    - session에 user 정보 저장(req.user에 저장됌)
![스크린샷 2021-11-30 오후 9 32 33](https://user-images.githubusercontent.com/66231761/144047917-8a7eac99-9475-40c5-bbe7-95df24d71f65.png)

- DB user table에 없는 정보를 요청했을 때
    - 401에러(Unauthorized) 발생
![스크린샷 2021-11-30 오후 9 32 58](https://user-images.githubusercontent.com/66231761/144047971-fabac81b-2ea8-48be-af29-8f1348a8c716.png)

# Logout 처리

- views/main.ejs

```jsx
<!DOCTYPE html>
<html>
<head>
<meta charset='utf-8'>
<title>main</title>
</head>
<body>
<h1>main page</h1>
<h3>welcome, <%= id %> 번째 가입고객님!!!</h3>
<p>Lorem, ipsum dolor sit amet consectetur adipisicing elit. Ab vero eos nemo aperiam quibusdam eveniet ipsam libero a sed sunt dolorum possimus ex culpa iste officia maiores, assumenda nobis sequi.</p>

<div><a href="/logout">logout</a></div>
</body>
</html>
```

- router/main/main.js

```jsx
var express = require('express')
var path = require('path')
var app = express()
var router = express.Router();

//main page는 login 될 때만(세션 정보가 있을 때만) 접근이 가능하게 하자
//url routing
router.get('/', function(req, res) {
	console.log('main is loaded');
	var user = req.user;
	if(!user) return res.render('login.ejs');
	res.render('main.ejs', {'id' : user});
})

module.exports = router;
```

- router/logout/index.js
    - req.logout() 호출시 session정보를 삭제함

```jsx
var express = require('express')
var app = express()
var router = express.Router()

router.get('/', function(req, res){
    console.log('logout router');
    req.logout();
    res.redirect('/login');
});

module.exports = router;
```

![스크린샷 2021-11-30 오후 9 35 07](https://user-images.githubusercontent.com/66231761/144048305-cfc446e1-0bd9-4121-8bc3-21b09d3660ee.png)
![스크린샷 2021-11-30 오후 9 33 33](https://user-images.githubusercontent.com/66231761/144048040-7f124bba-e7c3-4f39-aaa7-2679f3963e9f.png)

- 로그아웃 클릭시 session 정보가 지워지고 login 창으로 Redirect
    - 로그아웃 상태에서 다시 main창을 호출시 session을 삭제했으므로 다시 login창으로 돌아옴
![스크린샷 2021-11-30 오후 9 33 56](https://user-images.githubusercontent.com/66231761/144048100-d80f3f17-29fb-4706-ac61-2e43efb79bf5.png)
