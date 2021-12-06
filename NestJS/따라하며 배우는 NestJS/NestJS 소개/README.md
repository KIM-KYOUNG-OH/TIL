# NestJS란?

- NestJS는 Node.js 서버 측 애플리케이션을 구축하기 위한 프레임워크이다.
- 프로그레시브 JavaScript를 사용하고 TypeScript, OOP(Object Oriented Programming), FP(Functional Programming), FRP(Functional Reactive Programming)을 지원한다.

# NestJS의 철학

- NestJS는 테스트 가능하고 확장 가능하며 느슨하게 결합되고 유지보수가 유리한 애플리케이션 아키텍처를 제공한다.
- 해당 아키텍처는 Angular에서 크게 영감을 받았다.

# References
- [https://docs.nestjs.com/](https://docs.nestjs.com/)
- [https://github.com/jaewonhimnae/nestjs-board-app](https://github.com/jaewonhimnae/nestjs-board-app)

# Homebrew로 Node.js 설치

```bash
> brew install node

> brew info node

> brew unlink node && brew link node

> node -v
```

# NestJS CLI 설치

- 프로젝트 생성

```bash
> mkdir nestjs-board-app

> npm init
```

- NestJS CLI를 이용하면 간단히 프로젝트를 시작할 수 있다.
- i: install의 줄임말
- -g: global 설정

```bash
> npm i -g @nestjs/cli
```

- 해당 명령어를 실행하면 새프로젝트 디렉토리가 생성되고 초기 핵심 Nest 파일 및 지원 모듈로 디렉토리가 채워져 프로젝트 기본 구조가 생성된다.

```bash
> nest new project-name
```
