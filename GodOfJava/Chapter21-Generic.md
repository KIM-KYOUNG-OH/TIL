# 21장 Generic

# Generic을 쓰는 이유?
- 런타임 에러를 컴파일 시점에 발생시킬 수 있다
- 객체에 담긴 데이터를 꺼낼 때 매번 형변환해야하는 비용을 사전에 없애줌

# 제네릭 타입 명명 규칙
- E: element, Collection에서 주로 사용
- K: 키
- N: 숫자
- T: 타입
- V: 값
- S, U, V: 두, 세, 네번째 타입

# 와일드카드
- '?'
- ?를 사용하면 어떤 타입이 제네릭 타입이 되더라도 상관없다
- 클래스 내부에선 어떤 타입이 입력될 지 모르기 때문에 Object로 처리해야함
```java
public void wildcardStringMethod(WildcardGeneric<?> c) {
    Object value = c.getWildcard();
    if(value instanceof String) {
        System.out.println(value);
    }
}
```

```java
public class WildcardGeneric<W> {
    W wildcard;
    
    public void setWildcard(W wildcard) {
        this.wildcard = wildcard;
    }
    
    public W getWildcard() {
        return wildcard;
    }
}
```

- wildcard는 특정 타입으로 지정할 수 없기 때문에 메서드의 매개변수로만 사용하자

```java
WildcardGeneric<?> wildcard = new WildcardGeneric<String>();  // 컴파일에러 발생
```

# 제네릭 타입 경계(Bounded)
- ? extends 타입, T extends 타입
- 와일드카드로 사용하는 타입을 제한할 수 있다

```java
public void method(Sample<? extends Car> c) {}  // method는 Car를 상속받는 모든 클래스를 사용가능하다는 의미
```

# 매개변수로 사용된 객체에 값을 추가하는 방법
- ?를 사용하기보다 명시적으로 메소드 선언시 제네릭 타입을 지정해주면 더 코드가 견고해짐
```java
public <T> void genericMethod(WildcardGeneric<T> c, T addValue) {
    c.setWildcard(addValue);
    T.value = c.getWildcard();
}
```

```java
public void callGenericMethod() {
    WildcardGeneric<String> wildcard = new WildcardGeneric<>();
    genericMethod(wildcard, "Data");
}
```

# 두 개 이상의 제네릭 타입 선언하기
- 콤마로 구분
```java
public <S, T extends Car> void multiGenericMethod(WildcardGeneric<T> c, T addValue, S another) {}
```
