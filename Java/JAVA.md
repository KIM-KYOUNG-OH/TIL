# Java

#  java.util  
##  List   
-  동적으로 자료형의 갯수가 가변하는 상황일때 사용  
-  인터페이스 클래스  
-  java.util.List
###  1. add() 메서드  
~~~
.add(133)  //배열에 133 값 추가
.add(null)  //null값도 add가능
.add(0, 133)   //첫번째 위치에 133 삽입
~~~
###  2. get() 메서드  
~~~
.get(1)  //두번째 value 추출
~~~
###  3. size() 메서드
~~~
.size()  //배열에 담긴 index의 갯수 리턴
~~~
###  4. contains() 메서드
~~~
.contains(142)  //리스트안에 142가 있으면 true리턴
~~~
###  7.  indexOf() 메서드
~~~
.indexOf(1)  //리스트안에 앞에서부터 1이 처음발견되는 index를 리턴, 찾지못하면 -1을 리턴
~~~
###  5. remove() 메서드
~~~
.remove(0)  //첫번째 항목을 삭제하고 삭제된 항목을 리턴  
.remove("129")  //리스트안에 129를 상제하고 true를 리턴
~~~
###  6.  clear() 메서드
~~~
.clear()  //모든 값 제거  
~~~
##  ArrayList  
-  List 인터페이스를 상속받은 클래스
-  ArrayList<> list = new ArrayList<>();  //타입을 Wrapper클래스로 선언  
  
#  Java.lang   
  
## Wrapper Class  
-  기본 자료타입을 객체로 다루기 위해 사용하는 클래스
-  포장객체라고 함  
~~~
Integer num = new Integer(17);  //박싱
int n = num.intValue()  //언박싱   
~~~  
-  AutoBoxing / AutoUnBoxing  
~~~  
Integer num = 17;  //AutoBoxing
int n = num;  //AutoUnBoxing
~~~  
###  parse + 기본 타입명() 메서드
-  문자열을 기본 타입형으로 변환
###  equals() 메서드  
-  ==연산자는 객체의 참조주소를 비교하기 때문에 래퍼 객체 내부의 값을 위해서 사용  

##  Number 클래스

###  toString()  
~~~
Interger.toString(10);  //정수형을 문자열로 변환
~~~  
###  valueOf()  vs  parseInt  
-  valueOf() : Integer객체로 변환  
-  parseInt() : int형 데이터 리턴  
-  valueOf함수에 내부적으로 parseInt사용
###  charAt(index)  
-  index번째 문자가 출력  
###  subString(index,index)  
-  시작부터과 끝까지의 문자열을 출력  

### 문자열을 Date타입으로 변환하는 방법  
  ``` java    
  String birth = "1999-09-09";
  SimpleDateFormat sdf = new SimpleDateFormat("yyyy-MM-dd");
  Date birthday = null;
  try {
    birthday = sdf.parse(birth);
  } catch (ParseException e1) {
    e1.printStackTrace();
  }  
  ```  

### 날짜 출력
    Date 클래스(deprecated)보다 Calendar클래스가 권장됨
