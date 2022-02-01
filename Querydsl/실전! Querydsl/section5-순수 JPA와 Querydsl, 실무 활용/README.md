# 순수 JPA와 Querydsl, 실무 활용
- 순수 JPA Repository
- 동적 쿼리 Builder 적용
- 동적 쿼리 Where 적용
- 조회 API 컨트롤러 개발

# 순수 JPA vs Querydsl

- JPA createQuery() 메서드를 이용해서 jpql을 사용하면 문자열로 쿼리를 작성하므로 런타임이 되서야 오류를 발견할 수 있지만 Querydsl은 java코드로 쿼리를 작성하기 때문에 컴파일 타임에 오류를 발견할 수 있다.
- querydsl은 .(점)을 통해서 코드를 추천받을 수 있다.
- 파라미터 바인딩시 JPA JPQL은 setParameter() 메서드로 추가로 바인딩해야하는 코드를 작성해야하지만, Querydsl은 애초에 java코드이므로 추가 바인딩해야하는 코드가 없다.

# JPAQueryFactory

- querydsl을 사용하기 위해선 jpaQueryFactory가 필요한데 두가지 방법으로 생성한다.
    - JPAQuerydsl을 빈으로 등록하는 방법
        - lombok requiredConstructor을 사용할 수 있다는 장점이 있다.
    - 생성자 주입에서 new JPAQueryFactory(em);으로 생성하는 방법
        - 주입 받아야하는 파라미터가 하나라 테스트가 편하다.
- EntityManager는 싱글톤으로 관리되기 때문에 동시성 에러가 나지 않을까?
    - spring은 EntityManager를 transaction 단위로 proxy로 생성해서 사용하도록 지원하기 때문에 동시성 에러가 나지 않는다.

# 동적 쿼리와 성능 최적화 조회 - Builder 사용

- compileQuerydsl 명령으로 dto에 대한 Q파일 생성
    - dto가 querydsl에 종속적이게 된다.

# 동적 쿼리와 성능 최적화 조회 - where절 사용

- Builder 방식과 다르게 코드 재사용 가능
- BooleanExpression을 반환하는 여러 condition들 조합 가능

# 조회 API 컨트롤러 개발

- local에서 tomcat을 띄울때와 test를 할때 sample 데이터를 넣는 properties.yml를 분리하자
- profile: local, dev, real, test 구분

```
spring:
  profiles:
    active: local
```

- @PostConstructor로 api를 띄울시 샘플 데이터를 insert함
    - @PostConstructor와 @Transactional은 동시에 사용 불가하므로 임시Service를 만든다.
