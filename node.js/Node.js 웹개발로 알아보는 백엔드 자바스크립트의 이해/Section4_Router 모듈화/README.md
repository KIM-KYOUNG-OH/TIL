# Router 모듈화
- routing 정보를 다른 파일로 분리하여 보다 효율적으로 관리
- app.js에 코드가 집약된 코드를 라우팅 모듈화(require, export)로 분리하여 app.js를 가볍게 리팩토링할 수 있다.
- router/main.js
    - 'path' 모듈을 사용하면 path.join()함수를 이용하여 상대경로를 구할 수 있다.

```javascript
var express = require('express')
var path = require('path')
var app = express()
var router = express.Router();

//url routing
router.get('/', function(req, res) {
    console.log('main.js');
	res.sendFile(path.join(__dirname, "../public/main.html"))
})

module.exports = router;
```

- router/email.js

```javascript
var express = require('express')
var path = require('path')
var app = express()
var router = express.Router()
var mysql = require('mysql')

// Database Setting
var connection = mysql.createConnection({
    host: 'localhost',
    user: 'root',
    password: '1234',
    database: 'jsman'
});

// Routing
router.post('/form', function(req, res){
	// get : req.param('email')
	console.log(req.body.email)
	res.render('email.ejs', {email: req.body.email})
})

router.post('/ajax', function(req, res) {
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

module.exports = router;
```

- app.js

```javascript
const { response } = require('express')
var express = require('express')
var app = express()
var bodyParser = require('body-parser')
var main = require('./router/main')
var email = require('./router/email')

app.listen(3000, function() {
	console.log("start! express server on port 3000");
});

app.use(express.static('public'))
app.use(bodyParser.json())
app.use(bodyParser.urlencoded({extended: true}))
app.set('view engine', 'ejs')

app.use('/main', main)
app.use('/email', email)

app.get('/', function(req, res) {
	res.sendFile(__dirname + "/public/main.html")
})
```

# Routing 리팩토링
- Controller에 해당하는 라우팅 모듈들을 package를 나눠서 관리하고 index.js 미들웨어 하나를 둔다.
<img width="400" alt="스크린샷 2021-11-22 오후 9 54 45" src="https://user-images.githubusercontent.com/66231761/142865390-449c75f4-7900-4451-bba9-fde26d3e26c3.png"
     
- index.js

```javascipt
var express = require('express')
var path = require('path')
var app = express()
var router = express.Router()
var main = require('./main/main')
var email = require('./email/email')

router.get('/', function(req, res) {
	res.sendFile(path.join(__dirname, "../public/main.html"))
})

router.use('/main', main)
router.use('/email', email)

module.exports = router;
```

- email.js

```javascript
var express = require('express')
var path = require('path')
var app = express()
var router = express.Router()
var mysql = require('mysql')

// Database Setting
var connection = mysql.createConnection({
    host: 'localhost',
    user: 'root',
    password: '1234',
    database: 'jsman'
});

// Routing
router.post('/form', function(req, res){
	// get : req.param('email')
	console.log(req.body.email)
	res.render('email.ejs', {email: req.body.email})
})

// app.post('/ajax_send_email', function(req, res) {
// 	console.log(req.body.email);
// 	// check validation about input value => select db
// 	var responseData = {'result': 'ok', 'email': req.body.email}
// 	res.json(responseData);
// })

router.post('/ajax', function(req, res) {
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

module.exports = router;
```

- main.js

```javascript
var express = require('express')
var path = require('path')
var app = express()
var router = express.Router();

//url routing
router.get('/', function(req, res) {
	res.sendFile(path.join(__dirname, "../../public/main.html"))
})

module.exports = router;
```
