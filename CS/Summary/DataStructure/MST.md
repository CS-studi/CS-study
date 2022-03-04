# 🌲 Minimum Spanning Tree/최소 신장 트리

### Minimum Spanning Tree란?
그래프 G의 spanning tree 중 가중치의 합이 최소인 spanning tree를 의미한다.

## MST의 특징
- 간선의 가중치의 합이 최소여야 한다.
- n개의 정점을 가지는 그래프에 대해 반드시 (n-1)개의 간선만을 사용해야 한다.
- 사이클이 포함되어서는 안된다.
`spanning tree` : 그래프 내에 있는 모든 정점을 연결하고 사이클이 없는 그래프를 의미한다.

### Kruskal 알고리즘
탐욕적인 방법으로 가중치를 선택하여 모든 정점을 최소 비용으로 연결하는 최적의 해답을 찾는 방식이다.
- 최소 비용의 간선으로 구성되며 사이클을 포함하지 않고 각 단계에서 사이클을 이루지 않는 최소 비용 간선 선택
- 간선 선택을 기반으로 하는 알고리즘이다.
- 이전 단계에서 만들어진 신장 트리와는 관련없이 무조건 최소 간선만을 선택
- union-find 알고리즘을 이용하며 시간 복잡도는 간선을 정렬하는 시간에 좌우된다.
- O(ElogE + E)

<br>

<details>
<summary>크루스칼 알고리즘 코드</summary>

```python

import sys

v, e = map(int, input().split())
# 부모 테이블 초기화
parent = [0] * (v+1)
for i in range(1, v+1):
    parent[i] = i

# 부모를 찾는 연산 
def find_parent(parent, x):
    if parent[x] != x:
        parent[x] = find_parent(parent, parent[x])
    return parent[x]

# union 연산
def union_parent(parent, a, b):
    a = find_parent(parent, a)
    b = find_parent(parent, b)
    if a < b:
        parent[b] = a
    else:
        parent[a] = b

# 간선 정보 담을 리스트와 최소 신장 트리 계산 변수 정의
edges = []
total_cost = 0

# 간선 정보 주어지고 비용을 기준으로 정렬
for _ in range(e):
    a, b, cost = map(int, input().split())
    edges.append((cost, a, b))

# 간선 정보 비용 기준으로 오름차순 정렬
edges.sort()

# 간선 정보 하나씩 확인하면서 크루스칼 알고리즘 수행
for i in range(e):
    cost, a, b = edges[i]
    # find 연산 후, 부모노드 다르면 사이클 발생 X으므로 union 연산 수행 -> 최소 신장 트리에 포함!
    if find_parent(parent, a) != find_parent(parent, b):
        union_parent(parent, a, b)
        total_cost += cost

print(total_cost)
```

</details>

<br>
<br>

### Prim 알고리즘
시작 정점에서 부터 출발하여 신장트리 집합을 단계적으로 확장해나가는 방법이다.

- 정점 선택을 기반으로 한다.
- 이전 단계에서 만드렁진 신장트리를 점점 확장하는 방식이다. (크루스칼은 이전 신장트리와는 연관이 없음.)
- 시간 복잡도에 가장 큰 영향을 미치는 것은 가중치가 가장 작은 정점을 찾아내는 것과 인접한 정점의 탐색이다.
- 모든 노드에 대해 탐색 O(V)이고 우선 순위큐 사용으로 O(logV) 최종 시간 복잡도는 O(ElogV)이다.

<details>
<summary>프림 알고리즘 코드</summary>

```python
import heapq
import collections
import sys
sys.setrecursionlimit(10**6)
input = sys.stdin.readline

n, m = map(int,input().split()) # 노드 수, 간선 수
graph = collections.defaultdict(list) # 빈 그래프 생성
visited = [0] * (n+1) # 노드의 방문 정보 초기화

# 무방향 그래프 생성
for i in range(m): # 간성 정보 입력 받기
    u, v, weight = map(int,input().split())
    graph[u].append([weight, u, v])
    graph[v].append([weight, v, u])


# 프림 알고리즘
def prim(graph, start_node):
    visited[start_node] = 1 # 방문 갱신
    candidate = graph[start_node] # 인접 간선 추출
    heapq.heapify(candidate) # 우선순위 큐 생성
    mst = [] # mst
    total_weight = 0 # 전체 가중치

    while candidate:
        weight, u, v = heapq.heappop(candidate) # 가중치가 가장 적은 간선 추출
        if visited[v] == 0: # 방문하지 않았다면
            visited[v] = 1 # 방문 갱신
            mst.append((u,v)) # mst 삽입
            total_weight += weight # 전체 가중치 갱신

            for edge in graph[v]: # 다음 인접 간선 탐색
                if visited[edge[2]] == 0: # 방문한 노드가 아니라면, (순환 방지)
                    heapq.heappush(candidate, edge) # 우선순위 큐에 edge 삽입

    return total_weight

print(prim(graph,1))

```

</details>