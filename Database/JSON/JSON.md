# JSON(JavaScript Object Notation)  
- 자바스크립트에서 배열과 객체를 표현하는 데이터 교환언어  
- 어떤언어에서도 읽고 쓰는데 문제가 없다  
- 프로그래밍 언어와 플랫폼에 독립적  
- XML과 달리 종료태그가 없어서 간결하다('eval()'을 사용할수 있음)  

### 1. 객체  
{"key":"value"} 형태의 쌍의 집합(중괄호)  
### 2. 배열  
값의 순서화된 리스트(대괄호)  
### 3. JSON파서로 JSON텍스트를 Javascript객체로 변환하기  
```html
<script type="text/javascript">
	var s = '['+
		'{"name":"Hong", "age":"21"},' +
		'{"name":"Kim","age":"22"},'+
		'{"name":"Park","age":"23"}]'; 
	
	var students = JSON.parse(s); // JSON전용파서를 사용하는 것이 'eval()'을 사용하는것보다 보안상 안전함
	
	document.getElementById("name").innerHTML = students[1].name; // Kim
	document.getElementById("age").innerHTML = students[1].age; // 22
</script>
```
