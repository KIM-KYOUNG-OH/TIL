# 목차

- 홈 화면과 레이아웃
- 회원 등록
- 회원 목록 조회
- 상품 등록
- 상품 목록
- 상품 수정
- 변경 감지와 병합(merge)
- 상품 주문
- 주문 목록 검색, 취소

# validation

- @Valid
- @NotEmpty
- `BindingResult`
    - validation 에러가 나더라도 코드를 실행 → 에러화면 데이터 담아서 띄우기 가능
- form 화면으로 부터 받는 데이터를 별도의 DTO를 통해서 받자
    - entity와 구분하자
    - 단순히 웹상에 랜더링하기 위한 목적이면 선택적으로 entity를 그대로 반환해도 됨
    - API를 개발할 땐 이유불문 DTO를 사용해서 웹상에 반환해야함

# 수정기능

- id를 url로 넘길때 위부에서 id를 바꿔서 다른 유저의 정보를 바꾸도록 공격할 수 있기 때문에 유저의 권한을 체크해주는 로직을 추가하자
- EntityManager.merge() 란?

---

# 변경감지와 병합(merge)

## 준영속 엔티티란?

- 영속성 컨텍스트가 더이상 관리하지 않는 엔티티를 말함
- ex) 식별자를 통해서 DB에서 꺼내온 객체
- 준영속 엔티티는 JPA가 관리하지 않기 때문에 변경감지(Dirty Checking)가 일어나지 않는다

# 준영속 엔티티를 수정하는 방법 2가지

- 그럼에도 불구하고 변경감지 기능을 사용하는 방법
    - @Transactional 어노테이션을 사용해서 메서드 완료후 EntitiyManager.flush()가 호출됨
- 병합(merge)

# merge()

- merge(member)에서 파라미터로 보낸 객체가 없으면 db에서 가져오고 변경내용을 밀어넣고 영속성 엔티티를 반환한다.

# 변경감지 vs merge

- 변경감지는 영속성 컨텍스트로 관리할 속성을 선택할 수 있지만 merge는 모든 속성을 관리한다.
- 어떤 속성은 불변으로 제약조건을 줄 수도 있기 때문에 merge는 사용하면 안된다.
- 결론은 merge를 쓰지말고 항상 변경감지를 사용해야한다.

# Controller에서 어설프게 엔티티를 생성하지 말자

- 웹 계층 DTO를 서비스 계층에 보내지 말고 차라리 필드만 보내자
- 또는 서비스계층 DTO를 따로 선언하자

---

# Dirty Checking

- JPA가 영속성 관리하는 라이프 사이클을 주의하자

# API 만들기

- MSA가 등장하면서 서버끼리 통신하기 위해 API를 이용한 통신을 자주 사용한다.
