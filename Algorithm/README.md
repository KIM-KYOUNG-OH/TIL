# 알고리즘 팁
1. for data not in list
2. 딕셔너리타입 데이터삽입/삭제
3. 딕셔너리타입 keys(), values(), items(), get()
4. max, min
5. sort(), sorted()
6. 데이터의 범위가 너무 크다면 이진탐색트리를 고려해보자
7. 트리순회(전위순회, 중위순회, 후위순회)
8. 최소힙은 자동으로 작은값이 상위노드로 들어감(heapq.heappush())
9. if list변수 : 'list내부에 element가 존재하면'
10. 위상정렬
11. dfs/bfs
12. 백트래킹 : 모든 경우의 수를 구한다 (dfs로 구하는게 편함-재귀함수,stack)
13. 문제 - 추상화 - 절차적 사고 - 구현
14. 대표유형
    1) 구현: 파싱, 해싱, 정렬, 시뮬레이션
    2) 탐색: BFS/DFS, 완전탐색(백트래킹)
    3) 구조: 큐, 스택, 힙
    4) 알고리즘: greedy, DP, 이분탐색...
15. chr(): 아스키코드번호 to 문자, ord(): 문자 to 아스키코드번호 
16. shortcircuit
17. container 자료구조 : list, dictionary, set, tuple
    1) list
        - literable 자료형
        - list comprehension
        - sort/sorted
        - min/max
        - 인덱스 슬라이싱
    2) tuple
        - map()
        - a,b = b,a
    3) dictionary
        - collections.Counter
        - keys()/values()
    4) set
        - set()
        - 시간복잡도가 크다
18. iterable 객체란 반복 가능한 객체 immutable객체란 불변객체
19. enumerate() : for반복문에서 range자리에 쓰임, list자료형의 index와 value값을 튜플형태로 반환
20. 함수를 정의할때 매개변수,리턴값을 먼저 적기
21. 이분탐색
22. 리스트의 element들이 모두 같은지 확인하는 방법: len(set)을 사용
23. if 0 : false, if 1 : true
24. set은 element의 중복된 횟수를 체크할때 사용할 수 있다
25. index(), find()
26. split()은 리스트값을 반환한다
27. list comprehension은 괄호로 필히 감싸줘야한다
28. 모듈(deque, heapq, bisect, math)
29. 문자열 거꾸로 출력하기 s[::-1]
30. 동서남북 방향벡터 dx, dy = [0,1,0,-1], [1,0,-1,0]
31. 문자열을 list()로 감싸면 char단위로 나눔(split('') : 작동안됨)
32. 대부분의 코딩테스트는 BFS/DFS+재귀만 잘해도 풀 수 있다
33. 탐색문제) BFS: Queue, DFS: Stack(재귀함수)
34. 대표적인 탐색문제: 
        - 부분상태탐색(위치 이동, 수)
        - 전체상태탐색(전체map)
    1) 구현
        - Flood Fill
        - 트리 순회
    2) 알고리즘 지식
        - 위상정렬(Topological Sort)
        - 최소신장트리(MST)
        - 최단 거리
35. 'type' object is not subscriptable 오류 :
해당 변수의 타입선언을 제대로해줘야함
36. DFS
- 재귀함수, stack을 이용
37. BFS
- dequeue 이용
38. Count (builtin class) .most_common() : 해당리스트에 element를 자주 빈출된 순서의 2차원 배열로 반환 
39. lambda로 처리식의 priority를 지정
40. sys.stdin.readline()이 input()보다 빠르다
41. 파일명에 괄호"(",")"가 포함되면 에러남  
42. 나누기('/')하면 타입이 double형으로 바뀌기 때문에 주의  
43. count = 0 에서 시작할 수 있지만 count = len 에서부터 하나씩 빼면서 개수를 셀 수도 있다.
44. 형변환이 시간 복잡도에 영향을 줄 수 있다.
45. 줄바꿈하지 않고 뒤에 공백 붙여서 출력하기  
```python
v = test
print(v, end=' ')
```
46. 2차원 배열은 [[]*int]*int 이런식으로 선언하면 절대 안됨(row 복사됨)  

