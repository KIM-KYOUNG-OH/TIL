# 23장 Set & Queue

# Set
- 순서가 없음
- 중복을 허용하지 않음
- 구현체
  - HashSet
  - TreeSet
    - red-black 타입으로 저장
  - LinkedHashSet

# HashSet
- 순서가 없음
- 중복 불허

# Load Factor
- (데이터 개수) / (저장 공간)
- 데이터 개수가 로드 팩터보다 커지면 저장 공간의 크기가 증가하고 해시 재정리 작업(rehash)을 함
- 로드 팩터가 커질수록 공간을 커지지만 탐색 시간 증가

# Queue
- FIFO

# LinkedList
- 각 노드는 바로 다음 노드의 주소값을 가짐
- 중간에 노드가 지속적으로 추가되거나 삭제되는 경우 성능이 유리
  - 데이터 지속적으로 추가/삭제되는 경우 ArrayList, Vector는 중간에 나머지 데이터의 위치를 하나씩 이동시켜줘야하는 오버헤드 발생

# ListIterator
- Iterator는 next()만 검색 가능하지만 ListIterator는 Previous()도 검색 가능

