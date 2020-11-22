# WEB  
## 1. URL(Uniform Resource Locator)  
웹 브라우저 주소창에 입력하는 것  
기본구성(예시) : http://www.11st.co.kr/html/category/1.html?xzone=ctgrl^html  
프로토콜 + 서버이름 + 경로 + 쿼리문자열  

## 2. 웹브라우저(Client)와 웹서버(Server)  
### 2-1. 웹페이지 출력과정  
1) 웹브라우저 -> DNS(Domain Name Server)  
  도메인 주소의 IP주소 변환값 요청  
2) DNS -> 웹브라우저  
  IP주소 응답  
3) 웹브라우저 -> 웹서버  
  URL 요청  
4) 웹서버 -> 웹브라우저  
  HTML 응답  

### 2-2. IP주소  
연결할 컴퓨터를 구분하기위해 사용  
### 2.3 포트번호  
기능에 따라 사용할 서버프로그램을 구분하기 위해 사용  

## 3. HTTP(HyperText Transfer Protocol)  
클라이언트와 서버간 요청/응답 규칙  

## 4. 정적자원과 동적자원  
### 4-1. 정적자원  
파일이 바뀌기 전까지 항상 같은 결과의 자원  
### 4-2. 동적자원  
입력시점이나 조건에 따라 응답데이터가 바뀌는 자원  

## 5. JSP(Java Server Pages)  
동적 페이지를 작성하는데 사용하는 자바표준기술  
참고: [JSP.md](https://github.com/KIM-KYOUNG-OH/bitcamp_academy/blob/master/%EC%9A%94%EC%95%BD%EC%A0%95%EB%A6%AC(MarkDown)/JSP.md)
## 6. WAS(Web Application Server)
웹 브라우저에 맞는 프로그램을 찾아실행하고 응답을 전송 
