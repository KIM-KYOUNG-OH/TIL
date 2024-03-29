# 의존관계 주입 방법

- 생성자 주입
- 수정자 주입
- 필드 주입
- 일반 메서드 주입

# 생성자 주입

- 생성자 호출 시점에 딱 1번만 호출되는 것이 보장된다.
- immutable(불변), required(필수)인 의존관계에 주로 사용
    - 추가 객체 상태를 수정하는 메서드를 더이상 안만들면 불변이 됌
    - 생성자의 인자는 필수값
- 생성자가 딱 1개일 경우 @Autowired 생략 가능

# 수정자 주입

- @Autowired가 붙은 setter에서 의존관계 주입이 이루어짐
- required = false를 이용해서 선택적으로 의존관계 주입 가능
- 런타임중에 수정자를 호출해서 의존관계 변경 가능

# 필드 주입

- 필드 자체에 @Autowired를 붙임
- 안티 패턴
    - 필드 주입은 스프링 컨테이너가 있을 경우만 적용 가능
    - 테스트시 필드 주입으로는 의존관계 주입이 불가 → 별도의 의존관계를 주입할 setter를 만들어야함
    - 단, 애플리케이션과 실제 관계 없는 테스트 코드 안에서는 필드 주입 사용 가능

# 일반 메서드 주입

- 아무 메서드에 @Autowired를 붙여서 의존관계 주입
- 수정자 주입이 있기 때문에 특별한 경우가 아닌 이상 사용하지 않음
- 의존관계 자동 주입은 스프링 컨테이너가 관리하는 스프링빈이어야 동작한다.

---

# 옵션 처리

- @Autowired(required = true) 가 default임
- @Autowired(required = false) : 자동 주입할 대상이 없으면 수정자 메서드 자체가 호출 안됨
- @Nullable : 자동 주입할 대상이 없으면 null이 입력됨
- Optional<> : 자동 주입할 대상이 없으면 Optional.empty가 입력됨

---

# 생성자 주입을 선택하라

- 과거에는 수정자 주입과 필드 주입을 많이 사용했지만 최근에는 생성자 주입을 권장함
- 생성자 주입은 객체를 생성할 때 딱 1번만 호출되므로 불변하게 설계할 수 있다.
- final 사용 가능
    - final을 사용하면 초기값 대입이나 생성자를 통해서만 값을 변경할 수 있다.
    - 혹시라도 값이 설정되지 않는 오류를 컴파일 시점에서 막아줌

---

# 롬복과 최신 트렌드

- 막상 개발을 하다보면 대부분이 다 불변이다.
    - @RequiredArgsConstructor 사용

---

# 조회 빈이 2개 이상일 때

- @Autowired는 타입으로 조회하는데 만약 해당 타입의 하위 타입이 2개 이상이면 어떻게 될까?
    - 하나의 타입에 두개의 스프링 빈이 등록되었다고 오류 메시지로 알려줌
    - 하위 타입으로 지정할 수 있지만 그건 DIP 위반이므로 유연성이 떨어짐

# 조회 대상 빈이 2개 이상일 때 해결방법

# Autowired 필드명 매칭

- 타입의 매칭 결과가 2개 이상이면, 필드명과 같은 객체를 주입한다.

# @Qualifier

- @Qualifier는 추가 구분자를 붙여주는 방법이다.
- 추가적인 방법을 제공하는 것이지 빈 이름을 변경하는 것이 아니다.
- @Qualifier는 스프링 빈의 이름도 찾을 수 있지만 @Qualifier를 찾는 용도로만 쓰자
- 단점은 모든 코드에 @Qualifier를 붙여야 함

# @Primary

- 우선순위를 정하는 방법

# @Qualifier와 @Primary 둘다 쓰였을 때 우선순위

- @Qualifier가 우선
- 항상 더 명시적인 것이 우선이다.

---

# 어노테이션 직접 만들기

- @Qualifier
- 어노테이션은 상속이라는 개념이 없다. → 여러 어노테이션을 모아서 사용하는 기능은 스프링이 지원하는 기능
- 무분별한 사용자 정의 어노테이션은 오히려 유지보수에 더 혼란을 줄 수 있다.

---

# 조회한 빈이 모두 필요할 때, List, Map

- 의도적으로 해당 타입의 스프링 빈을 여러개 등록하고 필요할때마다 의존성 주입시키는 방법도 있다.

---

# 자동, 수동 빈 등록의 올바른 실무 운영 기준

- 자동 기능을 Default로 사용하자
- 스프링 빈을 등록할 때 @Component만 붙여주면 끝나는 일을 @Configuration 설정 정보에 가서 @Bean을 붙이고 객체를 생성하고, 주입할 대상을 일일이 적어주는 과정은 번거롭다.
- 관리할 빈이 많아질수록 설정 정보 관리 비용의 증가
- 자동 빈 등록을 사용해도 OCP, DIP 를 지킬 수 있다.

# 수동 빈 등록은 언제 사용하면 좋을까?

- 애플리케이션은 업무 로직과  기술 지원 로직으로 나눌 수 있다.
    - 업무 로직 빈: 웹을 지원하는 Controller, Service, Repository가 포함됨. 보통 비즈니스 요구사항을 개발할 때 추가되거나 변경됨
    - 기술 지원 빈: 기술적인 문제나 공통 관심사(AOP)를 처리할 때 주로 사용. 데이터베이스 연결이나 공통 로그 처리처럼 업무 로직을 지원하기 위한 하부 기술이나 공통 기술들이다.
- 업무 로직은 자동 기능을 사용하자. 문제가 발생해도 어떤 곳에서 문제가 발생했는지 명확하게 파악하기 쉽다.
- 기술 지원 로직은 애플리케이션 전반에 걸쳐서 광범위하게 영향을 미친다.
- 기술지원 객체는 수동 빈으로 등록해서 딱 설정 정보에 나타나게 하는 것이 유지보수하기 쉽다.
- 비즈니스 로직 중에서 다형성을 적극 활용할 때도 수동 빈을 등록하는게 좋은 경우도 있다.
    - 설정 정보만 봐도 한눈에 어떤 빈들이 주입될지 파악할 수 있다.
    - 또는 특정 패키지에 같이 묶어 두는게 좋다.

# 심화

- 자바빈 프로퍼티 규약
    - 필드 값을 직접 변경하지 않고 setter, getter를 통해 값을 조회/변경하는 것
- 해당 타입의 구현체 찾기 단축키 : command + option + B
