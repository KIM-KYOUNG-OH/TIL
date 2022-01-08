# 목차  
- 프로젝트 생성
- 라이브러리 살펴보기
- view 환경설정
- H2 데이터베이스 설치
- JPA와 DB 설정, 동작 확인

# 프로젝트 생성

- spring initializr
- library 추가
    - sprinb boot web starter
    - thymleaf
    - spring data jpa

# 라이브러리 살펴보기

- org.springframework.boot:spring-boot-starter-web:2.6.2
    - 내장 톰캣 보유
    - org.springframework:spring-webmvc 의존
- org.springframework.boot:spring-boot-starter-thymeleaf:2.6.2
- org.springframework.boot:spring-boot-starter-data-jpa:2.6.2
    - org.springframework.boot:spring-boot-starter-jdbc:2.6.2
        - com.zaxxer:HikariCP:4.0.3 : 커넥션 풀
    - ch.qos.logback:logback-classic:1.2.9 : 로깅관련(slf4j 구현체)
    - org.apache.logging.log4j:log4j-to-slf4j:2.17.0 : 인터페이스의 모음
- org.springframework.boot:spring-boot-devtools:2.6.2

# 핵심 라이브러리

- 스프링 MVC
- 스프링 ORM
- JPA, 하이버네이트
- 스프링 데이터 JPA
    - 스프링과 JPA를 먼저 이해하고 사용해야 하는 응용 기술

# 기타 라이브러리

- H2 데이터베이스 클라이언트
- 커넥션 풀: HikariCP
- thymleaf
- 로깅 SLF4J & LogBack
- 테스트

---

# View 환경 설정

- Thymleaf
    - [https://www.thymeleaf.org/](https://www.thymeleaf.org/)
    - Natural template
        - 랜더링이 편함
    - 웹브라우저에서 바로 열림
    - serverside rendering
- devtools 라이브러리를 사용하면 동적페이지를 수정하고 rebuild만하면 브라우저에서 변경된 내용을 확인 가능하다(서버를 다시 띄우지 않아도 됨!!)

---

# H2 Database

- 교육용으로 가벼운 Database
- http://localhost:8082접속
- jdbc:h2:~/jpashop 세션키 유지한 상태로 실행
- ~/jpashop.mv.db 파일 생성 확인
- 이후부터 jdbc:h2:tcp://localhost/~/jpashop 으로 접속

# JPA와 DB 동작 확인

- application.yml 작성

# 심화

- application.yml 작성법
    - [spring.io](http://spring.io) guide 참고
- @Transactional
    - test에서 사용할 경우 test후 결과를 rollback함
        - @Rollback(false)하면 rollback 취소 가능
    - EntityManager는 Transaction 안에서 동작해야함
- spring boot datasource decorator
- 개발 서버와 운영 서버에서 쓸 라이브러리를 구분하자(by 성능 테스트)
