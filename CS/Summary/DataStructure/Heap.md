# Heap

## Priority Queue(우선순위 큐)

- 들어간 순서에 상관없이 우선순위가 높은 데이터가 먼저 나오는 자료구조
- queue에 우선순위 개념을 도입한 자료구조
- 우선순위 큐는 Array, LinkedList, Heap으로 구현 가능 → Heap으로 구현하는게 가장 효율적
    - Array
        - 우선순위가 높은 순서대로 배열의 가장 앞부분부터 삽입 → `O(1)`
        - 우선순위가 중간인 데이터를 삽입하는 과정에서 위치를 탐색하고 한칸씩 뒤로 밀어야함 → `O(N)`
    - LinkedList
        - 우선순위가 높은 데이터를 head에 위치하도록 삽입 → `O(1)`
        - 우선순위가 중간인 데이터를 삽입하는 과정에서 위치를 탐색해야 함 → `O(N)`
    - Heap
        - 데이터 삽입, 삭제 모두 → `O(lo2N)`
    

## Heap

- 힙은 완전 이진 트리의 일종이다.
    - 완전 이진 트리는 중복을 허용하지 않음, 힙은 중복을 허용함
- 모든 노드의 우선순위(노드의 값)은 자식 노드의 우선순위보다 크거나 같다
    - 우선순위의 대소관계는 부모-자식 간에만 성립됨. 형제간에는 성립되지 않음
- 힙의 종류 : 최대 힙, 최소 힙
    - 최대 힙 : 노드의 값이 클수록 우선순위가 높다, 루트노드의 값이 가장 크다
    - 최소 힙 : 노드의 값이 작을수록 우선순위가 높다, 루트노드의 값이 가장 작다
- 힙의 삭제 과정
    1. 루트 노드를 삭제
    2. 마지막 노드를 루트 노드로 이동
    3. Heapify 시작 (부모노드와 자식노드 중 큰 노드와 대소 비교)
- 힙의 삽입 과정
    1. 마지막 노드에 삽입
    2. Heapify 시작 (부모노드와 자식노드의 대소 비교)
- 힙 자료구조는 대체적으로 배열로 구현된다.
    - 완전 이진 트리를 기본으로 하기 때문에 빈 공간이 없어 배열로 구현하기 용이함
    - 부모 노드 : `arr[(i - 1) / 2]`
    - 왼쪽 자식 노드 : `arr[i * 2 + 1]`
    - 오른쪽 자식 노드 : `arr[i * 2 + 2]`


<details>
<summary>최대 힙의 삽입, 삭제 과정</summary>

`삽입 과정`
![삽입 과정](/CS-study/CS/DataStructure/img/Heap/3.png)

`삭제 과정`
![삭제 과정](/CS-study/CS/DataStructure/img/Heap/3.png)


</details>

<details>
<summary>최소 힙 구현</summary>

```java
public class minHeap{
        private ArrayList<Integer> heap;

        //최소힙 생성자
        public minHeap() {
            heap = new ArrayList<Integer>();
            heap.add(0); // 0번째 인덱스는 사용 안합
        }

        //삽입
        public void insert(int val) {
            heap.add(val);
            int p = heap.size()-1;    //p는 새로 삽입한 노드의 인덱스 정보
            while(p>1 && heap.get(p/2)>heap.get(p)) {
                //새로 삽입한 노드의 위치가 1 초과이고 부모가 자식보다 크면 진행 ->새로 삽입한 노드의 위치가 루트까지 가거나 새로 삽입한 노드가 부모보다 클때까지 진행
                int tmp = heap.get(p/2);//부모 노드의 값
                heap.set(p/2, val);
                heap.set(p, tmp);

                p /= 2;    //새로 삽입한 노드가 한 레벨 상승했으니 인덱스 부모 노드 인덱스 값으로 변경
            }
        }
        //삭제
        public int delete() {
            //힙 이 비어있으면 0리턴
            if(heap.size()-1 < 1) {
                return 0;
            }

            //삭제할 노드, 루트 노드
            int deleteitem = heap.get(1);

            //마지막 노드를root에 삽입하고 마지막 노드 삭제
            heap.set(1,heap.get(heap.size()-1));
            heap.remove(heap.size()-1);

            int pos = 1; //루트에 새로 삽입한 노드의 인덱스 정보

            //pos*2는 왼쪽자식의 인덱스 값, 자식의 인덱스 값이 힙의 사이즈 값보다 크다는것은 더이상 삽입할 위치를 벗어났다는뜻 
            while((pos*2)<heap.size()) {
                int min = heap.get(pos*2);//왼쪽 자식의 값
                int minPos = pos*2;// 왼쪽 자식의 인덱스 값

                //오른쪽 자식의 인덱스가 사이즈보다 작고 왼쪽 보다 더 작을때 오른쪽 자식을 부모와 바꿔줄 자식으로 지정
                if(((pos*2+1)<heap.size()) && min > heap.get(pos*2+1)) {    
                    min = heap.get(pos*2 +1);
                    minPos = pos*2 +1;
                }

                //부모가 더 작으면 그만
                if(min > heap.get(pos))
                    break;

                //부모 자식 교환
                int tmp = heap.get(pos);
                heap.set(pos,heap.get(minPos));
                heap.set(minPos, tmp);
                pos = minPos;
            }

            return deleteitem;
        }

    }
```

</details>
