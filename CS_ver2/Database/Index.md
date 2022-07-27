# Index

<br/>
<br/>

## 디스크 IO

> 데이터베이스의 디스크 IO 작업은 물리적인 작업이 동반되기 때문에 시간이 많이 소요된다.
> 
- `탐색 시간(seek time)` : 디스크 헤더를 데이터가 저장된 트랙으로 옮기는데 걸리는 시간
- `회전 대기 시간(rotational latency time)` : 트랙에서 섹터 단위로 이동하면서 목적 데이터를 찾는데 걸리는 시간

결국 디스크의 성능은 디스크 헤더의 위치 이동을 최소화하면서 한번에 얼마나 많은 데이터를 읽을 수 있는지에 따라 결정된다.

> 순차 IO, 랜덤 IO
> 
- `순차 IO` : 논리적, 물리적 순서를 따라 차례대로 데이터를 읽어나가는 방식
    - 3개의 페이지를 읽기 위해 1번의 시스템 콜을 요청한다. 시작점만 알면 나머지 2개의 페이지는 연속적으로 존재하기 때문에 1개의 시작점을 찾기 위해 1번의 시스템콜이 필요하다
- `랜덤 IO` : 논리적, 물리적 순서를 따르지 않고 한 건의 데이터를 읽기 위해 한 블록씩 랜덤하게 접근하는 방식
    - 3개의 페이지를 읽기 위해 3번의 시스템 콜을 요청한다. 각각의 페이지는 랜덤한 위치에 있으므로 3지점의 위치를 찾기 위해 3번의 시스템 콜이 필요하다.

> 순차 IO는 랜덤 IO보다 빠르다
> 

데이터 갱신 위주의 서비스나 Index Range Scan은 랜덤 IO으로 수행되고, Full Table Scan은 주로 순차 IO로 수행된다.

> 대부분의 IO는 랜덤 IO방식으로 수행된다.
> 

순차 IO는 랜덤 IO보다 속도가 빠르다. 하지만 현실적으로 원하는 데이터가 디스크에 순차적으로 저장되어있긴 힘들다. → 즉, `대부분의 IO는 랜덤 IO 방식으로 수행된다`

따라서 데이터를 빠르게 조회하기 위해서 랜덤 IO의 빈도를 줄이는 `쿼리 튜닝`이 필요하다. (꼭 필요한 데이터만 읽도록 쿼리를 개선해야 한다)

<br/>
<br/>

## Index

인덱스는 검색 성능을 높혀주는 데이터베이스의 자료구조이다.

인덱스로 설정된 칼럼이 포함된 테이블을 초기에 생성하면 `.frm` , `.myd` , `.myi` 파일이 만들어진다.

- frm 파일 : 테이블 구조가 저장된 파일
- myd 파일 : 실제 데이터가 저장된 파일
- `myi 파일` : 인덱스가 저장된 파일
    - 인덱스를 사용하지 않는다면 myi 파일은 비어있게 된다
    - select 쿼리가 수행될 경우 myi 파일의 내용을 검색한다. 인덱스를 사용하지 않는다면 Table Full Scan이 이뤄진다.

> 인덱스의 구성 : `인덱스 컬럼 값(key) - 물리적 주소(value)`
> 

테이블의 특정 칼럼(속성)에 대해 인덱스를 생성하면 `인덱스 컬럼 값(key) - 물리적 주소(value)` 쌍의 데이터(인덱스)가 `.myi` 파일에 저장된다.

> 인덱스의 장단점
> 

`장점`

- 인덱스에 의해 데이터가 정렬되므로 검색 성능(select 쿼리 성능)이 향상된다.

`단점`

- update, insert, delete 수행시 추가적인 작업이 동반되어 성능이 떨어진다.
    - `특정 칼럼을 update할 경우` : 칼럼의 기존 인덱스(old index)를 ‘사용하지 않음’ 처리를 하고 → 갱신된 데이터에 대한 인덱스(new index)를 추가한다.
        
        old index를 제거하지 않고 ‘사용하지 않음’ 표시만 하기 때문에 myi 파일의 크기가 커져 별도의 저장공간을 차지한다.
        
    - `특정 칼럼을 insert할 경우` :  새롭게 삽입된 데이터에 대한 인덱스를 생성하여 추가한다.
        - indext split : 인덱스의 Block들이 하나에서 두개로 나누어지는 현상.
            - 인덱스는 데이터가 순서대로 정렬되어야 한다. 기존 블록에 여유 공간이 없는 상황에서 그 블록에 새로운 데이터가 입력되어야 할 경우, 오라클이 기존 블록의 내용 중 일부를 새 블록에다가 기록한 후, 기존 블록에 빈 공간을 만들어서 새로운 데이터를 추가하게 된다.
            - Index split은 새로운 블록을 할당받고 Key를 옮기는 복잡한 작업을 수행한다. 모든 수행 과정이 Redo에 기록되고 많은 양의 Redo를 유발한다.
            - Index split이 이루어지는 동안 해당 블록에 대해 키 값이 변경되면 안되므로 DML이 블로킹된다
    - `특정 칼럼을 delete할 경우` : 삭제된 데이터에 해당하는 인덱스를 ‘사용하지 않음’ 표시만 해두고 삭제하지 않는다
- 인덱스 정보를 위한 10% 내외의 추가 저장공간이 필요하다.

> 인덱스를 사용하면 좋은 컬럼
> 

저장되는 데이터의 범위가 넓고, 중복이 적고, 조회가 많거나, 정렬된 상태일 수록 유용한 칼럼에 인덱스를 생성하는게 바람직하다.

- `저장되는 데이터의 범위가 넓고 중복이 적은 칼럼`
    
    성별과 같이 데이터의 범위가 “남, 녀” 두개인 경우 데이터의 범위가 매우 좁고 중복이 잦기 때문에 인덱스로 바람직하지 않다. (where 조건으로 검색하거나 index range scan할 경우 인덱스를 사용해도 범위가 눈에 띄게 줄어들지 않기 때문)
    
- `insert, update, delete 작업이 자주 발생하지 않는 칼럼`
    
    추가적인 작업이 동반되어 성능이 떨어지기 때문이다.
    

<br/>
<br/>

<br/>
<br/>



## Index 종류

3. `관리 방식(알고리즘)`에 따른 인덱스 구분 → `B-Tree Index, Hash Index, Fractal-Tree Index`
1. `역할`에 따른 인덱스 구분 → `Clustered Index, Non-Clustered Index, Unique Index`
2. `중복값 여부`에 따른 인덱스 구분 → `Unique Index, Non-Unique Index`

<br/>
<br/>

<br/>
<br/>


## 📌관리 방식(알고리즘)에 따른 Index

### `🧩B-Tree(Balanced Tree) Index` ([참고](https://devlog-wjdrbs96.tistory.com/342#:~:text=B-Tree%20%EC%9D%B8%EB%8D%B1%EC%8A%A4%EB%8A%94%20%EB%8D%B0%EC%9D%B4%ED%84%B0%EB%B2%A0%EC%9D%B4%EC%8A%A4,%EC%95%84%EB%8B%88%EB%9D%BC%20Balanced%EB%A5%BC%20%EC%9D%98%EB%AF%B8%ED%95%98%EB%8A%94%EB%8D%B0%EC%9A%94.))

데이터베이스 인덱싱 알고리즘 가운데 가장 일반적으로 사용되는 B-Tree 알고리즘으로 관리되는 인덱스이다.

> 구조
> 

![image](https://user-images.githubusercontent.com/44316546/181248647-59a4108e-b6c5-4003-9918-3ff1e0dd53d8.png)

- `Root Node, Branch Node` : `index 칼럼 값 - Child Node 주소` 쌍의 데이터를 가지고 있다. (leaf node로 수직적 탐색에 필요한 정보)
- `Leaf Node` : `index 칼럼 값 - 실제 데이터의 레코드 주소` 쌍의 데이터를 가지고 있다. (leaf node는 실제 디스크의 주소를 가리킨다)
- Node는 Page를 의미한다.
    - Page는 InnoDB에서 디스크에 데이터를 저장하는 기본 단위이다. (디스크의 모든 읽기, 쓰기 작업 단위 = page)

> B-Tree는 랜덤 IO?
> 

인덱스의 key값은 정렬되어있지만 디스크 속 실제 데이터 파일들은 랜덤하게 저장되어있다.

> B-Tree Index의 트리 탐색(Tree Traversal)
> 

`B-Tree Index의 트리 탐색`이란 root node → branch node → leaf node까지 범위 비교를 하면서 인덱스를 탐색하는 작업이다.

![image](https://user-images.githubusercontent.com/44316546/181248698-7aa25934-7147-4ec5-94ef-28919d09476e.png)

***MySQL은 기본적으로 B-Tree Index를 사용한다. MySQL의 InnoDB는 B-Tree의 확장된 버전인 B+Tree를 사용한다.***

### `🧩Hash Index`

해시 함수로 관리되는 인덱스이다.

> 구조
> 

![image](https://user-images.githubusercontent.com/44316546/181248731-2e72c35a-a63e-4760-a256-14acfea1ab1f.png)

- node가 아닌 bucket에 Hash Index가 저장된다.
- Hash Index는 `index 키 - 실제 데이터의 레코드 주소` 쌍으로 구성된다.
- Hash Index의 index키는 `칼럼의 해시값(칼럼의 원본값)` 으로 구성된다.

> 속도
> 

B-Tree Index보다 검색 속도가 매우 빠르다 → O(1)

> Hash Index를 사용하면 범위 연산으로 데이터를 검색할 수 없다
> 

Hash Index의 경우 동등 연산에만 특화된 자료구조이므로 부등호 연산 `<, >, ≥ , ≤`시 문제가 발생한다. 이러한 이유로 index 관리에 Hash가 아닌 B-Tree 자료구조를 사용한다.

<br/>
<br/>

## 왜 B-Tree 로 데이터베이스 인덱스를 관리할까?

> Balanced Tree vs Tree
> 
- `Tree`
    
    Tree의 평균적인 탐색 시간복잡도는 O(logN)이다. 하지만 최악의 경우(한쪽으로 치우쳐서 데이터가 저장된 경우) O(N)의 시간복잡도를 가진다.
    
- `Balanced Tree`
    
    Balanced Tree의 경우 최악의 경우에도 O(logN)의 시간복잡도를 가진다.
    

> Balanced Tree에는 B-Tree, B+Tree, Red-Black Tree가 존재한다.
> 

Balanced Tree에는 B-Tree말고도 B+Tree, Red-Black Tree가 존재한다. 그럼에도 불구하고 B-Tree를 사용하는 이유는 무엇일까?

`B-Tree의 특징`

- 자식 노드가 여러개 존재하고, 각 노드는 데이터를 가질 수 있다.
- 각 노드 속 데이터들은 항상 정렬된 상태이다.
- 좌, 우 자식 노드 개수의 균형을 유지하기 때문에 탐색 시 O(logN)의 시간복잡도를 가진다.

> Red-Black Tree는 왜 사용하지 않을까?
> 

B-Tree와 Red-Black Tree의 차이는 `하나의 노드가 가질 수 있는 데이터의 개수`이다.

- B-Tree는 하나의 노드에 여러개의 데이터를 저장할 수 있다. 노드 속에 존재하는 데이터들로 범위 비교를 통해 타켓 데이터가 저장되어있는 노드로 이동할 수 있다.
    
    ![image](https://user-images.githubusercontent.com/44316546/181248776-992dcc44-2c90-4bda-81dc-26d4ec35c9ea.png)
    
    빨간색 부분이 노드이다. 노드에는 데이터가 배열처럼 실제 메모리에 인접하여 저장된다. 따라서 같은 노드 속의 데이터에 접근할 때 시작 위치의 포인터만 필요하다.
    
    - 필요한 포인터 개수가 적다 → 포인터로 데이터에 접근하는 빈도가 적다 → IO가 적다!
- Red-Black Tree는 하나의 노드에 하나의 데이터 요소만 저장된다.
    
    ![image](https://user-images.githubusercontent.com/44316546/181248806-1e286106-f828-4cd9-a434-df96bf9f433b.png)
    
    하나의 노드에는 하나의 데이터만 존재한다. 그렇기 때문에 B-Tree보다 노드의 개수가 훨씬 많고, 노드마다 데이터로 접근하기 위해 필요한 포인터의 개수도 많다.
    
    - 필요한 포인터의 개수가 많다 → 포인터로 데이터에 접근하는 빈도가 많다 → IO가 많다!
    

이와 같은 이유로 B-Tree가 Red-Black Tree보다 탐색 속도가 빠르다.

> 참조 포인터 개수가 탐색시간을 좌우한다면, 데이터베이스 인덱스를 Array로 관리하면 안될까? → NO!!
> 

배열은 접근속도도 O(1)로 매우 빠르고, Hash와 다르게 부등호 연산도 가능하다. 하지만 데이터를 저장하거나 삭제할 경우 B-Tree보다 매우 비효율적이다.

- 배열이 B-Tree보다 나은 점은 탐색 성능 뿐이다.

데이터를 배열 중간에 삽입하거나 삭제하면 뒤의 데이터들을 shift해야한다. 최악의 경우 O(N)의 시간복잡도가 소요된다.

1. 삽입, 삭제할 데이터를 찾기 위해 탐색 시간이 최소 O(logN)이 걸린다. 
    - 배열의 데이터가 정렬되어있다면 이진탐색으로 O(logN)의 시간복잡도로 삭제, 삽입할 데이터를 찾을 수 있다.
    - 정렬되어있지 않다면, 퀵소트, 머지소트 등으로 O(NlogN)의 시간복잡도로 데이터를 찾을 수 있다.
2. shift할 때 최악의 경우 O(N)이 걸린다.

> LinkedList로 데이터베이스 인덱스를 관리하면 어떨까?
> 

삽입, 삭제의 경우 O(1)으로 빠르지만 탐색을 무조건 HEAD부터 시작해야하기 때문에 매우 비효율적이다.

> B+Tree로 인덱스를 관리하면 어떨까? → 모르겠음..
> 

B+Tree는 B-Tree의 leaf node들을 LinkedList로 연결한 것이다. LinkedList의 탐색 비효율성 때문에 사용하지 않는걸까??...

| 구분 | B-Tree | B+Tree |
| --- | --- | --- |
| 데이터포인터 | 모든 내부적인 노드들은 데이터 포인터를 지님 | 리프 노드에만 데이터 포인터가 존재 |
| 시퀀셜 엑세스 탐색 방식 | 모든 key가 리프노드에 존재하지 않기 때문에 모든 노드를 탐색해야 함 | 모든 key가 리프노드에 존재하기 때문에 리프 노드 레벨에서 탐색하면 됨 |
| 키 중복여부 | 모든 노드는 서로 다른 key를 지님 | 부모 노드와 자식 노드가 같은 key를 가질 수 있음 |
| 링크드 리스트 | 존재하지 않음 | 리프 노드는 링크드 리스트로 연결되어 있음 |

<br/>
<br/>

<br/>
<br/>

## 📌역할에 따른 Index

### `🧩 Clustered Index`

테이블의 PK에만 적용되는 인덱스이다.

- Clustered Index는 테이블 당 한개만 생성할 수 있다.
- Clustered Index가 적용된 칼럼이 추가될 때마다 Data Page 전체를 재정렬한다
- Clustered Index의 leaf page가 곧 data page이다. (leaf page에 데이터가 포함되어 있다)
- Clustered Index로 타겟 데이터를 검색할 경우 `O(logN)`의 시간복잡도가 소요된다.
- Unique + Not Null의 특징을 가진다

> 데이터는 Clustered Index 기준으로 정렬된다
> 

테이블에 삽입되는 순서와 상관없이 **Clustered Index로 설정된 PK를 기준**으로 순차적으로 물리적 레코드에 저장된다.

테이블의 PK값이 변경되면 레코드에 저장된 데이터의 물리적 위치도 new PK에 맞게 변경된다.

> Clustered Index 구조
> 

![image](https://user-images.githubusercontent.com/44316546/181248870-6e81c7f7-94ec-46c5-96bd-44ca3e906b63.png)

index page에는 root node, leaf node가 있고, leaf node가 곧 data page이다.

- root node는 `시작 데이터 key-data page 번호` 쌍의 데이터가 존재한다.
    - key=6인 데이터를 찾기 위해서 `5-102` 데이터를 참조하여 102번의 data page로 이동해야 한다.
- leaf node는 곧 data page이며 data page의 데이터들은 PK값을 기준으로 순차 정렬되어 있다.
    - data page의 데이터는 `데이터의 key-데이터의 value` 쌍으로 이뤄져 있다.
    - data page의 데이터들은 value를 기준으로 순차 정렬된다.

> Clustered Index의 속도
> 
- `검색 속도` : data page의 데이터들이 값을 기준으로 정렬되어 있기 때문에 검색 속도가 Non-Clustered Index보다 빠르다
- `입력, 수정, 삭제 속도` : 데이터를 입력, 수정, 삭제하면 data page를 재정렬해야 하므로 Non-Clustered Index보다 느리다.

> Clustered Index로 설정하기에 적합하지 않은 컬럼
> 

주민등록번호처럼 법률적인 이유로 인해 PK에서 일반 칼럼으로 변경될 가능성이 있는 칼럼은 PK로 적합하지 않다.

**따라서 주민등록번호처럼 PK에서 일반 칼럼으로 변경될 가능성이 있는 칼럼은 `Unique Key`로 설정하고, PK는 `auto_increment의 인조 키`로 설정하는게 바람직하다.**

### `🧩 Non-Clustered Index`

PK를 제외한 나머지 칼럼에 적용되는 인덱스이다.

- 하나의 테이블에 여러 Non-Clustered Index를 생성할 수 있다. (남용시 성능 저하)
- Non-Clustered Index의 data page와 실제 데이터는 독립적이다.
- leaf page에는 실제 데이터가 아닌 데이터가 위치하는 주소가 존재한다.

> Non-Clustered Index 구조
> 

![image](https://user-images.githubusercontent.com/44316546/181248918-2702d776-61a5-4701-97ff-da572455dd42.png)

- index page는 root node, leaf node가 있고, 실제 데이터는 data page에 저장된다.
    - leaf page에는 실제 데이터가 존재하는 주소(address)가 담겨져 있다.
    - data page에는 실제 데이터가 저장되어 있고, 정렬되어있지 않는다.

> Non-Clustered Index의 속도
> 
- `검색 속도` : Clustered Index보다 느리다
- `입력, 수정, 삭제 속도` : Clustered Index보다 빠르다

|  | Clustered Index | Non-Clustered Index |
| --- | --- | --- |
| 정렬 여부 | PK의 정렬순서에 맞게 데이터 페이지가 정렬되어있다. | 정렬 순서와 상관없이 인덱스가 저장된다. |
| 개수 | 테이블 당 오직 하나만 존재한다. | 한 테이블에 여러 개 생성할 수 있다 |
| 검색 속도 | 데이터 페이지가 군집화 되어있고, 물리적으로 정렬되어있기 때문에 Non-Clustered Index보다 검색 속도가 빠르다 | 데이터 페이지에 접근하기 위해 여러 인덱스 페이지(root → leaf)를 거쳐야 한다. |
| 데이터 삽입 | 테이블 속 데이터들을 정렬해야하기 때문에 insert 비용이 크다 | 별도의 공간에 인덱스를 생성해야하기 때문에 추가작업이 필요하다 |

<br/>
<br/>

<br/>
<br/>

## 📌중복값 여부에 따른 Index

### `🧩Unique Index`

- DBMS 쿼리를 수행하는 Optimizer는 동등 조건(==)으로 Unique Index의 중복 여부를 확인한다.
- Unique Index로 데이터를 검색할 경우, Optimizer가 동등 조건으로 타겟 데이터를 탐색한다. 찾았다면 탐색을 멈춘다. 왜냐하면 테이블의 Unique Index는 오직 한개 뿐이기 때문이다.

> Primary Key(PK)와 Unique Index 비교
> 

|   | Primary Key | Unique Index |
| --- | --- | --- |
| 목적 | Constraint + Index | Index |
| 공통점 | 유일성 보장 | 유일성 보장 |
| 참조 무결성 | PK/FK에 의해 지정 가능 | 지정 불가능 |
| 테이블당 개수 | 1개만 가능 | 여러 개 가능 |
| 인덱스 생성 | Unique Index 생성 | Unique Index 생성 |
| 역공학 적용시 | PK 인식 가능 | PK인식 불가능 |
| Null 허용 | 허용 안됨 | 허용됨 |

### `🧩Non-Unique Index`

Unique 인덱스가 아닌 인덱스들이다.


<br/>
<br/>

<br/>
<br/>

## Index를 활용한 데이터 검색

### Index Full Scan

`Index Full Scan`이란 인덱스 칼럼을 처음부터 끝까지 모두 읽는 스캔 방식이다.

> Index Full Scan 방식
> 

![image](https://user-images.githubusercontent.com/44316546/181248963-d89a77fa-1252-4b83-98e0-973b88d5e927.png)

수직적 탐색(root node → branch node → leaf node)없이 leaf node의 첫번째 블록부터 끝까지 수평적 탐색을 한다.

> Index Full Scan vs Table Full Scan
> 
- 인덱스의 크기가 테이블의 크기보다 훤씬 작기 때문에 Index Full Scan이 Table Full Scan보다 빠르다
- 인덱스 선두 칼럼(탐색의 시작점)이 조건절에 없거나, 끝까지 스캔하는 경우 Table Full Scan을 고려해야 한다
    - Index Full Scan은 랜덤 IO가 자주 발생하지만 Table Full Scan의 경우 한번에 큰 block을 긁어오기 때문에 IO가 덜 발생한다?(뇌피셜)
- Table Full Scan보다 IO를 줄일 수 있거나 정렬된 결과를 얻으려 할 때 Index Full Scan을 고려한다.

## Index Range Scan

`Index Range Scan`이란 테이블의 특정 범위의 레코드에 접근하여 스캔하는 방식이다.

- sql 조건절에서 `<, >, IS NULL, BETWEEN, IN, LIKE` 등의 연산을 이용하여 검색할 때 Index Range Scan이 일어난다.

> Index Range Scan 방식
> 

![image](https://user-images.githubusercontent.com/44316546/181248991-7f6ac7e3-c44f-4f6f-9423-40e5aea23c2b.png)

1. root node → branch node → leaf node 순서로 수직적 탐색을 한다.
    - root node, branch node는 `인덱스 칼럼 값 - leaf node로 가기 위해 거쳐야할 child node 주소값`쌍의 데이터가 존재한다.
    - leaf node에는 `인덱스 칼럼 값- 물리적 주소` 쌍의 인덱스가 저장되어 있다. 해당 칼럼 값의 물리적 주소에 접근하면  랜덤 IO가 발생하고 튜플을 검색할 수 있다.
2. leaf node에서 탐색 시작 지점을 찾고 수평적으로 스캔한다.
3. leaf node의 끝지점까지 읽었지만 스캔할 데이터가 남아있다면 link로 연결된 그 다음 leaf node로 이동하여 스캔을 이어간다
4. 스캔 종료 지점에 도달하면 읽은 데이터를 반환하고 쿼리를 종료한다

> Index Range Scan 의 단점 : 잦은 랜덤 IO
> 

leaf node에서 인덱스 칼럼값에 대응되는 물리적 주소로 데이터 파일을 읽을 때마다 `랜덤 IO`가 발생한다. 따라서 Index Range Scan 방식으로 **스캔해야 하는 데이터의 개수가 전체의 20 ~ 25%라면 Index Full Scan 방식이 효율적이다.**

<br/>
<br/>

## Hint

Hint란 SQL 문장에 특별한 키워드를 지정하여 옵티마이저에게 어떻게 데이터를 읽는 것이 효과적인지 알려주는 키워드를 의미한다.

> Hint의 사용방법
> 

`SQL 문장의 일부로 사용하는 방식`

```sql
SELECT * FROM member USE INDEX (PRIMARY) WHERE id = 1001;
```

`주석 표기 방식`

```sql
SELECT * FROM member /*! USE INDEX (PRIMARY) */ WHERE id = 1001;
```
