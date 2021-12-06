# NestJS 프로젝트 구조
- nestjs/cli를 통해 생성된 NestJS 프로젝트 구조

![스크린샷 2021-12-06 오후 9 04 25](https://user-images.githubusercontent.com/66231761/144842969-44e4ff66-299c-4b59-8461-7ff84e025291.png)
- eslintrc.js
    - 개발자들이 특정한 규칙을 가지고 코드를 깔끔하게 짤 수있게 도와주는 라이브러리 타입스크립트를 쓰는 가이드 라 인 제시, 문법에 오류가 나면 알려주는 역할 등등
- prettierrc
    - 주로 코드 형식을 맞추는데 사용
    - 작은 따옴표(')를 사용할지 큰 따옴표(")를 사용할지, Indent 값을 2로 줄지, 4로 줄지 등
- nest-cli.json
    - nest 프로젝트 JSON 설정 파일
- tsconfig.json
    - 어떻게 타입스크립트를 컴파일할지 설정
- tsconfig.build.json
    - tsconfig.json의 연장선상 파일
    - build할 때 필요한 설정들
- package.json
    - 프로젝트 운영을 위한 모듈 저장
- src
    - main.ts
    - app.module.ts

# NestJS 로직 흐름
![스크린샷 2021-12-06 오후 9 05 23](https://user-images.githubusercontent.com/66231761/144843073-e88507aa-165f-4351-bf09-4f4bc52f22da.png)

# NestJS 모듈
![스크린샷 2021-12-06 오후 9 06 01](https://user-images.githubusercontent.com/66231761/144843139-e294e17b-248f-48f9-8f0f-895c5d2f2ac9.png)
- 모듈은 밀접하게 관련된 기능 집합이다. ex) 유저 모듈, 주문 모듈, 채팅 모듈...
- NestJS 애플리케이션에선 최소 하나 이상의 모듈이 존재함
- 모듈은 싱글 톤이므로 여러 모듈간에 쉽게 공급자의 동일한 인스턴스를 공유할 수 있다.
![스크린샷 2021-12-06 오후 9 06 44](https://user-images.githubusercontent.com/66231761/144843238-60382693-99b3-4f35-b06f-1c1102b5217c.png)

# Board Module 생성하기
- terminal에서 nestjs cli로 생성 가능
    - nest: using nestcli
    - g: generate
    - boards: name of module
    ![스크린샷 2021-12-06 오후 9 07 24](https://user-images.githubusercontent.com/66231761/144843361-559274d8-3ebb-4c3a-8d4b-d9ddcd9c5dbf.png)

# Controller란?
- Controller는 클라이언트의 요청을 처리하고 알맞은 응답을 반환한다.
- @Controller 데코레이션으로 정의한다.

# Handler란?
- Hander는 Controller 내의 메서드를 의미한다.
- @Get, @Post, @Delete 등의 데코레이터로 정의한다.

# Board Controller 생성하기
- nestjs cli로 생성 가능
    - --no-spec: 테스트 코드를 생성하지 않겠다는 뜻
    ![스크린샷 2021-12-06 오후 9 08 40](https://user-images.githubusercontent.com/66231761/144843557-199d3240-aa21-4eb0-9e15-6d741445da3f.png)

# NestJS의 Provider란?
- Nest의 클래스는 Provider로 취급된다.
- Provider의 주요 아이디어는 DI(의존성 주입)으로, 객체들간 관계를 맺는 기능은 Nest 런타임에 위임된다.

# Service란?
- Service는 Controller에서 데이터의 유효성 체크나 데이터베이스에 아이템을 생성하는 등의 작업을 수행한다.
- @Injectable 데코레이터로 감싸서 정의한다.

# BoardService 생성하기
- nestjs cli 로 생성  
  ![스크린샷 2021-12-06 오후 9 12 24](https://user-images.githubusercontent.com/66231761/144843989-33d0403c-3389-4290-a933-c758fba9b55b.png)
