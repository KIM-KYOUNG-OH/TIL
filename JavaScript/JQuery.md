# jQuery  
- 일종의 자바스크립트 라이브러리이다  
- 프로그래밍 양을 대폭 줄일 수 있다  
- 문서가 완전히 로드된 후 실행됨  

## 1. jQuery 구조  
```javascript
$(selector).action();
```

## 1-1. 선택자  
1) 타입선택자  
```
$("p").hide();
```
2) class 선택자  
```
$(".menu").slideup();
```
3) id 선택자  
```
$("#check").show();
```
4) 그외 선택자  
```
$("*") // 모든 요소 선택
$(this) // 현재 요소 선택
$("p.myclass") // p태그중 class가 'myclass'인 요소  
$("p:first") // p태그중 첫번째  
$("div span") // div태그안에 포함된 span요소
$(":button") // 버튼과 버튼타입 요소를 모두 선택
```

## 2. 이벤트  
1) 키보드  
- keypress  
- keydown  
- keyup  
2) 마우스  
- click  
- dbclick  
- mouseenter  
- mouseleave  
3) form창
- submit  
- change  
- focus  
- blur  
4) 웹브라우저  
- resize  
- load  
- unload  
- scroll  

### 2-1. 매개변수 선언 
function(e)와 같이 이벤트 객체를 선언해서 이벤트 처리기 함수 안에서 사용가능한 정보를 출력가능
- e.pageX, e.pageY // 마우스 위치  
- e.type  // 이벤트 위치  
- e.which // 눌려진 마우스 버튼이나 키  

## 1. 미래에 선언될 태그에 event를 주고싶을때 'on'을 사용  
## 2. 'mouseover' 이벤트로 커서위치에 Tooptip 띄우기  
e.pageX  
e.pageY  

