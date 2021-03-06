# ๐ฒ Minimum Spanning Tree/์ต์ ์ ์ฅ ํธ๋ฆฌ

### Minimum Spanning Tree๋?
๊ทธ๋ํ G์ spanning tree ์ค ๊ฐ์ค์น์ ํฉ์ด ์ต์์ธ spanning tree๋ฅผ ์๋ฏธํ๋ค.

## MST์ ํน์ง
- ๊ฐ์ ์ ๊ฐ์ค์น์ ํฉ์ด ์ต์์ฌ์ผ ํ๋ค.
- n๊ฐ์ ์ ์ ์ ๊ฐ์ง๋ ๊ทธ๋ํ์ ๋ํด ๋ฐ๋์ (n-1)๊ฐ์ ๊ฐ์ ๋ง์ ์ฌ์ฉํด์ผ ํ๋ค.
- ์ฌ์ดํด์ด ํฌํจ๋์ด์๋ ์๋๋ค.
`spanning tree` : ๊ทธ๋ํ ๋ด์ ์๋ ๋ชจ๋  ์ ์ ์ ์ฐ๊ฒฐํ๊ณ  ์ฌ์ดํด์ด ์๋ ๊ทธ๋ํ๋ฅผ ์๋ฏธํ๋ค.

### Kruskal ์๊ณ ๋ฆฌ์ฆ
ํ์์ ์ธ ๋ฐฉ๋ฒ์ผ๋ก ๊ฐ์ค์น๋ฅผ ์ ํํ์ฌ ๋ชจ๋  ์ ์ ์ ์ต์ ๋น์ฉ์ผ๋ก ์ฐ๊ฒฐํ๋ ์ต์ ์ ํด๋ต์ ์ฐพ๋ ๋ฐฉ์์ด๋ค.
- ์ต์ ๋น์ฉ์ ๊ฐ์ ์ผ๋ก ๊ตฌ์ฑ๋๋ฉฐ ์ฌ์ดํด์ ํฌํจํ์ง ์๊ณ  ๊ฐ ๋จ๊ณ์์ ์ฌ์ดํด์ ์ด๋ฃจ์ง ์๋ ์ต์ ๋น์ฉ ๊ฐ์  ์ ํ
- ๊ฐ์  ์ ํ์ ๊ธฐ๋ฐ์ผ๋ก ํ๋ ์๊ณ ๋ฆฌ์ฆ์ด๋ค.
- ์ด์  ๋จ๊ณ์์ ๋ง๋ค์ด์ง ์ ์ฅ ํธ๋ฆฌ์๋ ๊ด๋ จ์์ด ๋ฌด์กฐ๊ฑด ์ต์ ๊ฐ์ ๋ง์ ์ ํ
- union-find ์๊ณ ๋ฆฌ์ฆ์ ์ด์ฉํ๋ฉฐ ์๊ฐ ๋ณต์ก๋๋ ๊ฐ์ ์ ์ ๋ ฌํ๋ ์๊ฐ์ ์ข์ฐ๋๋ค.
- O(ElogE + E)

<br>

<details>
<summary>ํฌ๋ฃจ์ค์นผ ์๊ณ ๋ฆฌ์ฆ ์ฝ๋</summary>

```python

import sys

v, e = map(int, input().split())
# ๋ถ๋ชจ ํ์ด๋ธ ์ด๊ธฐํ
parent = [0] * (v+1)
for i in range(1, v+1):
    parent[i] = i

# ๋ถ๋ชจ๋ฅผ ์ฐพ๋ ์ฐ์ฐ 
def find_parent(parent, x):
    if parent[x] != x:
        parent[x] = find_parent(parent, parent[x])
    return parent[x]

# union ์ฐ์ฐ
def union_parent(parent, a, b):
    a = find_parent(parent, a)
    b = find_parent(parent, b)
    if a < b:
        parent[b] = a
    else:
        parent[a] = b

# ๊ฐ์  ์ ๋ณด ๋ด์ ๋ฆฌ์คํธ์ ์ต์ ์ ์ฅ ํธ๋ฆฌ ๊ณ์ฐ ๋ณ์ ์ ์
edges = []
total_cost = 0

# ๊ฐ์  ์ ๋ณด ์ฃผ์ด์ง๊ณ  ๋น์ฉ์ ๊ธฐ์ค์ผ๋ก ์ ๋ ฌ
for _ in range(e):
    a, b, cost = map(int, input().split())
    edges.append((cost, a, b))

# ๊ฐ์  ์ ๋ณด ๋น์ฉ ๊ธฐ์ค์ผ๋ก ์ค๋ฆ์ฐจ์ ์ ๋ ฌ
edges.sort()

# ๊ฐ์  ์ ๋ณด ํ๋์ฉ ํ์ธํ๋ฉด์ ํฌ๋ฃจ์ค์นผ ์๊ณ ๋ฆฌ์ฆ ์ํ
for i in range(e):
    cost, a, b = edges[i]
    # find ์ฐ์ฐ ํ, ๋ถ๋ชจ๋ธ๋ ๋ค๋ฅด๋ฉด ์ฌ์ดํด ๋ฐ์ X์ผ๋ฏ๋ก union ์ฐ์ฐ ์ํ -> ์ต์ ์ ์ฅ ํธ๋ฆฌ์ ํฌํจ!
    if find_parent(parent, a) != find_parent(parent, b):
        union_parent(parent, a, b)
        total_cost += cost

print(total_cost)
```

</details>

<br>
<br>

### Prim ์๊ณ ๋ฆฌ์ฆ
์์ ์ ์ ์์ ๋ถํฐ ์ถ๋ฐํ์ฌ ์ ์ฅํธ๋ฆฌ ์งํฉ์ ๋จ๊ณ์ ์ผ๋ก ํ์ฅํด๋๊ฐ๋ ๋ฐฉ๋ฒ์ด๋ค.

- ์ ์  ์ ํ์ ๊ธฐ๋ฐ์ผ๋ก ํ๋ค.
- ์ด์  ๋จ๊ณ์์ ๋ง๋๋ ์ง ์ ์ฅํธ๋ฆฌ๋ฅผ ์ ์  ํ์ฅํ๋ ๋ฐฉ์์ด๋ค. (ํฌ๋ฃจ์ค์นผ์ ์ด์  ์ ์ฅํธ๋ฆฌ์๋ ์ฐ๊ด์ด ์์.)
- ์๊ฐ ๋ณต์ก๋์ ๊ฐ์ฅ ํฐ ์ํฅ์ ๋ฏธ์น๋ ๊ฒ์ ๊ฐ์ค์น๊ฐ ๊ฐ์ฅ ์์ ์ ์ ์ ์ฐพ์๋ด๋ ๊ฒ๊ณผ ์ธ์ ํ ์ ์ ์ ํ์์ด๋ค.
- ๋ชจ๋  ๋ธ๋์ ๋ํด ํ์ O(V)์ด๊ณ  ์ฐ์  ์์ํ ์ฌ์ฉ์ผ๋ก O(logV) ์ต์ข ์๊ฐ ๋ณต์ก๋๋ O(ElogV)์ด๋ค.

<details>
<summary>ํ๋ฆผ ์๊ณ ๋ฆฌ์ฆ ์ฝ๋</summary>

```python
import heapq
import collections
import sys
sys.setrecursionlimit(10**6)
input = sys.stdin.readline

n, m = map(int,input().split()) # ๋ธ๋ ์, ๊ฐ์  ์
graph = collections.defaultdict(list) # ๋น ๊ทธ๋ํ ์์ฑ
visited = [0] * (n+1) # ๋ธ๋์ ๋ฐฉ๋ฌธ ์ ๋ณด ์ด๊ธฐํ

# ๋ฌด๋ฐฉํฅ ๊ทธ๋ํ ์์ฑ
for i in range(m): # ๊ฐ์ฑ ์ ๋ณด ์๋ ฅ ๋ฐ๊ธฐ
    u, v, weight = map(int,input().split())
    graph[u].append([weight, u, v])
    graph[v].append([weight, v, u])


# ํ๋ฆผ ์๊ณ ๋ฆฌ์ฆ
def prim(graph, start_node):
    visited[start_node] = 1 # ๋ฐฉ๋ฌธ ๊ฐฑ์ 
    candidate = graph[start_node] # ์ธ์  ๊ฐ์  ์ถ์ถ
    heapq.heapify(candidate) # ์ฐ์ ์์ ํ ์์ฑ
    mst = [] # mst
    total_weight = 0 # ์ ์ฒด ๊ฐ์ค์น

    while candidate:
        weight, u, v = heapq.heappop(candidate) # ๊ฐ์ค์น๊ฐ ๊ฐ์ฅ ์ ์ ๊ฐ์  ์ถ์ถ
        if visited[v] == 0: # ๋ฐฉ๋ฌธํ์ง ์์๋ค๋ฉด
            visited[v] = 1 # ๋ฐฉ๋ฌธ ๊ฐฑ์ 
            mst.append((u,v)) # mst ์ฝ์
            total_weight += weight # ์ ์ฒด ๊ฐ์ค์น ๊ฐฑ์ 

            for edge in graph[v]: # ๋ค์ ์ธ์  ๊ฐ์  ํ์
                if visited[edge[2]] == 0: # ๋ฐฉ๋ฌธํ ๋ธ๋๊ฐ ์๋๋ผ๋ฉด, (์ํ ๋ฐฉ์ง)
                    heapq.heappush(candidate, edge) # ์ฐ์ ์์ ํ์ edge ์ฝ์

    return total_weight

print(prim(graph,1))

```

</details>