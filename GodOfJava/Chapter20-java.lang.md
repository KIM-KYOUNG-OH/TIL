# 20장 java.lang

# java.lang
- import 안해도 동작

# OutOfMemoryError(OOME)
- 메모리 부족시 발생하는 에러

# StackOverFlowError
호출된 메소드 깊이가 너무 깊을 때 발생하는 에러

# Wrapper 클래스
- 숫자를 처리하는 기본 자료형을 감싼 클래스
- Number 추상 클래스를 extends 함
- JVM에서 Auto Casting하므로 기본 자료형처럼 사용가능

# Wrapper 클래스를 왜 쓸까?
- 매개변수를 참조 자료형으로만 받는 메소드를 처리하기 위해
- 제네릭
- MIN_VALUE와 MAX_VALUE 같은 Wrapper 클래스에서 지원하는 상수값을 사용하기 위해
- 문자열과 숫자간 casting
- 2, 8, 10, 16 진수 변환을 쉽게 하기 위해(toBinaryString(), toHexString()...)

# System 클래스
- static PrintStream err 에러처리 및 오류 출력
- static InputStream in 입력값 처리
- static OutputStream out 출력값 처리

# System.out.println()
- System.out.println()은 매개변수의 toString()을 호출하는게 아니라 String.valueOf()를 호출한다.

```java
Object obj = null;
System.out.println(obj);  // null이 출력됨, NullPointerException 발생X
System.out.println(obj.toString());  //  NullPointerException 발생O
```

# 문자열 +연산
- JVM은 내부적으로 StringBuilder로 변환하여 처리함
- 아래의 두 코드는 같다
```java
obj + "is object's value"
```

```java
new StringBuilder().append(obj).append("is object's value")
```
