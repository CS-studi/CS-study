# Tree

## Tree 란?
트리는 노드로 이루어진 비선형적 구조이다.

**만족해야하는 조건 5가지**
1. 임의의 노드에서 다른 노드로 가는 경로는 유일하다.
2. **사이클(회로)가 존재하지 않는다.**
3. 모든 노드는 서로 연결되어 있다.
4. **edge:간선을 하나 자르면 트리가 두개로 분리된다.**
5. 간선의 수|E|는 |V|에서 1을 뺀 것과 같다.

**트리를 구성하는 용어**
- 루트 노드: 부모가 없는 노드, 트리는 하나의 루트노드만을 가진다.
- 단말 노드(terminal, leaf): 자식이 없는 노드
- 내부 노드(Internal): 단말 노드가 아닌 노드
- 부모 노드: 루트 노드 방향으로 직접 연결된 노드
- 형제 노드: 같은 부모를 가지는 노드
- 자식 노드: 루트 노드 반대방향으로 직접 연결된 노드
- 간선: 노드를 연결하는 선
- 노드의 크기: 자신을 포함한 모든 자손노드의 개수
- 깊이: 루트에서 어떠 노드에 도달하기 위해 거쳐야하는 간선의 수
- 레벨: 트리의 특정 깊이를 가지는 노드의 집합
- 차수: 각 노드가 지닌 자식의 수
- 트리의 차수: 트리의 최대 차수
- 트리의 높이: 루트 노드에서 가장 깊숙히 있는 노드의 깊이

## Binary Tree
> 이진트리란 자식 노드가 최대 두개의 노드들로 구성된 트리이다.
> 힙 정렬과 이진 탐색 트리는 모두 이진 트리를 기반으로 만들어진 기법이다.

- 전 이진 트리(full)
    - 잎새 노드를 제외한 모든 노드가 자식 노드를 `2개 또는 0개` 가짐
- 완전 이진 트리(complete)
    - `마지막 레벨을 제외한 모든 레벨에서 노드들이 꽉 채워진 이진트리`
    - 데이터가 `왼쪽 단말노드`부터 채워져야 한다.
    - 1차원 배열로도 표현이 가능
- 균형 이진 트리(balanced)
    - 모든 잎새 노드의 깊이 차이가 최대 1인 트리
    - 균형 이진 트리는 예측 가능한 깊이를 가지며, 노드가 n개인 균형 이진 트리의 깊이는 log n을 내림한 값을 가진다.

## 순회
<details>
<summary>전위 순회 코드</summary>
def preorder(self, node):
    print(node, end = '')
    if not node.left == None : self.preorder(node.left)
    if not node.right == None : self.preorder(node.right)
</details>

<details>
<summary>중위 순회 코드</summary>
def inorder(self, node):
    if not node.left == None : self.preorder(node.left)
    print(node, end = '')
    if not node.right == None : self.preorder(node.right)
</details>

<details>
<summary>후위 순회 코드</summary>
def preorder(self, node):
    if not node.left == None : self.preorder(node.left)
    if not node.right == None : self.preorder(node.right)
    print(node, end = '')
</details>

## 이진 탐색 트리(Binary Search Tree/BST)
- 이진 트리의 일종으로 노드 왼쪽은 자기 자신보다 작은 값만 가지고, 오른쪽은 자기 자신 보다 큰 값만을 가진다.
- 이렇게 구성하면 어떤 값 n을 찾을 때, 루트 노드와 비교해서 n이 더 작다면 오른쪽 트리를 탐색할 필요가 없다.
- 이상적인 상황에서 탐색/삽입/삭제 모두 시간 복잡도가 O(logn)이다. (이상적인 상황 = 균형잡인 이진 트리)