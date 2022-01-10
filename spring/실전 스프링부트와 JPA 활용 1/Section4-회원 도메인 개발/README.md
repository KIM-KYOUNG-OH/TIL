# 구현 기능

- 회원 등록
- 회원 목록 조회

# 회원 레포지토리 개발

- @Repository : component scan의 대상으로 만듬
- @PresistenceContext : EntityManager를 주입해줌
    - spring-data-jpa는 @PersistenceContext(표준 어노테이션)를 @Autowired로 바꿀 수 있도록 지원함
- jpql: 객체를 대상으로하는 쿼리
- @GeneratedValue 전략에선 em.persist()호출시 id가 insert되는게 아니라 commit시 insert됨

# 회원 서비스 개발

- @Transactional
    - 모든 서비스코드는 트랜잭션 안에서 실행되야 한다.
    - lazy loading을 사용하려면 추가하자
    - readOnly = true시 spring이 성능 최적화함

# 회원 기능 테스트

- 테스트 요구사항
    - 회원가입을 성공해야 한다.
    - 회원가입할 때 같은 이름이 있으면 예외가 발생해야 한다.
- @Transactional이 테스트에 있으면 테스트후 자동으로 rollback됨
    - 그래도 db변경 쿼리가 보고싶으면 EntityManager를 주입받아서 em.flush()를 호출하면됌
    - 또는 @Rollback(false)를 해주면 됌
- @SpringBootTest
    - spring container를 띄우고 테스트 하고 싶을 때
- 메모리 DB
    - 테스트용 작은 DB 사용
    - test 패키지에 별도의 application.yml 생성
    - 별도의 설정이 없으면 spring이 메모리 DB로 설정해버림
    - ddl-drop으로 동작함
        - ddl-drop은 애플리케이션 종료시 drop 쿼리를 수행함

# 심화

- Inline code style 단축키: option + command + N
- dirty checking
- 필드 주입과 생성자 주입을 사용하지 않는 이유
- 테스트용 메모리 DB 띄우는 전략
