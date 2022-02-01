# 스프링 데이터 JPA가 제공하는 Querydsl 기능
- 스프링 데이터 JPA에서 제공하는 기능이지만 제약이 커서 실무에선 사용하기 힘들다

# Querydsl Web 한계점

- 조인을 지원 안함
- 클라이언트 코드가 Querydsl에 의존해야 한다.
    - 서비스 클래스가 Querydsl이라는 구현 기술에 의존해야 한다.
- 조건을 커스텀하는 기능이 복잡함
- 복잡한 실무 환경에서 사용하기에는 한계가 명확

# QuerydslRepositorySupport

- QueryFactory를 사용하지 않고 쿼리를 작성할 수 있다.
- Pagination을 지원하는 querydsl 유틸리티 클래스를 제공함
- 한계점
    - sort 미지원
    - from으로 쿼리를 시작해야해서 가독성 감소

# Querydsl 지원/유틸 클래스 직접 만들기

- 스프링 데이터가 제공하는 페이징을 더 편리하게 변환
- 페이징과 카운트 쿼리 분리 가능
- 스프링 데이터 sort 지원
- select(), selectFrom()으로 시작 가능
- EntityManage, Querydsl 지원
