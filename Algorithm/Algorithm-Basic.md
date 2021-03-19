# <알고리즘 원리>  
## 1. 그리디 알고리즘(탐욕법)   
- 문제풀이 과정에서 순간순간마다 최적이라고 생각되는 방식만을 선택하여 해답에 도달하는 방법이다  
- 사전에 외우고 있지 않아도 풀 수 있는 가능성이 높은 문제 유형이다
- 다익스트라 알고리즘은 그리디 알고리즘으로 분류되지만 암기가 필요하다 
- 만약 오랜시간 그리디 알고리즘으로 해결책을 찾을 수 없다면, 다이나믹 프로그래밍이나 그래프 알고리즘 등의 풀이를 고려해보아야 한다  

## 2. 구현 알고리즘  
- 머리속에 있는 알고리즘을 소스코드로 바꾸는 과정 
ex) 방향벡터 문제
- 완전탐색, 시뮬레이션, 구현 알고리즘은 서로 유사하다
- 완전탐색: 모든 경우의 수를 주저없이 다 계산하는 방법  
- 시뮬레이션: 문제에서 제시한 알고리즘을 한단계씩 차례대로 직접 수행

## 3. 탐색 알고리즘 DFS/BFS  
### 3-1. 그래프의 구조  
- 노드(node)와 간선(edge)로 이루어짐, 노드=정점(vertex)
- 인접행렬(Adjacency Matrix)  
```python
INF = 999999999 #  무한의 비용 선언
[[0, 7, 5], [7, 0, INF], [5, INF, 0]]
```
- 인접리스트(Adjacency List)  
```python
[[(1, 7), (2, 5)], [(0, 7)], [(0, 5)]]  #(노드, 거리)
```
### 3-2. DFS(Depth-First Search)  
- 깊이 우선탐색  
- 특정한 경로로 탐색하다가 특정한 상황에서 최대한 깊숙이 들어가서 노드를 방문한 후, 다시 돌아가 다른 경로로 탐색하는 알고리즘  
- 스택 자료구조 사용, 재귀함수를 이용했을때 알고리즘을 간결하게 표현 가능  
- 시간복잡도 : O(N)  

### 3-3. BFS(Breath-First Search)  
- 너비 우선 탐색  
- 가까운 노드부터 탐색하는 알고리즘  
- 큐 자료구조 사용  
- 시간복잡도 : O(N), 실제 수행 시간은 DFS보다 좋은편