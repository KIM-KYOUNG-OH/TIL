# 환경 설정
- java 11
- IntelliJ
- [https://start.spring.io/](https://start.spring.io/)

# 비즈니스 요구사항 및 설계

- 회원
    - 회원을 가입하고 조회할 수 있다.
    - 회원은 일반과 VIP 두 가지 등급이 있다.
    - 회원 데이터는 자체 DB를 구축할 수 있고, 외부 시스템과 연동할 수 있다.(미확정)
- 주문과 할인 정책
    - 회원은 상품을 주문할 수 있다.
    - 회원 등급에 따라 할인 정책을 적용할 수 있다.
    - 할인 정책은 모든 VIP는 1000원을 할인해주는 고정 금액 할인을 적용(나중에 변경될 수 있다.)
    - 할인 정책은 변경 가능성이 매우 높다. 오픈 전까지 적용을 미루고 싶다.

- 요구사항을 보면 회원 데이터, 할인 정책 등을 지금 당장 결정할 수 없다.
- 이를 객체 지향 설계로 극복 가능
- 순수 자바로 개발

# 회원 도메인 설계

<img width="600" alt="스크린샷 2021-12-22 오전 1 54 40" src="https://user-images.githubusercontent.com/66231761/146968559-a5eff73f-8880-4e03-8f72-77d948dd36da.png">

<img width="600" alt="스크린샷 2021-12-22 오전 1 54 51" src="https://user-images.githubusercontent.com/66231761/146968588-d274ae01-cfaa-4ecb-b299-00f606a053be.png">

### 회원 객체 다이어그램  
<img width="600" alt="스크린샷 2021-12-22 오전 1 56 34" src="https://user-images.githubusercontent.com/66231761/146968832-cf3526ee-fe33-408b-8522-d9b65d9f0502.png">


- 도메인 협력 관계: 기획자들도 볼 수 있는 문서
- 클래스 다이어그램: 실제 서버를 실행하지 않고 클래스만 보고 분석할 수 있는 문서(정적)
- 객체 다이어그램: 서버가 실제로 떠서 클라이언트가 동적으로 사용하는 객체들끼리 참조를 분석한 문서(동적)

# 회원 예제
- jUnit을 사용하면 좋은점
    - 기존의 콘솔창에 찍어서 눈으로 직접 검증하지 않고 테스트 코드를 통해 검증
    - 코드의 변경에 의해 놓칠 수 있는 에러를 테스트 코드를 통해서 빠르게 캐치할 수 있다.
    - 
![스크린샷 2021-12-22 오전 1 57 46](https://user-images.githubusercontent.com/66231761/146969045-5fa3251d-a4b2-47d7-b1f3-cf22288d2cad.png)

![스크린샷 2021-12-22 오전 1 58 12](https://user-images.githubusercontent.com/66231761/146969098-c7142eea-4c7f-4f8e-ac4b-03d0a54bdb8a.png)


# 회원 도메인 설계의 문제점

- 다른 저장소로 변경할 때 OCP 원칙을 잘 준수할까?
- DIP를 잘 지키고 있을까?
    - 해당 코드는 추상화에도 의존하고 구체화에도 의존한다.
    
    ![스크린샷 2021-12-22 오전 1 58 40](https://user-images.githubusercontent.com/66231761/146969153-c4623c54-f0e8-4877-b8a7-4632a94f5d01.png)


---

# 주문과 할인 도메인 설계

<img width="600" alt="스크린샷 2021-12-22 오전 1 59 20" src="https://user-images.githubusercontent.com/66231761/146969259-5b7f163a-b3ab-49d3-9bb7-0fe2083e8ed7.png">

1. 주문 생성: 클라이언트는 주문 서비스에 주문 생성을 요청
2. 회원 조회: 할인을 위한 회원 등급을 조회
3. 할인 적용: 주문 서비스는 회원 등급에 따른 할인 여부를 할인 정책에 위임함
4. 주문 결과 반환: 주문 서비스는 할인 결과를 포함한 주문 결과를 반환함

- 역할과 구현을 분리했기 때문에 회원 저장소나 할인 정책을 유연하게 변경할 수 있다.

### 주문 도메인 전체
<img width="600" alt="스크린샷 2021-12-22 오전 1 59 49" src="https://user-images.githubusercontent.com/66231761/146969330-c200f7dd-5f28-4ac0-b23f-b0e7092cfde1.png">

<img width="600" alt="스크린샷 2021-12-22 오전 2 00 16" src="https://user-images.githubusercontent.com/66231761/146969391-3d58fd46-45c9-4b93-86fe-36e0048835b3.png">

<img width="600" alt="스크린샷 2021-12-22 오전 2 00 25" src="https://user-images.githubusercontent.com/66231761/146969405-691982c9-a629-4631-aaac-c1729b3d411b.png">

<img width="600" alt="스크린샷 2021-12-22 오전 2 00 35" src="https://user-images.githubusercontent.com/66231761/146969428-9d521406-38ad-4f6b-a939-02dfa3e62c2a.png">

- ‘역할들의 협력 관계를 그대로 재사용할 수 있다.’

# 주문 예제

![스크린샷 2021-12-22 오전 2 01 42](https://user-images.githubusercontent.com/66231761/146969570-52168ca9-bcd7-48fc-84f0-21eeca0a8809.png)

# 정리

- 다형성을 활용하여 역할과 구현을 분리했다
- 과연 요구사항의 변경에 유연하게 대처할 수 있을까?

# 심화

- 동시성 오류
    - HashMap을 사용하면 여러 사용자가 동시에 접근할 때 동시성 에러가 발생할 수 있기 때문에 ConcurrentHashMap을 사용해야 한다.
- 자동완성과 동시에 콤마(;)찍는 단축키: command + shift + enter
- new Constructor();만 입력하고 대입할 변수와 변수명 자동 생성해주는 단축키: command + option + v
- 오류난 곳으로 바로 이동하기 단축키: F2
- 통합 테스트보다 단위 테스트가 훨씬 빠르다.
    - 단위 테스트: 스프링의 도움없이 순수 자바 코드로 검증하는 작은 단위의 테스트
