# RDBMS vs NoSQL

## 용어

`Database` :  필요한 정보를 얻기 위해 논리적으로 연관된 데이터를 모아 구조적으로 통합해 놓은 것

`DBMS(Database Management System)` : 데이터베이스 관리 시스템으로 사용자와 데이터베이스를 연결해주는 소프트웨어

`SQL(Structured Query Language)` : 구조적 질의어로 데이터베이스를 제어하고 관리하는 언어

`Scaling Out` : 수평적 확장으로 서버를 증설하는 것

`Scaling Up` : 수직적 확장으로 서버의 성능을 향상시키는 것

## CAP 이론

네트워크로 연결된 분산 데이터베이스 시스템은 `일관성(Consistency)`, `가용성(Availability)`, `분할 내구성(Partition Tolerance)` 이라는 3가지 특성 중 2가지 특성만 만족할 수 있다는 이론

- 3가지 특성을 모두 만족할 수 없다

> 3가지 특성
> 
1. `일관성 (Consistency)` : 여러 클라이언트에서 같은 시점에 조회하는 데이터는 항상 동일한 값임을 보장하는 특성
    - 데이터 일관성을 보장하기 위해서 Transaction과 같은 매커니즘이 필요하다
    - RDBMS에서는 기본적으로 지원하지만, NoSQL은 기본적으로 지원하지 않는다
    - NoSQL은 `최종 일관성(궁극적 일관성)` 을 지원한다. (궁극적 일관성이란 일관성을 완전히 보장하지 않지만, 궁극적으로 언젠가는 데이터의 일관성을 보장됨을 의미한다)
        - `NoSQL의 동기식 일관성 유지법`
            - 데이터가 업데이트 될 경우, 클라이언트에 응답하기 전에 모든 서버에 존재하는 데이터에 변경사항을 반영한 뒤에 응답하는 방법
            - 응답시간이 느리지만 데이터 정합성을 보장한다
        - `NoSQL의 비동기식 일관성 유지법`
            - 데이터가 업데이트 될 경우, 변경사항을 임시 파일에 기록하고 클라이언트에게 응답한 뒤에 변경사항을 모든 서버에 반영하는 방법
            - 응답시간이 빠르지만 변경사항을 반영하는 서버에 장애학 발생할 경우 데이터가 손실될 수 있다
2. `가용성 (Availability)` : 모든 클라이언트의 Read/Write 요청에 대해 항상 응답 가능해야한다는 특성
    - 특정 노드에 장애가 발생해도 성공적으로 서비스를 지속하는 것
    - 데이터 저장소에 대한 모든 동작(read, write 등) 항상 성공적으로 리턴되어야한다.
    - NoSQL은 여러 서버에 데이터 복제(Replication)를 하여 가용성을 보장한다
    - Replication 방식에는 Master-Slave, Peer-to-Peer 방식이 있다
3. `분할 내구성(Partition Tolerance)` : 지역적으로 분할된 네트워크 환경에서 동작할 때, 두 지역간 네트워크가 단절되거나 네트워크 데이터 유실이 일어나도 각각의 지역 내의 DB 시스템은 정상동작해야 한다는 특성

> 조합
> 
1. `CA : 일관성 + 가용성` → RDBMS
    - 시스템이 다운되어도 데이터 손실을 방지하고, 일관성을 유지하는 데이터베이스 시스템
2. `CP : 일관성 : 분할 내구성` → NoSQL
    - 모든 Read/Write 연산에 대해 데이터 정합성을 유지하는 데이터베이스 시스템
    - 데이터보다는 성능이 중요한 시스템에 적합하다
3. `AP : 가용성 + 분할 내구성` → NoSQL
    - 궁극적 일관성(Eventual Consistency)을 보장하는 시스템에 적합하다
    - 일관성을 요구하지 않는 시스템에 적합하다

## RDBMS

`RDBMS(Relational Database Management System)` 이란 관계형 데이터베이스 관리 시스템이다.

> 특징
> 
- `Relational` : 데이터는 관계를 가지고 여러 테이블에 분산되어 저장된다. (`관계` 라는 개념으로 데이터 중복을 피할 수 있다)
- `Schema` : RDBMS에 데이터는 정해진 스키마에 따라 테이블에 저장된다
- RDBMS는 NoSQL 데이터베이스보다 Scaling Out이 번거롭다 (관계를 맺는 데이터를 서로 다른 DB서버에 분산시키기 어렵다)
- `데이터의 일관성`을 보장한다 (트랜잭션을 통해) = 데이터의 ACID 속성을 제공한다
- RDBMS는 `CA(Consistency + Avaliablity) 시스템`이다. (네트워크 분할 내구성 P을 보장하지 않기 때문에 Scaling Out을 통한 서버 Clustering 구조에 부적합하다)

> 장단점
> 

`장점`

- `관계` 라는 개념을 통해 데이터를 중복없이 저장할 수 있다.
- 데이터 변경이 용이하다 (데이터 일관성을 지키는 `관계` 개념 덕분에)
- 명확히 정의된 스키마 덕분에 데이터 무결성을 보장한다

`단점`

- NoSQL에 비해 DB 서버의 Scaling Out이 번거롭다. Scaling Up으로 서버 성능을 향상할 수 밖에 없다
- 스키마 때문에 저장할 수 있는 데이터의 형태가 제한된다. (때에 때라 장점이 될 수도)
- 테이블 관계가 복잡해 질수록 JOIN 연산의 수행시간이 길어진다.

## NoSQL

`NoSQL(Not Only SQL)` 이란 비관계형 데이터베이스들을 칭하는 단어이다.

> NoSQL이 나타난 배경
> 

빅데이터의 등장으로 DB 서버로의 트래픽 증가 

→ RDB의 성능 향상 필요성 대두 

→ Scaling Out은 RDB 특성상 부적절, Scaling Up은 비용 문제가 큼

→ 데이터 일관성은 포기하되 Scaling Out이 가능한 NoSQL이 등장하게 됨

> 특징
> 
- `Not Relational` : 저장되는 데이터에 관계라는 개념이 존재하지 않는다.
- `No Schema` : 데이터에 대한 스키마가 없다 (서로 다른 구조의 데이터를 같은 컬렉션에 저장할 수 있다.)
- 서버의 Scaling Up, Scaling Out 방식을 지원한다
- 데이터 일관성을 완벽하게 보장하지 못한다. (`궁극적 일관성`) = 완화된 데이터의 ACID 속성을 제공한다
- `분산형 구조` : 데이터를 여러 대의 서버에 복제하여 분산 저장한다. (특정 DB 서버에 장애가 발생해도 무중단 서비스 제공 가능)
- NoSQL은 `CP(Consistency + Partition Tolerance) 또는 AP(Avaliablity + Partition Tolerance) 시스템`이다.

> 장단점
> 

`장점`

- 스키마가 없어서 저장되는 데이터 구조에 대한 제약이 비교적 없다
- 어플리케이션이 필요로 하는 형식으로 데이터를 저장할 수 있다
- Read 연산 속도가 RDB보다 빠르다
- Scaling Out이 가능하다.

`단점`

- 저장되는 데이터 구조를 직접 결정해야 한다
- 데이터 중복 문제가 심하다 (데이터 업데이트 시, 여러 collection의 중복된 데이터를 찾아 갱신해야 한다)

> NoSQL의 데이터 저장 방식
> 
1. `Key-Value 방식` : key를 데이터의 고유 식별자로 사용하여 key-value 쌍의 데이터를 저장한다.
    - Partitioning, Scaling out이 가능하다
    - Redis, Riak, Voldmort
2. `Document 방식` : 데이터를 document라는 타입으로 저장한다
    - document는 XML, JSON, YAML과 같은 형식으로 JSON을 많이 사용한다.
    - document들을 collection으로 관리한다. collection에는 document의 id가 저장되어 있고, O(1) 시간복자도로 document를 조회할 수 있다
    - Write보다 Read연산이 많은 서비스에 적합하다
    - MongoDB, CouchDB
3. `Column Model 방식` : 하나의 key에 `Column 이름 - Column 값` 쌍의 데이터를 저장하고 조회한다
    - Column은 `Column 이름, Column 값, Timestamp` 로 구성된다
    - Read보다 Write 연산이 많은 서비스에 적합하다
    - 구글의 Big Table
4. `Graph 방식` : 데이터를 노드로 표현하고 노드 사이의 관계를 화살표로 표현하는 방식
    - 데이터들의 관계를 중요시한다.
    - Sones, Allegro Graph

## RDBMS vs NoSQL

> 용어 비교 ([참고](https://aws.amazon.com/ko/nosql/))
> 

| SQL | MongoDB | DynamoDB | Cassandra | Couchbase |
| --- | --- | --- | --- | --- |
| 테이블 | 컬렉션 | 테이블 | 테이블 | 데이터 버킷 |
| 열 | 문서 | 항목 | 열 | 문서 |
| 컬럼 | 필드 | 속성 | 컬럼 | 필드 |
| 기본 키 | ObjectId | 기본 키 | 기본 키 | 문서 ID |
| 인덱스 | 인덱스 | 보조 인덱스 | 인덱스 | 인덱스 |
| 보기 | 보기 | 글로벌 보조 인덱스 | 구체화된 보기 | 보기 |
| 중첩된 테이블 또는 객체 | 포함 문서 | 맵 | 맵 | 맵 |
| 배열 | 배열 | 목록 | 목록 | 목록 |

> RDBMS가 적합한 경우
> 
- 데이터 구조가 변경될 여지가 없고, 저장될 데이터의 규칙이 중요한 경우
- 관계를 맺고 있는 데이터가 자주 변경되는 경우(NoSQL의 경우 여러 컬렉션에 중복 저장된 데이터를 하나씩 찾아 업데이트 해줘야 한다)

> NoSQL이 적합한 경우
> 
- 저장되는 데이터의 구조를 정확히 알 수 없거나 자주 변경되고 확장될 경우
- Read 연산이 많고 Write 연산이 드문경우
- 막대한 양의 데이터를 다루기 때문에 서버의 Scaling Out이 불가피할 경우
