# JSP  
## 1. 탄생 배경  

### 1-1. 서버와 클라이언트  
client와 server간에 요청과 응답반복  

### 1-2. CGI(Common Gateway Interface) 프로그램  
- 웹 서버 안에서 동적인 웹페이지를 요청받을 때마다 실행처리됨  

### 1-3. servlet  
- 자바를 서버쪽에 사용하는 기술 중 하나  
- 클라이언트 요청마다 실행되는 CGI와는 다르게 스레드로 각 요청을 서비스  
- 이식성이 좋다(= 플랫폼의 종류에 상관없이 실행가능)  
   
## 2. JSP(JavaServer Pages)  
- 서블릿은 자바언어안에 HTML코드를 넣어야해서 불편해서 등장
- 자바 언어 기반으로 동적 웹페이지를 구축할 수 있는 Server-Side Script언어(자바 Servlet기반)  
- Servlet과 반대로 HTML문서 안에 필요할 때만 자바코드가 들어감
(<% %> 이용함)  

### 2-1. JSP엔진(JSP Container)  
-Web Server가 JSP를 처리하기 위해 필요한 Container  
ex) Apache Tomcat  

### 2-2. JSP 처리과정  
1) client가 URL을 통해 JSP페이지 요청  
2) Servlet 컨테이너가 JSP페이지를 검색  
3) 자바파일로 컴파일후  
4) 서블릿객체 생성  
5) 서블릿 컨테이너에 로딩되어 실행  
6) 웹서버를 통해 응답  

## 3. JSP 구성요소  
- Directive  
- 스크립트: scriptlet, 표현식, 선언부  
- 표현언어  
- 기본객체(Implicit Object)  
- 정적 데이터  
- 액션태그  
- 커스텀태그, 표준 태그 라이브러리(JSTL)  

## 3-1. 디렉티브(Directive)  
JSP 페이지에 대한 설정정보 지정 때 사용됨
- <%@ %>
- page, taglib, include  
### 3-1-1. page 디렉티브  
JSP 페이지에 대한 정보를 입력하기위해 사용
### 3-1-2. ContentType  
문서의 MIME타입을 입력(Multipurpose Internet Mail Extensions)  

## 3-2. 스크립트  
JSP에서 문서내용을 동적으로 생성하기 위해 사용되는 것  
- 표현식(Expression): 값 출력  
- Scriptlet: 자바코드실행  
- 선언부(Declaration): 자바 메서드를 만듬  
### 3-2-1. scriptlet  
```JSP
<% %> //내부에 자바코드가 위치함
```  
### 3-2-2. 표현식  
```jsp
<%=값 %>
```
### 3-2-3. 선언부  
```jsp
// 함수 선언 (자바와 똑같다)
```

## 3-3. 기본객체(Implicit Object)  
웹 어플리케이션 프로그래밍을 하는데 필요한 기능 제공  
ex) request, response, session, application, page....  
## 3-3 request 객체  
### 3-3-1. form으로 클라이언트 요청을 보낼때  
```html
<form action="./view.jsp" method="post">
   <input type="text" name="name">
   <button type="submit">전송</button>
</form>
```
form 태그안에 input 태그의 'name'속성을 파라미터로 서버에 요청을 보낼수 있다.
### 3-3-2. GET방식과 POST방식  
- GET방식은 URL에 쿼리문자열을 붙여서 전송한다  
- POST방식은 HTTP데이터영역을 이용하여 파라미터값을 전송하며 그덕분에 요청할 수 있는 파라미터의 길이에 제한이 없다  
- setCharactorEncoding()메서드는 HTTP 데이터 영역을 인코딩할 때 사용할 charSet을 지정한다.  
따라서 POST는 적용되지만 GET방식은 적용되지 않는다.  
따라서 GET방식 파라미터를 디코딩하기 위해선 server.xml파일에 useBodyEncodingForURI를 추가해준다.  

## 3-3 response 객체  
### 3-3-3. 캐시(Cache)  
브라우저가 요청의 응답 결과를 캐시에 임시로 보관하여 WAS의 중복로딩을 줄여주는 역할을 한다.
결과적으로 웹브라우저의 응답속도를 향상시킬 수 있다.  
### 3-3-4. 헤더추가  
해당 응답헤더에 캐시에 보관할건지 아닌지를 명시할 수 있다.  
### 3-3-5. 리다이렉트  
서버측에서 리다이렉트로 지정한 페이지에 다시 요청하라고 지시하는 기능(페이지 이동)  
```jsp
response.sendRedirect("이동할 페이지");
```

## 3-4. 표현 언어  
'${'와 '}'사이에 정해진 문법을 따르는 식을 사용함으로서 복잡한 코드를 간결하게 줄일 수 있다.  

## 3-5. 표준 액션태그와 태그라이브러리
표준 액션태그 : <jsp:액션태그이름>  
태그라이브러리 : 자주 사용하는 커스텀태그를 모아 표준화한 것(JSTL: JavaServer Pages Standard Tag Library)  










