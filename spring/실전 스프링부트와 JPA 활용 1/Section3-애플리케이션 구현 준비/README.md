# 애플리케이션 아키텍처

![스크린샷 2022-01-11 오전 1 30 17](https://user-images.githubusercontent.com/66231761/148801926-13513b06-a5c0-4c38-bad9-869710a2b6fa.png)

- 계층형 구조
    - controller, web
    - service
    - repository
    - domain
- 패키지 구조
    - jpabook.jpashop
        - domain
        - exception
        - repository
        - service
        - web

# 개발 순서

- 서비스, 레포지토리 계층을 개발하고 테스트 케이스를 작성해서 검증, 마지막에 웹 계층 적용
