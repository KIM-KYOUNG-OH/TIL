# REST API

- REpresentational State Transfer
- REST한 방식의 API라는 것은 아래의 규칙을 따르는 것을 말한다.
    - 리소스를 URI로 표현하며 단순하고 직관적인 구조이다.
    - 리소스 상태는 HTTP Method를 활용해서 구분한다.
    - xml/json을 활용해서 데이터를 전송한다.(주로 json)

# CRUD

- 각 행위를 처리하기 위한 HTTP Method
    - Create(POST)
    - Retrieve(GET)
    - Update(PUT)
    - Delete(DELETE)

# API Design

- 복수명사를 사용(/movies)
- 필요하면 URL에 하위 자원을 표현(/movies/23)
- 필터조건을 허용할 수 있다.(/movies?state=active)
- Example
    - 모든 영화리스트 가져오기 - GET - /movies/
    - 영화 추가 - POST - /movies
    - title 해당 영화 가져오기 - GET - /movies/:title
    - title 해당 영화 삭제 - DELETE - /movies/:title
    - title 해당 영화 업데이트 - PUT - /movies/:title
    - 상영중인 영화 리스트 가져오기 - GET - /movies?min=9
    

# RESTful API

- router/movie/index.js

```jsx
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

connection.connect()

// Routing
router.get('/list', function(req, res){
    res.render('movie.ejs');
})

// /movie , GET
router.get('/', function(req, res) {
    var responseData = {};

    var query = connection.query('select title from movie', function(err, rows){
        if (err) throw err
        if (rows.length) {
            console.log(rows);
            responseData.result = 1;
            responseData.data = rows;
        }else {
			responseData.result = 0;
        }
		res.json(responseData);
    })
})

// /movie, POST
router.post('/', function(req, res) {
    var title = req.body.title;
    var type = req.body.type;
    var grade = req.body.grade;
    var actor = req.body.actor;

    var sql = {title, type, grade, actor};  // ES6에서 적용되는 문법(key값 생략 가능)
    var query = connection.query('insert into movie set ?', sql, function(err, rows){
        if(err) throw err;
        return res.json({'result': 1});
    })
})

// /movie/:title, GET
router.get('/:title', function(req, res) {
    var title = req.params.title;
    console.log('title : ', title);

    var responseData = {};

    var query = connection.query('select * from movie where title = ?', [title], function(err, rows){
        if (err) throw err
        if (rows[0]) {
            responseData.result = 1;
            responseData.data = rows;
        }else {
			responseData.result = 0;
        }
		res.json(responseData);
    })
})

// /movie/:title, DELETE
router.delete('/:title', function(req, res) {
    var title = req.params.title;
    
    var responseData = {};
    
    var query = connection.query('delete from movie where title = ?', [title], function(err, rows){
        console.log(rows)
        if (err) throw err

        if (rows.affectedRows > 0) {
            responseData.result = 1;
            responseData.data = title;
        }else {
			responseData.result = 0;
        }
		res.json(responseData);
    })
})

module.exports = router;
```

- views/movie.ejs

```jsx
<!DOCTYPE html>
<html>
<head>
<meta charset='utf-8'>
<title>movie list</title>
</head>
<body>
<h1>movie list</h1>

<section class="showWrap">
    <ul>
        <li class='get_all'>
            <button>모든 영화 보기</button>
            <section class="showResult"></section>
        </li>

        <li class="post">
            <form action="" method="post">
                <div> 제목 : <input type="text" name="title" placeholder="Terminator"></div>
                <div> 장르 : <input type="text" name="type" placeholder="drama"></div>
                <div> 평점 : <input type="text" name="grade" placeholder="8.43"></div>
                <div> 주연 : <input type="text" name="actor" placeholder="Tom cruise"></div>
            </form>
            <button>영화추가</button>
            <section class="showResult"></section>
        </li>

        <li class="get_id">
            <input type="text" name="title">
            <button>영화상세정보</button>
            <section class="showResult"></section>
        </li>

        <li class="delete_id">
            <input type="text" name="title">
            <button>영화삭제</button>
            <section class="showResult"></section>
        </li>

        <li class="update_id">
            <form action="" method="post">
                <div> 제목 : <input type="text" name="title" placeholder="Terminator"></div>
                <div> 장르 : <input type="text" name="type" placeholder="drama"></div>
                <div> 평점 : <input type="text" name="grade" placeholder="8.43"></div>
                <div> 주연 : <input type="text" name="actor" placeholder="Tom cruise"></div>
            </form>
            <button>영화정보갱신</button>
            <section class="showResult"></section>
        </li>

        <li class="get_min">
            <button>현재 상영중인 영화</button>
            <section class="showResult">aaa</section>
        </li>
    </ul>
</section>
<script>
    document.querySelector('.showWrap').addEventListener('click', function(e){
        let url, method, data;
        const target = e.target;
        const li = target.closest('LI');
        const showResult = li.querySelector(".showResult");
        const type = li.className;
        if(target.tagName !== "BUTTON") return;

        switch(type) {
            case "get_all":
                url = '/movie';
                method = 'GET';
                fn = function(result) {
                    if(result.result === 1) {
                        let titles = result.data.reduce(function(pre, next){
                            pre += '<li>' + next.title + '</li>';
                            return pre;
                        }, '');
                        showResult.innerHTML = '<ul>' + titles + '</ul>'
                    }else {
                        showResult.innerHTML = 'list not found';
                    }
                }
                break;
            case "post":
                url = '/movie';
                method = 'POST';
                let inputs = [].slice.call(document.querySelector('form').elements);
                data = inputs.reduce(function(pre, next) {
                    pre[next.name] = next.value;
                    return pre;
                }, {});
                fn = function(result) {
                    if(result.result === 1) showResult.innerHTML = '새로운 영화 데이터가 잘 추가됐습니다.'
                    else showResult.innerHTML = 'list not found';
                }
                break;
            case 'get_id':
                url = '/movie/' + li.getElementsByTagName('input')[0].value;
                method = 'GET';
                fn = function(result) {
                    if(result.result === 1) showResult.innerHTML = result.data[0].actor;
                    else showResult.innerHTML = '해당하는 영화가 없습니다.';
                }
                break;
            case 'delete_id':
                url = '/movie/' + li.getElementsByTagName('input')[0].value;
                method = 'DELETE';
                fn = function(result) {
                    if(result.result === 1){
                        showResult.innerHTML = '선택한 영화 ' + result.data + '가 삭제됐습니다.';
                    } else {
                        showResult.innerHTML = '해당 영화가 없습니다.';
                    }
                }
                break;
            default:
                console.log('default');
                break;
        }
        sendAjax('http://127.0.0.1:3000' + url, method, data, fn);
    })

    function sendAjax(url, method, data, fn) {

        const xhr = new XMLHttpRequest();
        xhr.open(method, url);

        if(data) {
            data = JSON.stringify(data);
            xhr.setRequestHeader('Content-Type', 'application/json');
        }else {
            data = null;
        }

        xhr.send(data);

        xhr.addEventListener('load', function(){
            const result = JSON.parse(xhr.responseText);
            fn(result);
        });
    }
</script>
</body>
</html>
```

![스크린샷 2021-11-30 오후 9 38 46](https://user-images.githubusercontent.com/66231761/144048821-46e0300b-c6b9-40d3-987a-f207d0b8a5c7.png)
