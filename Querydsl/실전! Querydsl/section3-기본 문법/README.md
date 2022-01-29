# JPQL vs Querydsl

- Querydsl은 파라미터 바인딩시 JPA prepare statement의 parameter binding 방식을 사용한다.
- JPQL은 문자열로 작성하기 때문에 오류가 실행시점에 발생한다.
- Querydsl은 컴파일타임에 sql 오류를 잡아줌
- 쿼리도 자바코드로 작성할 수 있다.

# 기본 Q-Type 활용

- Q-Type 생성시 내부적으로 생성된 인스턴스를 제공한다.
- QXXXX.xxxx로 접근함
    - static import시 코드를 줄일 수 있다.

# 검색 조건 쿼리

- where의 and는 .and()말고도 ‘,’ 구분자를 사용해도 상관없다.

# 결과 조회

- fetch() : 리스트 반환
- fetchOne() : 단건 조회(결과가 없으면 null, 결과가 둘 이상이면 NonUniqueResultException)
- fetchFirst() : limit(1).fetchOne()
- fetchResults() : 페이징 정보 포함, total count 쿼리 추가 실행
- fetchCount() : count 쿼리로 변경해서 count 수 조회

# 조인 - 기본 조인

- 조인의 기본 문법은 첫 번째 파라미터에 조인 대상을 지정하고, 두 번째 파라미터에 별칭으로 사용할 Q 타입을 지정하면 된다.
- 세타조인
    - from 절에서 여러 엔티티를 나열해서 조인
    - 외부 조인 불가능 → on을 사용하면 가능

# 조인 - on 절

- 조인 대상 필터링
- 연관관계 없는 엔티티를 외부 조인

# Fetch 조인

- sql에서 제공하는 기능이 아니다.
- 성능 최적화를 위해 연관된 엔티티를 쿼리 한방에 조회하는 방법이다.

# 서브쿼리

- com.querydsl.jpa.JPAExpressions 사용
- jpa에서 서브쿼리를 사용할 때
    - from절의 서브쿼리(인라인 뷰)는 지원하지 않는다.
    - Querydsl도 지원하지 않는다.
    - 해결방안
        - 서브쿼리를 join으로 변경한다.(일반적으로 join으로 바꿀수 있을 것이다.)
        - 애플리케이션에서 쿼리를 2번 분리해서 실행한다.
        - nativeSQL을 사용한다.
- 궁극적으로 서브쿼리를 줄이고 쿼리를 여러 작은 단위 쿼리로 쪼개는 것이 더 나은 해결방법이지 않을까 고민해보자

# Case 문

# 상수, 문자 더하기

# 심화

- 오류가 있는 라인으로 바로가기 단축키: F2
- leftJoin vs innerJoin vs join
