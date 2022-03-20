# 19장 JVM

# Java의 철학
- 'Write Once, Run Anywhere'
- 플랙폼에 구애받지 않고 실행가능한 개발 언어

# JRE
- JDK안에 포함
- 자바를 실행할 수 있는 환경의 집합

# 자바의 특징
1. 단순하고 객체지향적
2. 견고하고 보안상 안전
3. 플랫폼 중립적
4. 높은 성능
5. 인터프리트 언어, 쓰레드 제공

# JIT 컴파일러
- 바이트 코드를 동적으로 기계어로 변환하는 역할
- JDK 1.2 버전에서 추가

# NIO
- 버퍼를 이용해 non-blocking으로 IO 처리
- JDK 1.4 버전에 추가

# 제네릭
- 안전하게 컬렉션 데이터 처리
- JDK 5 버전에 추가

# 람다 표현식
- JDK 8 버전에 추가

# HotSpot JVM
### HopSpot Client Compiler
- CPU 코어가 하나뿐인 사용자를 위한 JVM
- 애플리케이션 시작 시간을 빠르게하고 적은 메모리를 점유하도록 하는게 목적

### HopSpot Server Compiler
- CPU 코어가 많은 컴퓨터
- 애플리케이션 수행속도를 높이기 위한 목적

# JVM
- 자바 프로그램이 수행되는 프로세스

# GC
- Garbage 객체를 힙 메모리에서 해제하는 역할
- Old/Young/Perm
- Eden/Survivor1/Survivor2
- Minor GC(Young GC)/Major GC(Full GC)

# GC의 종류
1. Serial GC
2. Parallel Young Generation Collector
3. Parallel Old Generation Collector
4. Concurrent Mark & Sweep Collector(CMS)
5. G1(Garbage First)


