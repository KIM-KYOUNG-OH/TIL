# 스프링 컨테이너 생성

- ApplicationContext를 스프링 컨테이너라고 함
- ApplicationContext는 인터페이스이다.
- XML기반이나 어노테이션 기반으로 자바 설정 클래스를 만들 수 있다.

```java
ApplicationContext ac = new AnnotationConfigApplicationContext(AppConfig.class);
```

1. 스프링 컨테이너 생성

![스크린샷 2021-12-28 오전 2 03 19](https://user-images.githubusercontent.com/66231761/147492629-4f565dd4-02c9-4efc-a98d-3d6f3caf0dba.png)

2. 스프링 빈 등록

![스크린샷 2021-12-28 오전 2 03 44](https://user-images.githubusercontent.com/66231761/147492658-2533345e-d696-41dd-927f-09c233e7af6a.png)

- @Bean으로 지정된 함수를 전부 호출해서 스프링 빈으로 등록
- 빈 이름은 항상 다른 이름을 부여해야 한다.

3. 스프링 빈 의존관계 설정

![스크린샷 2021-12-28 오전 2 04 10](https://user-images.githubusercontent.com/66231761/147492675-f492ba0e-2647-4b3e-846a-b672e6b68e2f.png)

- 설정 정보를 참조하여 동적인 의존관계를 주입함

---

# 컨테이너에 정상 등록되었는지 테스트하기

- `ROLE_APPLICATION`

# 스프링 빈 조회

- ac.getBean(빈 이름, 타입)
- ac.getBean(타입)
- ac.getBeanOfType() : 해당 타입의 모든 빈 조회

# 스프링 빈 조회 - 상속 관계

- 부모 타입으로 조회하면, 자식 타입도 함께 조회한다.
- 즉, 모든 클래스의 최고 부모인 Object타입으로 조회하면 모든 스프링 빈을 조회한다.

# BeanFactory와 ApplicationContext

# BeanFactory

- 스프링 컨테이너의 최상위 인터페이스
- 스프링 빈을 관리하고 조회하는 역할 담당
- getBean() 제공
- BeanFactory를 직접 사용할 일은 거의 없다

# ApplicationContext

- BeanFactory의 기능을 상속받음
- BeanFactory에서 제공하는 기능 말고도 수많은 부가기능을 제공
    - 메시지소스를 활용한 국체화 기능
    - 환경변수 : 로컬, 개발, 운영 등을 구분해서 처리
    - 애플리케이션 이벤트 : 이벤트 발행, 구독
    - 편리한 리소스 조회 : 파일, 클래스패스, 외부 등에서 리소스를 편리하게 조회

---

# XML 설정 형식 지원

- 스프링 컨테이너는 다양한 형식의 설정 정보를 받아드릴 수 있게 유연하게 설계됌

![스크린샷 2021-12-28 오전 2 04 33](https://user-images.githubusercontent.com/66231761/147492702-d321bf82-a044-444e-b542-151a78f2dbc6.png)
    
- 어노테이션 기반 자바 코드 설정
- XML 기반 설정
    - 최근에는 잘 안씀
    - 컴파일 없이 설정 정보를 변경할 수 있다는 장점

---

# 스프링 빈 설정 메타 정보 - BeanDefinition

- BeanDefinition 역할을 통해 스프링은 다양한 설정 형식을 지원한다.
- 스프링 컨테이너는 자바 코드인지 XML인지 몰라도 되고 오직 BeanDefinition만 알면 된다.
- 스프링 컨테이너는 BeanDefintion이라는 메타정보를 기반으로 스프링 빈을 생성한다.
- 실무에서 ApplicationContext를 사용하는 입장에서, BeanDefinition을 직접 정의하거나 사용하는 일은 거의 없다
- 자바 코드에서 annotation을 사용하는 방식은 팩토리 메서드 방식이다.

# BeanDefinition 정보

- BeanClassName
- factoryBeanName
- factoryMethodName
- Scope
- lazyInit
- InitMethodName
- DestroyMethodName
- Constructor arguments, Properties

# 심화

- 로컬 영역에 list나 array가 있으면 반복문 자동 생성해주는 단축키 : iter 입력후 Enter
- 메서드명 찍는 단축키 : soutm 입력후 Enter
- 필드명 찍는 단축키: soutv 입력후 Enter
- 코드 복사 : command + D
- 코드 해치지 않고 다음줄로 넘어가기 단축키 : command + shift + enter
