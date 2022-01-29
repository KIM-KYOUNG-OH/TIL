# 프로젝션

- 프로젝션: select 대상 지정
- 프로젝션 대상이 하나면 타입을 명확히 지정할 수 있음
- 프로젝션 대상이 둘 이상이면 튜플이나 DTO로 조회
- tuple을 service 계층에서 사용하는 것은 좋지 않은 설계다
    - tuple은 JPA에 의존하기 때문에 service에서 사용하면 JPA에 종속적이게 된다.

# Dto로 결과 받기

- setter 방식 - bean()
- field()
- 생성자 방식

# 프로젝션 결과 반환 - @QueryProjection

- 위의 setter, field, 생성자 방식은 없는 컬럼을 호출했을때 런타임 시점에야 오류를 발견할 수 있는데 Qtype을 사용하는 방식을 사용하면 컴파일 타임에 오류를 발견할 수 있다.
- 단점
    - Q파일을 생성해야 한다.
    - DTO가 querydsl 라이브러리에 대한 의존성을 가지게 된다.

# 동적 쿼리

- 검색 조건에 따라 where절에 파라미터를 넣을지 안넣을지
- BooleanBuilder
- where 다중 파라미터 사용

# 동적 쿼리 - where 다중 파라미터 사용

- BooleanBuilder보다 깔끔함
- where에 null이 파라미터로 들어오면 무시됨
- 장점
    - 여러 조건들을 하나로 조합할 수 있다.
    - 코드 재사용 가능
    - 쿼리 자체의 가독성이 높아짐
    - 단, null 체크 처리 주의

# 수정, 삭제 배치 쿼리

- 쿼리 한번으로 대량 데이터 수정
- Dirty checking에 의한 update 쿼리는 단건 query가 여러번 나가는데 성능 최적화를 위해선 한번에 update하는게 나을 경우도 있다.
- bulk 연산의 단점
    - bulk update후 영속성 컨텍스트의 내용과 db의 내용이 다르기 때문에 조회 쿼리를 날리면 영속성 컨텍스트의 내용이 조회된다(영속성 컨텍스트가 우선권을 가지기 때문)
    - 따라서 bulk 연산후 무조건 영속성 컨텍스트를 초기화 해주자

# SQL Function 호출하기

- SQL Function은 JPA와 같이 Dialect에 등록된 내용만 호출할 수 있다.
