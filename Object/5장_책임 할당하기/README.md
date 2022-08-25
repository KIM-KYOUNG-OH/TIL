# 5장 책임 할당하기

# 책임을 어떻게 할당할 것인가?

1. 책임 주도 설계
    - 책임을 먼저 결정하고 이를 수행할 객체를 나중에 결정한다
2. Information Expert Pattern
    - 메시지를 수신할 객체는 해당 책임을 수행시 가장 많은 정보를 가진 객체로 선택한다

# 캡슐화말고 코드 리팩토링 포인트가 있을까?

1. 메소드를 호출하고 속성 초기화가 일부만 이루어질 경우 클래스 분리를 고려해봐야함
2. 메서드 내부 구현에 멤버 변수의 일부만 사용될 경우 응집도가 낮을 클래스 분리를 고려해봐야함
3. 타입 변수 if문 이 너무 많고 조건문을 통한 타입분기 너무 많으면 클래스 분리를 고려해봐야함

```java
public class Movie{
		private List<DiscountCondition> discountCondtions;  // 추상 인터페이스를 공개

		public Money calculateMovieFee(Screening screening) {
				if(isDiscountable(screening)) {
						return fee.minus(calculateDiscountAmount());
				}

				return fee;
		}

		private boolean isDiscountable(Screening screening) {
				return discountCondtions.stream().anyMatch(condition -> condition.isSatisfiedBy(screening));
		}
}
```

# Polymorphism Pattern

- 조건문을 통한 타입 분기는 변경에 취약하므로 추상화된 인터페이스를 두고 다형적으로 메시지를 수신할 객체를 선택하는 방식
- 주의할 점
    - 역할에 책임이 여러개일경우나 중복 조건이 허용될경우 ‘…’ 연산자로 파라미터를 여러 개 주입할 수 있도록 허용해야한다.
    ```java
    public class Movie {
        private List<DiscountCondition> discountConditions;

        public Movie(DiscountCondition... discountConditions) {
            this.discountConditions = Arrays.asList(discountConditions);
        }
    }
    ```
