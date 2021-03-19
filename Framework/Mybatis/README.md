# MyBatis  
- 'SQL Mapping 프레임워크'  
- JDBC코드의 복잡하고 지루한 작업을 회피하는 용도로 사용  
- 스프링은 다른 프레임워크들을 배척하는 대신에 다른 프레임워크들간의 연동을 쉽게 처리하는 추가적인 라이브러리들이 많다
Mybatis 역시 mybatis-spring 라이브러리를 통해 스프링 연동을 처리한다
- 일반 JDBC와 차이점  
### 1) 전통적인 JDBC 프로그램  
- 직접 Connection을 맺고 마지막에 close()
- PreparedStatement 직접 생성 및 처리  
- PreparedStatement의 setXXX() 등에 대한 모든 작업을 개발자가 처리  
- SELECT의 경우 직접 ResultSet 처리  
### 2) MyBatis  
- 자동으로 Connection close() 가능  
- MyBatis 내부적으로 PreparedStatement 처리  
#{prop}와 같이 속성을 지정하면 내부적으로 자동으로 처리  
- 리턴 타입을 지정하는 경우 자동으로 객체 생성 및 ResultSet처리  

