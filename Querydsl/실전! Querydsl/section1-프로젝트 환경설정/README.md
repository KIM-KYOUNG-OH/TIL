- gradle이 아닌 intellij에게 실행 및 테스트 권한 위임

<img width="1275" alt="스크린샷 2022-01-30 오전 1 15 01" src="https://user-images.githubusercontent.com/66231761/151668400-bfd12d9b-c547-477c-b690-3a8234a5dd37.png">

- lombok setting
    - ‘Enable annotation processing’ 체크

<img width="1274" alt="스크린샷 2022-01-30 오전 1 15 17" src="https://user-images.githubusercontent.com/66231761/151668412-a2f7b420-58a0-4dab-b726-0b8a5c0155d2.png">

# Querydsl 설정과 검증

- build.gradle
    
    ```gradle
    buildscript {
        ext {
            queryDslVersion = "5.0.0"
        }
    }
    
    plugins {
    	id 'org.springframework.boot' version '2.6.3'
    	id 'io.spring.dependency-management' version '1.0.11.RELEASE'
    	id "com.ewerk.gradle.plugins.querydsl" version "1.0.10"
    	id 'java'
    }
    
    group = 'study'
    version = '0.0.1-SNAPSHOT'
    sourceCompatibility = '11'
    
    configurations {
    	compileOnly {
    		extendsFrom annotationProcessor
    	}
    }
    
    repositories {
    	mavenCentral()
    }
    
    dependencies {
    	implementation 'org.springframework.boot:spring-boot-starter-data-jpa'
    	implementation 'org.springframework.boot:spring-boot-starter-web'
    	implementation "com.querydsl:querydsl-jpa:${queryDslVersion}"
    	implementation "com.querydsl:querydsl-apt:${queryDslVersion}"
    
    	compileOnly 'org.projectlombok:lombok'
    	runtimeOnly 'com.h2database:h2'
    	annotationProcessor 'org.projectlombok:lombok'
    	testImplementation 'org.springframework.boot:spring-boot-starter-test'
    }
    
    test {
    	useJUnitPlatform()
    }
    
    //querydsl 추가 시작
    def querydslDir = "$buildDir/generated/querydsl"
    
    querydsl {
        jpa = true
        querydslSourcesDir = querydslDir
    }
    sourceSets {
        main.java.srcDir querydslDir
    }
    compileQuerydsl {
        options.annotationProcessorPath = configurations.querydsl
    }
    configurations {
        compileOnly {
            extendsFrom annotationProcessor
        }
        querydsl.extendsFrom compileClasspath
    }
    //querydsl 추가 끝
    ```
    
- ‘./gradlew compileQuerydsl’나 ‘./gradlew java’를 실행하면 entity 데이터를 읽고 build 폴더하위에 Q클래스가 생성됨

    <img width="501" alt="스크린샷 2022-01-30 오전 1 15 44" src="https://user-images.githubusercontent.com/66231761/151668442-84fb9504-32b8-4c86-aa71-1676ac513187.png">

    - 단, generated 파일은 git으로 관리해선 안된다.

# 라이브러리 살펴보기

- com.querydsl:querydsl-apt:5.0.0
    - 코드 generation 용도
- com.querydsl:querydsl-jpa:5.0.0
    - 애플리케이션 코드 작성을 위한 핵심 라이브러리

# 참고

- [http://querydsl.com/](http://querydsl.com/)
- [https://joel-costigliola.github.io/assertj/index.html](https://joel-costigliola.github.io/assertj/index.html)

# H2 Database 설치

- 홈페이지에서 h2 database 설치
- 권한 주기
    - `chmod 755 h2.sh`
- 데이터베이스 파일 생성하기
    - `jdbc:h2:~/querydsl`
        - 파일 모드 최소 한번 실행하면 home 폴더에 ~/querydsl.mv.db 파일 생성됨
    - 이후부턴 tcp 모드로 접속
        - `jdbc:h2:tcp://localhost/~/querydsl`

# Test

- application.yml
    
    ```graphql
    spring:
      datasource:
        url: jdbc:h2:tcp://localhost/~/querydsl
        username: sa
        password:
        driver-class-name: org.h2.Driver
    
      jpa:
        hibernate:
          ddl-auto: create
        properties:
          hibernate:
    #        show_sql: true
            format_sql: true
    
    logging.level:
      org.hibernate.SQL: debug
    #  org.hibernate.type: trace
    ```
    
    - ddl-auto: create 하면 애플리케이션 로딩시 테이블을 전체 drop하고 다시 create함
- p6spy 라이브러리를 설치하면 동적 쿼리에 바인딩된 런타임 쿼리를 콘솔화면에서 볼 수 있다.
    - 단, 설계 단계의 라이브러리를 운영 서버에 남길지는 성능 테스트를 통해 결정해야 한다.
