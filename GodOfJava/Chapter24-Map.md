# 24장 Map

# Map
- key-value로 이루어짐
- 구현체
  - HashMap
  - TreeMap
  - LinkedHashMap
  - HashTable

# HashMap vs HashTable
- HashMap
  - key값에 null 저장 가능
  - Not Thread-Safe
- HashTable
  - key값에 null 저장 불가
  - Thread-Safe

# HashMap의 Key를 클래스 타입으로 넣을 경우
- HashCode()와 equals()를 정의해야함

# java map bucket
- HashMap에 객체가 들어가면 hashcode() 결과값에 따른 bucket이라는 list형태 바구니가 만들어짐
- hashCode()가 동일하면 한 bucket에 여러 값이 들어갈 수 있다
- get()호출시 hashCode()로 먼저 탐색하고 bucket안에 값이 여러개일 경우 equals()로 다시 탐색함

# HashMap안에 데이터 출력
- HashMap은 정렬을 지원하지 않으므로 출력 순서를 보장하진 못함
1. keySet() 사용
  ```java
  Set<String> keySet = map.keySet();
  for(String key: keySet) {
      System.out.println(key + "=" + map.get(key));
  }
  ```
2. values() 사용
  ```java
  Collection<String> values = map.values();
  for(String value: values) {
      System.out.println(value);
  }
  ```
3. entrySet() 사용
  ```java
  Set<Map.Entry<String, String>> entries = map.entrySet();
  for(Map.Entry<String, String> entry: entries) {
      System.out.println(entry.getKey() + "=" + entry.getValue());
  }
  ```
  
# TreeMap
- 정렬을 지원하는 Map 구현체
- 숫자 > 알파벳 대문자 > 알파벳 소문자 > 한글 순으로 자동 정렬
- 정렬해서 저장하기 때문에 HashMap보다 느림

# Properties
- XML이나 파일의 속성을 읽거나 저장하는 메서드 제공
- IO 같은 시스템 상수 제공

```java
Properties prop = System.getProperties();
Set<Object> keySet = prop.keySet();
for(Object obj: keySet) {
    System.out.println(obj + "=" + prop.get(obj));
}
```
