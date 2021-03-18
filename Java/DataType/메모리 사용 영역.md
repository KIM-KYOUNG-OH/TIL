# 1. 데이터 타입 분류  
  - ### 원시 타입(primitive type)  
  - ### 참조 타입(reference type)  

  - ## 1-1. 원시 타입  
    - 정수, 실수, 문자, 논리 타입  
    - byte, char, short, int, long, float, double, boolean  
    - 실제 값을 변수에 저장한다  
    - 스택(stack)영역에 생성된다

  - ## 1-2. 참조 타입  
    - 객체의 번지를 참조하는 타입  
    - 배열, 열거, 클래스, 인터페이스 타입  
    - 메모리의 번지 값을 변수에 저장한다  
    - 힙(Heap)영역에 객체가 생성된다  

# 2. 메모리 사용 영역  
  - java.exe로 JVM이 시작되면 JVM은 운영체제에서 할당받은 메모리 영역(Runtime Data Area)을 세부 영역으로 구분해서 사용한다  
  - 메소드 영역, 힙영역, JVM 스택영역  

  - ## 2-1. Method 영역  
    - 코드에 사용되는 클래스들을 런타임 상수풀(runtime constant pool), 필드(field) 데이터, 메소드(method) 데이터, 메소드 코드, 생성자(constructor) 코드 등을 분류해서 저장한다  
    - JVM이 시작할 때 생성되고 모든 스레드가 공유하는 영역이다  
 
  - ## 2-2. Heap 영역  
    - 객체와 배열이 생성되는 영역이다  
    - 참조하는 변수가 없다면 JVM이 Garbage Collector를 실행시켜 쓰레기 객체로 취급하고 자동으로 제거한다  

  - ## 2-3. Stack 영역  
    - 각 스레드마다 하나씩 존재하며 스레드가 시작될 때 할당된다  
    - JVM 스택은 메소드를 호출할 때마다 Frame을 push하고 메소드가 종료되면 해당 Frame을 pop한다  
    - 프레임 내부에 로컬 변수 스택이 있는데 최초로 변수에 값이 저장될 때 push되고 블록을 벗어나면 pop된다  

  - ## String 타입  
    ```java
    String name1 = "홍길동"; //문자열 리터럴 방식(코드내에서 값을 직접 입력)
    String name2 = "홍길동";
    String name3 = new String("홍길동"); //new 연산자 방식
    ```
    - name1과 name2는 동일한 문자열 리터럴로 생성된 객체를 참조하기 때문에 name1 == name2의 결과가 true가 나온다  
    - name3는 new 연산자로 객체를 별도로 생성했기 때문에 name1 == name3는 false가 나온다  
    </br>  
    
    ```java
    String hobby = "여행";
    hobby = null;
    ```
    - hobby 변수가 참조하는 객체가 없다는 뜻이다  
    - 참조를 잃은 String 객체는 Garbage Collector에 의해 자동 제거된다  
    
    
