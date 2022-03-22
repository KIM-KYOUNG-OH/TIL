# Java HashMap은 어떻게 동작하는가?

# HashMap이란?

- key에 대한 해시 값을 사용하여 값을 저장하고 조회하며, 키-값 쌍의 개수에 따라 동적으로 크기가 증가하는 associate array

# HashMap vs HashTable

- HashMap은 보조 해시 함수를 사용하기 대문에 HashTable에 비해 해시 충돌(Hash Collision)이 적다

# HashMap이 bucket을 사용하는 이유

- HashMap은 hashCode() 반환 값을 key로 사용하는데 해당 자료형은 int이므로 경우의 수는 2^32이다
- HashMap에 저장할 자료의 수가 2^32를 넘을 수 있다
- 메모리 절약을 위해 해시 함수의 표현 정수 범위 보다 작은 원소 배열을 사용하고 해시 코드의 나머지 값을 해시 버킷 인덱스 값으로 사용함

```java
int index = X.hashCode() % M;
```

# 해시 충돌 회피 방법

![image](https://user-images.githubusercontent.com/66231761/159542829-4e87db33-9e2b-4193-b894-634b0eca1e1e.png)

1. Open Addressing
2. Separate Chaining

# Open Addressing

- 데이터를 삽입하려는 해시 버킷이 이미 사용중인 경우 다른 해시 버킷에 해당 데이터를 삽입하는 방식
- 데이터를 저장/조회할 해시 버킷을 찾을 때는 Linear Probing, Quadratic Probing을 사용
- O(M)이지만 연속된 공간에 저장하므로 데이터가 적다면 캐시 효율이 높아 Separate Chaining보다 성능이 좋다
- 반대로 데이터가 많다면 캐시 적중률이 낮아짐
- 빈번한 데이터 삭제 처리시 효율이 떨어짐

# Separate Chaining

- 버킷 배열의 인자에 링크드 리스트를 다시 연결한 구조
- O(M)
- Java HashMap이 사용하는 방식
- 보조 해시 함수로 해시 충돌이 잘 발생하지 않도록 조정할 수 있다
- Java 8 HashMap은 Entry 대신 Node 클래스를 사용하고 Entry 클래스와 내용은 같지만 하위 클래스로 TreeNode를 가진다는 차이가 있다

# 보조 해시 함수

- 데이터가 일부 버킷에만 집중되지 않고 균등 분포되도록 함
- 키-값 데이터 개수가 일정 이상일 때 링크드 리스트 대신 트리를 사용한다
- 같은 값이 빈번히 삽입/삭제되는 경우에 불필요하게 링크드 리스트와 트리로 빈번히 변경되므로 링크드리스트로 변경되는 데이터 개수를 6, 트리로 변경되는 데이터 개수를 8개로 간격을 둔다

# TreeNode

- Red-Black Tree 사용

# 해시 버킷 동적 확장

- 해시 충돌을 낮추기 위해 키-값 데이터 개수가 일정 개수 이상이 되면 해시 버킷의 개수를 두 배로 늘린다
- 해시 버킷 개수 Default값은 16
- 버킷의 최대 개수는 2^30
- 두 배로 증가할 때 모든 데이터를 새로운 Separate Chaining으로 구성해야하는 오버 헤드가 존재함
- 두 배로 확장하는 임계점은 load factor(0.75) * 현재 해시 버킷 개수

# 보조 해시 함수

- 해시 버킷이 항상 두 배씩 동적으로 증가하기 때문에 index = X.hashCode() % M에서 M이 2의 승수 형태가 되기 때문에 해시 충돌이 잦아진다
- M 값이 소수가 아닌 이상 index 값 분포가 균등할 수 없으므로 보조 해시 함수를 이용해서 키의 해시 값을 변형하여 해시 충돌 가능성을 줄인다

# Reference

- [https://d2.naver.com/helloworld/831311](https://d2.naver.com/helloworld/831311)
