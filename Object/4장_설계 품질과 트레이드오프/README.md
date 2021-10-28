# Chapter 4 설계 품질과 트레이드오프

## 3장 정리
객체지향 설계의 핵심은 협력, 책임, 역할이다.  
협력은 애플리케이션의 기능을 구현하기 위해 메시지를 주고받는 객체들 사이의 상호작용이다.  
책임은 객체가 다른 객체와 협력하기 위해 수행하는 행동이다.  
역할은 대체 가능한 책임의 집합이다.  
그 중에서 가장 중요한 책임이 객체지향 애플리케이션 전체의 품질을 결정한다.  

## 객체지향 설계  
객체지향 설계란 올바른 객체에게 올바른 책임을 할당하면서 낮은 결합도와 높은 응집도를 가진 구조를 창조하는 활동이다.  
객체지향 설계란 협력하는 객체들의 공동체를 구축하는 것이다.  
객체지향 설계란 협력이라는 문맥안에서 필요한 책임을 결정하고 이를 수행할 역할로 적절한 객체를 결정하는 것이다.  
객체지향의 원칙은 객체의 상태가 아니라 객체의 행동(책임)에 초점을 맞추는 것이다.  
객체끼리 협력하는 '객체 외부'에 초점 맞춰져 있다.  
설계가 '어떤 기능을 구현하기 위해 객체의 책임이 무엇인가'라고 묻는 것으로 시작한다.  
객체의 상태와 행동을 하나의 단위로 캡슐화하는 객체지향 패러다임을 따른다.  
객체를 협력에 참여하는 구성원으로 본다.  
캡슐화를 최우선 반영하고 응집도가 높고 결합도가 낮다.  

## 데이터 중심 설계
객체의 행동(책임)이 아닌 객체의 상태에 초점을 맞춰 설계한다.  
협력이라는 문맥에 대한 고민 없이 객체가 관리할 세부 데이터인 '객체 내부'에 초점이 맞춰져 있다.  
설계가 '객체에 포함해야할 데이터는 무엇인가'라고 묻는 것으로 시작한다.  
데이터와 기능을 분리하는 절차적 프로그래밍 방식을 따른다.  
접근자와 수정자를 공개하여 캡슐화를 위반한다.  
객체를 단순히 데이터 제공자로 본다.  
캡슐화를 위반하고 응집도가 낮고 결합도가 높다.  

## 설계 품질의 척도  
캡슐화, 응집도, 결합도  

## 캡슐화  
캡슐화란 변경될 수 있는 어떤 것이라도 감추는 것을 의미한다.  
캡슐화를 통해서 에러확산을 막고 변경에 유리하도록 한다.  
내부 구현의 변경으로 인해 외부 객체가 영향을 받는다면 캡슐화를 위반한 것이다.  
'외부로부터 객체의 내부 속성을 감추는 것'이나 '외부에서 인터페이스에만 의존하도록 하는 것', '객체가 스스로를 책임지도록 하는 것' 캡슐화의 한 종류일 뿐이다.  
캡슐화를 설계의 첫 번째 목표로 삼아야 한다.  

'캡슐화란 필드를 private으로 선언해서 외부로부터 접근을 막는 것이다' (x)  

## 응집도
응집도란 변경이 발생할 때 모듈 내부에서 발생하는 변경의 정도를 의미한다.  
객체지향의 관점에서 객체 또는 클래스에 얼마나 관련 높은 책임들을 할당했는지를 나타낸다.  

## 결합도
결합도는 변경이 발생할 때 외부 다른 모듈의 변경을 요구하는 정도를 의미한다.  
객체지향의 관점에서 객체 또는 클래스가 협력에 필요한 적절한 수준의 관계만 유지하고 있는지를 나타낸다.  

## 유지보수성이란?
아무런 주저 없이 코드를 변경할 수 있는 능력  

## 좋은 설계란?
오늘의 기능을 수행하면서 내일의 변경을 수용할 수 있는 설계이다.  

## 응집도와 결합도
캡슐화의 정도가 응집도와 결합도에 영향을 미친다.  
하나의 요구사항 추가나 변경을 반영하기 위해 오직 하나의 모듈만 수정하면 된다면, 응집도가 높고 결합도가 낮다고 할 수 있다.  
반대로 하나의 모듈을 변경시 이에 의존된 다수의 모듈이 함께 변경돼야 한다면, 응집도가 낮고 결합도가 높은 것이다.  

## 접근자와 수정자
```java
public class Movie {
  private Money fee;
  
  public Money getFee() {
    return fee; 
  }
  
  public void setFee(Money fee) {
    this.fee = fee; 
  }
}
```
위의 코드는 객체의 상태를 private으로 선언해서 캡슐화의 원칙을 지키고 있는 것처럼 보이지만 사실은 아니다.  
책임(오퍼레이션)은 협력이라는 문맥을 고려할 때만 얻을 수 있다는 것을 기억하자.  
위의 코드는 추측에 의한 설계 전략(design-by-guessing strategy)에 불과하다.  

set 메서드를 사용하는 것은 객체가 스스로의 상태를 변경하는 자율적인 객체의 성질을 위반한다.  
get 메서드를 사용하는 것은 객체의 상태를 private에서 public으로 변경하는 것과 거의 동일하다.  
get, set 메서드를 선언하는 것 자체만으로 객체에 어떤 필드가 있는지 노출하는 것과 다름 없다.  
비록 set 메서드는 없더라도 get 메서드를 포함한 코드가 존재한다면, 객체 내부 속성 변경시 영향이 퍼지는 파급 효과(Ripple Effect)가 발생한다.  

-> 접근자를 마구 남발하기보다 객체의 행동에 초점을 맞추어 필요에 맞게 접근자를 정의하자  
```java
boolean is~~~() {}

void check~~~() {}
```
기존의 get 메서드로 호출하여 service단에서 로직을 작성하기보다 위 코드처럼 is~, check~ 메서드를 작성하는 방식으로 객체에게 책임을 할당하자  

## 단일 책임 원칙(Single Responsibility Principle, SRP)
클래스는 단 한 가지의 변경 이유만 가져야 한다.  

## 메서드 파라미터, 리턴값을 통한 정보 노출
```java
public class DiscountCondition {
  private DiscountConditionType type;
  private int sequence;
  private DayOfWeek dayOfWeek;
  private LocalTime startTime;
  private LocalTime endTime;
  
  public DiscountConditionType getType() {...}
  
  public boolean isDiscountable(DayOfWeek dayOfWeek, LocalTime time) {...}
  
  public boolean isDiscountable(int sequence) {...}
}
```
위의 코드에서 getType()메서드는 리턴값으로 DiscountConditionType을 반환한다.  
이는 DiscountCondition 내부에 DiscountConditionType을 속성으로 가진다는 것을 의미하므로 캡슐화를 위반한다.  
isDiscountable는 파라미터값으로 DayOfWeek, LocalTime, Sequence을 요구한다.  
마찬가지로 객체 내부 속성을 노출시키는 캡슐화 위반이다.  

## 과도한 책임 부여로 인한 정보 노출
```java
public class Movie {
  private String title;
  private Duration runningTime;
  private Money fee;
  private List<DiscountCondition> discountConditions;
  
  private MovieType movieType;
  private Money discountAmount;
  private double discountPercent;
  
  public MovieType getMovieType() {...}
  public Money calculateAmountDiscountedFee() {...}  //금액할인기준
  public Money calculatePercentDiscountedFee() {...} //비율할인기준
  public Money calculateNoneDiscountedFee() {...}  //할인미적용
}
```
calculateAmountDiscountedFee, calculatePercentDiscountedFee, calculateNoneDiscountedFee 메서드로 인해 할인 조건의 종류를 노출시켰다.  
과도한 메서드 선언에 의해 객체 내부 구현을 노출할 수도 있다.  

## 데이터 중심 설계의 본질적인 문제 원인
1. 데이터 중심 설계는 너무 이른 시기에 데이터에 관해 결정하도록 강요한다.  
2. 데이터 중심 설계에서는 협력이라는 문맥을 무시하고 객체를 고립시킨 채 오퍼레이션을 결정한다.  

데이터를 먼저 결정하고 나중에 오퍼레이션을 결정하는 방식은 인터페이스에 데이터에 관한 정보를 고스란히 노출하게 된다.  
객체 세부 정보가 이미 결정된 상태에서 협력을 고민하므로, 이미 구현된 객체에 인터페이스를 억지로 끼워 맞출 수 밖에 없다.  
협력이 세부 구현에 종속돼 있고 객체 내부 구현이 변경됐을 때 협력하는 객체가 모두 영향을 받을 수 밖에 없다.  
