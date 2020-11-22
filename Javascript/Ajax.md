# Ajax(Asynchronous JavaScript And XML)  
- XMLHttpRequest객체를 사용하여 비동기적 페이지 업데이트 작업을 수행하는 것  
- 전체 페이지를 로드하지 않고 데이터를 서버로부터 받을 수 있는 기술  
- 새로운 프로그래밍 언어가 아닌 기존의 기술을 현명히 사용한 것
- ex) 구글맵, 메일, 게시판, 검색어 추천기능
## 1. XMLHttpRequest 객체
- 웹서버와 데이터 교환에 사용
- 웹페이지 전체를 다시 로드하지 않고 웹페이지 일부만 업데이트할 수 있음
### 1-1. open(), send()  
- 서버에 요청을 보냄  
```javascript  
var xhttp = new XMLHttpRequest();
xhttp.open("GET", "ajax_info.txt", true); //("GET" or "POST", file location, async 비동기 or 동기)
xhttp.send();
```  
### 1-2. onreadystatechange 속성  
- onreadystatechange 함수는 readyState가 변경될 때마다 호출된다.  
- readyState는 XMLHttpRequest의 상태를 가진다  
### 1-3. redayState  
0: request not initialized  
1: server connection established  
2: request received  
3: processing request  
4: request finished and response is ready  
### 1-4. status  
200: "OK"  
403: "Forbidden"  
404: "Page not found"  
### 1-5. responseText  
서버 응답을 js문자열로 반환  
### 1-6. responseXML  
서버 응답을 XML DOM객체로 반환  
## 2. JSON to HTML table 
### 2-1. person_to_json.jsp  
```jsp
<%@page import="java.sql.SQLException"%>
<%@page import="java.sql.Connection"%>
<%@page import="oracle.db.OracleConnect"%>
<%@page import="java.sql.ResultSet"%>
<%@page import="java.sql.PreparedStatement"%>

<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<% 
	Connection conn = null;
	PreparedStatement pstmt = null;
	ResultSet rs= null;
	String sql = "select * from person2 order by name asc";
	OracleConnect db = OracleConnect.getInstance();
	
	// 오라클 서버 연결
	conn = db.getConnection();
	StringBuffer sb = new StringBuffer();
	sb.append("{\"person\":[");
	try{
		pstmt = conn.prepareStatement(sql);
		rs=pstmt.executeQuery();
		while(rs.next()){
			String name = rs.getString("name");
			String blood = rs.getString("blood");
			int java = rs.getInt("java");
			int spring = rs.getInt("spring");
			double height = rs.getDouble("height");
			String birth = rs.getDate("birthday").toString();
			sb.append("{\"name\":\""+name+"\",");
			sb.append("\"blood\":\""+blood+"\",");
			sb.append("\"java\":"+java+",");
			sb.append("\"spring\":"+spring+",");
			sb.append("\"height\":"+height+",");
			sb.append("\"birth\":\""+birth+"\"");
			sb.append("},");
		}
		//마지막 컴마제거
		//sb.replace(sb.length()-1, sb.length(), "");
		sb.deleteCharAt(sb.lastIndexOf(","));
				
	}catch(SQLException e){
		System.out.println("person오류:"+e.getMessage());
	}
	sb.append("]}");
%>
<%=sb.toString() %>
```
### 2-2. personjson.html
```html
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
<script src="https://code.jquery.com/jquery-3.5.0.js"></script>
<style type="text/css">
table{
		border: 2px solid gray;
		text-align: center;
	}
	
table tr, table th, table td{
		border-bottom: 1px solid gray;
		border-right: 1px solid gray;
	}
</style>
<script type="text/javascript">
window.onload=function(){
	if(window.XMLHttpRequest){
		var xhr=new XMLHttpRequest();
		//alert('크롬,ie')
	}else{
		alert('Ajax를 지원하지 않는 브라우저입니다');
	}
	// 버튼 이벤트
	document.getElementById('btn').onclick=function(){
		xhr.open('get','person_to_json.jsp',true);
		xhr.onreadystatechange = function(){
			if(xhr.readyState == 4){
				if(xhr.status==200){
					//alert('성공');
					process();
				}
			}
		}
		xhr.send(null);
	}

	function process(){
		
		var textData = xhr.responseText;
		var data = JSON.parse(textData);
		
		var len = data.person.length;
		
		//인원수만큼 반복해서 데이터 출력하기
		var s = "";
		for(var i=0; i<len; i++){
			s+= "<tr>";
			s+= "<td>" +(i+1)+ "</td>";
			s+= "<td>"+data.person[i].name+"</td>";
			s+= "<td>"+data.person[i].blood.toUpperCase()+"형</td>";
			s+= "<td>"+data.person[i].java+"점</td>";
			s+= "<td>"+data.person[i].spring+"점</td>";
			s+= "<td>"+data.person[i].height+"cm</td>";
			s+= "<td>"+data.person[i].birth+"</td>";
			s+= "</tr>"
		}
		document.getElementById('tb').innerHTML = s;
	}
}
</script>
</head>
<body>
<button type='button' id=btn>목록 가져오기</button>
<hr>
<div>
	<table>
		<thead>
			<tr>
				<th width='100'>num</th>
				<th width='100'>name</th>
				<th width='100'>blood</th>
				<th width='100'>java</th>
				<th width='100'>spring</th>
				<th width='100'>height</th>
				<th width='100'>birth</th>
			</tr>
		</thead>
		<tbody id='tb'>
		
		</tbody>
	</table>
</div>
</body>
</html>
```
## 3. XML to HTML table 예제  
### 3-1. sawon.xml  
```jsp  
<?xml version="1.0" encoding="UTF-8"?>
<%@page import="java.text.NumberFormat"%>
<%@page import="java.sql.SQLException"%>
<%@page import="java.sql.Connection"%>
<%@page import="oracle.db.OracleConnect"%>
<%@page import="java.sql.ResultSet"%>
<%@page import="java.sql.PreparedStatement"%>
<%@ page language="java" contentType="text/xml; charset=UTF-8"
    pageEncoding="UTF-8"%>
<%-- jsp 주석2 : 소스보기에서도 안보인다. 완벽한 비실행문 --%>
<%-- <% %> : 스크립트릿(scriptlet)라고 부른다. 자바영역을 의미 --%>
<%-- <%=변수명 %> : 표현식이라고 한다. 자바의 변수값을 출력할 때 사용 --%>
<%
	// 변수 선언
	Connection conn = null;
	PreparedStatement pstmt = null;
	ResultSet rs = null;
	String sql="select * from sawon order by name asc";
	//자바프로젝트에서 복사해온 클래스
	OracleConnect db = OracleConnect.getInstance();
	//db 서버에 연결
	conn = db.getConnection();
%>
<sawon>
<%
	try{
		pstmt=conn.prepareStatement(sql);
		//실행
		rs=pstmt.executeQuery();
		// 데이터 가져오기
		while(rs.next()){
			String name = rs.getString("name");
			String buseo = rs.getString("buseo");
			int pay = rs.getInt("pay");
			int paytax = rs.getInt("paytax");
			// 컴마와 화폐단위 붙여서 문자열 형태로 만들기
			NumberFormat nf = NumberFormat.getCurrencyInstance();
			String spay = nf.format(pay);
			String stax = nf.format(paytax);
			%>
			<item>
				<name><%=name%></name>
				<buseo><%=buseo%></buseo>
				<pay><%=spay%></pay>
				<tax><%=stax%></tax>
			</item>
		<%}
	}catch(SQLException e){
		System.out.println("출력 오류 : "+e.getMessage());
	}
%>
</sawon>
```
### 3-2. sawonxmlread.html  
```html
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
<script src="https://code.jquery.com/jquery-3.5.0.js"></script>
<style type="text/css">
table{
		border: 2px solid gray;
	}
	
table tr, table th, table td{
		border-bottom: 1px solid gray;
		border-right: 1px solid gray;
	}
</style>
<script type="text/javascript">
window.onload=function(){
	if(window.XMLHttpRequest){
		var xhr=new XMLHttpRequest();
		//alert('크롬,ie')
	}else{
		alert('Ajax를 지원하지 않는 브라우저입니다');
	}
	document.getElementById('btn').onclick=function(){
		// method방식, 파일명, 비동기방식(true)
		xhr.open('get','sawon.xml',true);
		xhr.onreadystatechange=function(){
			if(xhr.readyState==4){
				if(xhr.status==200){
					//alert('성공\n'+xhr.responseText);
					// 하지만 xml데이터 형태로 받아야 분석이 가능하다
					var data=xhr.responseXML;
					// item 단위로 데이터 가져오기
					var itemNode=data.getElementsByTagName('item');
					//item 갯수 확인하기
					//alert(itemNode.length);
					var numNode = data.getElementsByTagName("num");
					var nameNode = data.getElementsByTagName('name');
					var buseoNode = data.getElementsByTagName("buseo");
					var payNode = data.getElementsByTagName('pay');
					var paytaxNode = data.getElementsByTagName('paytax');
					
					var s = '';
					for(i=0;i<itemNode.length; i++){
						s+= "<tr>";
						s+= "<td>"+numNode[i].childNodes[0].nodeValue+"</td>";
						s+= "<td>"+nameNode[i].childNodes[0].nodeValue+"</td>";
						s+= "<td>"+buseoNode[i].childNodes[0].nodeValue+"</td>";
						s+= "<td>"+payNode[i].childNodes[0].nodeValue+"</td>";
						s+= "<td>"+paytaxNode[i].childNodes[0].nodeValue+"</td>";
						s+= "</tr>";
					}
					document.getElementById("tb").innerHTML=s;
				}else{
					alert("error:"+xhr.status);
				}
			}
		}
		xhr.send(null);
	}
}
</script>
</head>
<body>
<button type='button' id=btn>목록 가져오기</button>
<hr>
<div>
	<table>
		<thead>
			<tr>
				<th width='100'>num</th>
				<th width='100'>name</th>
				<th width='100'>buseo</th>
				<th width='100'>pay</th>
				<th width='100'>paytax</th>
			</tr>
		</thead>
		<tbody id='tb'>
		
		</tbody>
	</table>
</div>
</body>
</html>
```



