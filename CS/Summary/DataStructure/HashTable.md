# Hash Table

`key`에 대한 `hash` 값을 인덱스로 사용하여 `key-value` 쌍의 데이터를 저장하고 조회하며, key-value 쌍의 데이터의 개수에 따라 동적으로 크기가 증가하는 자료구조이다. 

**hash table**은 `array`로 이루어져있고 array 각각의 주소를 `bucket` 이라 부른다.
**hash function** 은 key-value의 `key값을 array의 index로 매핑`시키는 함수이다.

## Hash Functin
해시 함수는 임의 크기의 데이터를 고정 크기의 값으로 매핑하는데 사용되는 함수이다.

- hash function로 hash table이라고 불리는 array에 저장될 데이터의 위치(index)를 구할 수 있다.
    - index를 구할 때, 보통 `mod` 연산을 사용한다.
    - hash function을 통해 index를 구하는 과정을 `hashing` 이라고 한다.

## Hash Collision
> 1/m 확률의 해시 충돌 (hash table의 크기: m)

hash function의 표현범위를 m으로 좁힘으로써 서로 다른 hashCode값을 가진 객체가 1/m 의 확률로 동일한 해시 버킷 인덱스를 가질 수 있는 해시 충돌(hash collision) 이 발생한다. 해시 충돌은 hash function을 아무리 잘 구현하여도 상관없이 발생한다.

## `해시 충돌` 해결방법

### Open Addressing (개방 주소법)
데이터를 삽입하려는 hash bucket이 이미 사용중이라면 비어있는 bucket을 찾아 데이터를 삽입하는 방식이다.

- Worst Case의 경우 비어있는 bucket을 찾지 못하고 탐색을 시작한 위치로 되돌아올 수 있다.
    - 데이터가 존재하는 bucket이 모여있으면 Worse Case 발생 빈도가 높아진다.
- 비어있는 bucket을 탐색하는 방식에는 Linear Probing, Quadratic Probing, Double Hashing 이 있다.
    - Linear Probing(선형 탐색)
        - 해시 충돌이 발생한 현재 bucket index부터 고정폭 `n`만큼 이동하면서 비어있는 bucket을 찾아 데이터를 저장하는 방식이다.
        - `Primary Clustering` 문제가 발생한다. 
        - 데이터가 존재하는 filled bucket들이 모여있다면 비어있는 bucket을 찾기 까지의 탐색 시간이 늘어난다. Primary Cluster가 형성되면 빠르게 커지고 결국 성능 저하를 야기한다.
    - Quadratic Probing(제곱 탐색)
        - 해시 충돌이 발생한 현재 bucket index부터 `n^2` 만큼 이동하면서 비어있는 bucket을 찾아 데이터를 저장하는 방식으로 Linear Probing 방식의 Primary Clustering 발생 가능성을 줄일 수 있다.
        - `Secondary Clustering` 문제가 발생한다.
        - `n^2` 간격으로 filled bucket이 존재하여 key의 hash값(index) 보다 **훨씬 떨어진 곳에 데이터가 삽입되는 현상**이다. 이는 filled bucket 군집을 크게만들지 않기 때문에 Primary Clustering 보다 덜 심각한 문제이다.
    - Double Hashing(이중 해싱)
        - hash값을 다른 hash function으로 한번 더 해싱하여 hash의 규칙성을 없애는 방식으로 Secondary Clustering 발생 가능성을 줄일 수 있다.

### Open Addressing의 데이터 탐색 및 삭제
- 탐색
    - target 데이터를 찾거나 empty bucket에 도달하기 전까지 탐색(probing)을 진행한다. target 데이터를 찾는 과정에서 empty bucket에 도달하면 탐색(probing)을 종료하는 문제점이 있다.
- 삭제
    - 중간 요소를 삭제하게 되면 탐색 중 empty bucket을 만나 탐색을 종료하게 되어 원하는 요소를 탐색할 수 없는 문제가 발생한다.
    - 삭제한 데이터의 bucket에 dummy node 를 넣거나 flag (Occupied, Empty, Deleted) 를 활용하여 탐색이 올바르게 진행되도록 할 수 있다.
### Separate Chaining (분리 연결법)
각각의 hash bucket을 LinkedList를 가리키는 포인터로 구성하여 해시 충돌시 해당 bucket의 LinkedList에 추가하는 방식이다.

- 일반적으로 Seperate Chaining 방식은   Open Addressing 방식보다 빠르다.
    - Open Addressing은 데이터가 존재하는 bucket이 모여있으면 Worst Case 발생 빈도가 높아진다
    - Seperate Chaining은 해시 출돌을 피하도록 보조 해시 함수를 조정하면 Worse Case에 가까워지는 빈도를 줄일 수 있다.
- Java 7에서 HashMap은 Seperate Chaining 방식으로 구현되어있다.
- Seperate Chaining 방식은 보조 해시 함수를 사용하여 해시 충돌 가능성을 줄인다.

- Red Black Tree로 구현한 Separate Chaining
    - 하나의 hash bucket에 해당하는 LinkedList의 데이터 개수가 8개가 되면 구조를 LinkedList -> Red-Black Tree 로 변환한다. LinkedList 탐색 성능의 Worst Case는 O(N) 이지만 Red-Black Tree의 경우 O(logN)이기 때문이다.

    - 하나의 hash bucket에 할당된 데이터 개수가 6개이면 구조를 Red-Black Tree -> LinkedList로 변환한다. 데이터 개수가 적으면 Red-Black Tree와 LinkedList간의 탐색 성능 차이가 거의 없고, Red-Black Tree는 LinkedList보다 메모리 사용량이 많기 때문이다.

    - 변환 기준을 6개, 8개 처럼 2개라는 여유를 두어 과도한 구조 변환을 막을 수 있다. 만약 변환 기준이 6개 , 7개 이라면 데이터가 반복적으로 삽입, 삭제되는 경우 불필요한 LinkedList ↔ Tree 변환이 일어나 성능 저하가 발생할 수 있다.

### Open Addressing vs Seperate Chaining

|  | Open Addressing | Seperate Chaining |
| :---: | --- | --- |
| Worst Case | O(M) | O(M) |
| 캐시 효율 | 좋다 (연속된 공간에 데이터를 저장하기 때문이다) | Open Addressing 보다 좋지 않다 (해시 충돌시 LinkedList에 데이터를 저장하기 때문이다) |
| 공간 효율 | 좋다 (해시 충돌이 발생한 경우에도 probing을 통해 빈 bucket에 저장되기 때문이다) | Open Addressing 보다 좋지 않다 (해시 충돌이 발생하면 LinkedList에 추가되기 때문에 사용되지 않는 bucket이 존재한다) |
| Resizing 빈도 | 높다 (bucket 사용률이 높아 load factor의 임계점에 쉽게 도달하기 때문이다) | Open Addressing 보다 낮다 (bucket 사용률이 낮기 때문이다) |


## Dynamic Resizing
hash bucket의 개수가 적다면 메모리 사용을 아낄 수 있지만 해시 충돌로 인해 성능 저하가 발생한다. 그러므로 자바의 HashMap은 `load factor`가 `임계점(0.75)`을 넘어갈 경우에 hash bucket 개수를 2배로 늘린다.