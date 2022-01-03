- 지금까지 스프링 빈을 등록할 때는 자바 코드의 @Bean을 통해 스프링 빈을 나열했다.
- 대규모 프로젝트에선 반복과 데이터 누락 등의 문제가 발생할 수 있다.
- 스프링은 설정 정보가 없어도 자동으로 스프링 빈을 등록하는 컴포넌트 스캔이라는 기능을 제공한다.
- @Component 어노테이션이 붙은 클래스를 스캔해서 스프링 빈으로 등록한다.
- @Autowired를 쓰게되면 스프링이 타입에 맞는 스프링 빈을 등록해준다.

# @ComponentScan

<img width="900" alt="스크린샷 2022-01-04 오전 1 00 59" src="https://user-images.githubusercontent.com/66231761/147952230-fb11e810-6af2-4bc9-be93-76661dcab761.png">

- @ComponentScan을 붙인 설정 클래스는 @Componet가 붙은 모든 클래스를 스프링 빈으로 등록한다.
- properties
    - @Configuration을 붙인 클래스도 스프링 빈으로 등록되기 때문에 excludeFilters를 이용해서 컴포넌트 스캔 대상에서 제외한다.
    - basePackages properties를 사용하면 컴포넌트 스캔할 패키지를 지정할 수 있다.
    - basePackageClasses
    - Default는 @ComponentScan이 붙은 설정 정보 클래스의 패키지 시작 위치가 됨
        - 권장하는 방법: 설정 정보 클래스의 위치를 프로젝트 최상단에 두자
        - 권장하는 방법: @SpringBootApplication이 붙은 클래스와 같은 level에 두자
            - @SpringBootApplication에 @ComponentScan이 속해있다.
- 이때 스프링 빈의 기본 이름은 클래스 이름을 그대로 사용하되 맨 앞글자만 소문자를 사용한다.

# @Autowired

- @Autowired를 붙이면 스프링 컨테이너가 자동으로 해당 타입이 같은 스프링 빈을 찾아서 주입한다.
- ac.getBean()와 동일하다고 이해하면 된다.

# 컴포넌트 스캔 기본 대상

- @Component가 붙은 클래스 뿐만 아니라 아래의 대상도 포함된다.
    - @Controller
    - @Service
    - @Repository
    - @Configuration

---

# 필터

- properties
    - includeFilters: ComponentScan 대상 포함
    - excludeFilters: ComponentScan 대상 불포함
- FilterType 옵션
    - ANNOTATION: 기본값, 어노테이션을 인식해서 동작
    - ASSIGNABLE_TYPE
    - ASPECTJ
    - REGEX
    - CUSTOM

---

# 중복 등록과 충돌

1. 자동 빈 등록 vs 자동 빈 등록
2. 수동 빈 등록 vs 자동 빈 등록

# 자동 빈 등록 vs 자동 빈 등록

- 컴파일 오류 발생
    - ConflictBeanDefinitionException

# 수동 빈 등록 vs 자동 빈 등록

- 이 경우 수동 빈 등록이 우선권을 가지며 수동 빈이 자동 빈을 Overriding한다.
    - 그러나 이 경우 찾기 어려운 버그를 초래하기 때문에 스프링 부트는 수동 빈 등록과 자동 빈 등록이 충돌날 경우 오류가 나도록 바뀌었다.
        - spring.main.allow-bean-definition-overriding=false

# 심화

- Annotation 생성하기
    - Annotation 하위에 Annotation을 상속하는 기능은 자바 언어가 아닌 스프링에서 지원하는 기능이다.
- 코드가 길어지거나 중복이 발생하더라도 명확하고 오류를 빨리 찾을 수 있는 길을 선택하자
