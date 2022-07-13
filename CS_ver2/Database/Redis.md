## Redis 
> Remote(외부) Dictionary(HashMap:Key-Value) Server(서버)
- 빠른 오픈 소스 인메모리 키-값 데이터 구조 스토어 이다. 
- String, Bitmap, Hash, List, Set, Sorted Set 등의 다양한 자료 구조를 저장할 수 있는 NoSQL 데이터베이스이다.

> Cache
Temporary Location For Speed
- 데이터의 원래 소스보다 더 빠르고 효율적으로 액세스할 수 있는 임시 데이터 저장소

>  In-memory Database
Database보다 더 빠른 Memory에 더 자주 접근하고 덜 자주 바뀌는 데이터를 저장

## Redis 사용
운영 중인 웹 서버에서 키-값 형태의 데이터 타입을 처리해야 하고, I/O가 빈번히 발생해 다른 저장 방식을 사용하면 효율이 떨어지는 경우에 사용한다. 
- `조회수 카운트 형태의 데이터` : IO작업이 빈번하게 발생하여 Redis가 아닌 다른 저장방식을 사용하면 비효율적인 경우에 Redis를 사용한다
- `사용자 세션 관리` : 사용자의 세션을 유지하려는 용도로 사용된다
- `인증 토큰 저장용` : Refresh Token 저장용
- `API 요청 결과 캐싱용`

## Redis as a cache
- 단순한 key-value 구조
- In-memory 데이터 저장소(RAM)
- 빠른 성능
    - 평균 작업속도 < 1ms
    - 초당 수백만 건의 작업 가능

### 캐싱 전략
Redis를 cache로 사용할 때 어떻게 배치하는지가 시스템의 성능에 큰 영향을 끼치기도 한다. 

> 읽기 전략
1. Look-Aside(Lazy Loading)
- 어플리케이션이나 데이터를 읽는 작업이 많을 때 사용하는 전략
- Redis를 캐시로 쓸때 가장 일반적으로 사용하는 기법
- 이 구조는 Redis가 다운되더라도 db에서 데이터를 가져올 수 있다. 
- 대신 cache로 붙어있던 커넥션이 많이 있었다면 그 커넥션이 db로 붙기 때문에 
db에 갑자기 많은 부하가 몰릴 수 있다. 
    - 이럴 때에는 미리 db에서 cache로 데이터를 넣어주는 작업을 할 수 있다.
    - 이를 `cache warining`이라고 한다. 
```
1. 애플리케이션은 데이터를 찾을때 cache에 먼저 확인
2. cache에 데이터가 있으면 cache에서 데이터를 가지고 오는 작업을 반복
3. 만약 Redis에 찾는 키가 없다면 애플리케이션은 db에 접근해서 데이터를 직접 가지고 온 뒤 다시 Redis에 저장하는 과정을 거쳐야 한다.
```

> 쓰기 전략
1. write around 전략
- db에만 데이터를 저장한다. (모든 데이터는 db에 저장)
- cache miss가 발생한 경우에 cache에 데이터를 끌어오게 된다. (읽은 데이터만 캐시에 로딩)
- cache 내의 데이터와 db 내의 데이터가 다를 수 있다는 단점이 있다. 

2. write through 전략
- db에 데이터를 저장할때 cache에도 함께 저장하는 방법이다. 
- cache는 항상 최신 정보를 가지고 있다는 장점이 있지만 저장할때마다 두단계 스텝을 거쳐야 하기 때문에 상대적으로 느리다고 볼 수 있다. 
- 저장하는 데이터가 재사용되지 않을 수도 있는데 무조건 캐시에 넣어버리기 때문에 일종의 리소스 낭비라고도 볼 수 있다.
- 따라서 이렇게 데이터를 저장할때에는 몇분 혹은 몇시간 동안만 데이터를 보관하겠다는 의미인 `expire time`을 설정해주는 것이 좋음

3. wirte Back (쓰기 작업이 많은 서비스(배치 작업)에서 사용되는 구조)
- 서버는 모든 데이터를 캐시에만 저장한다
- 일정 주기마다 캐시에 있는 데이터를 DB에 저장한다
- DB에 저장된 데이터를 캐시에서 제거한다

## Redis의 데이터 타입
- String
    - 대부분의 데이터를 문자열로 표현
- Bitmaps
- Lists
    - 링크드 리스트이다. 
    - 리스트 안의 데이터는 문자열만 가능하다. 
- Hashes
    - 해시는 필드를 가질 수 있다. 
    - 사용자 정보라는 해시가 있따면 이메일과 닉네임을 가질 수 있다.
    - 전체 혹은 개별 필드를 가져올 수 있다. 
- Sets
    - 리스트와 비슷하지만 고유 값을 저장한다는 점에서 차이가 있다. 
- Sorted Sets
- HyperLogLogS
- Streams

- user:userId:email: 문자열
- user:userId: nickname:문자열

키 값을 활용하여 사용자 아이디에 해당하는 userId만 알면 열결된 데이터인 email, nickname도 알수 있게 된다. 


### Best Practice - Counting
### String
- 단순 증감 연산
- INCR/INCRBY/INCRBYFLOAT/HINCRBY/HINCRBYFLOAT/ZINCRBY

```
redis> SET score: a 10
"OK"
redis> INCR score:a
(integer) 11
redis> INCRBY score:a 4
(integer) 15
```
score a라는 키에 10을 저장한뒤 INCR 함수를 써서 증가 시킨다. 

### Bits
- 데이터 저장공간 절약
- 정수로 된 데이터만 카운팅 가능

```
redis> SETBIT visitors:20220711 3 1
(integer) 0 
redis> SETBIT visitors:20220711 36 1
(integer) 0 
redis> BITCOUNT visitors:20220711
(integer) 2
```
오늘 서비스에 접속한 유저 수를 세고싶을 때 날짜 키를 만들어 놓고 유저 아이디에 해당하는 bit를 1로 올려준다. 
비트 카운트를 통해 1로 설정된 비트 수 확인할 수 있다. 

### HyperLogLogs
- 모든 스트링 데이터 값을 유니크하게 구분할 수 있다.
- 대용량 데이터를 카운팅 할때 적절(오차 0.81%)
- set과 비슷하지만 저장되는 용량은 매우 작은(12KB로 고정)
- 저장된 데이터는 다시 확인할 수 없음

```
redis> PFADD crawled:20220711 "https://www.google.com/"
(inter)1
redis> PFADD crawled:20220711 "https://www.redis.com/"
(inter)1

redis> PFCOUNT crawled:20220711
(integer)878423
```

ex) 웹 사이트에 방문안  ip가 몇개가 되는지, 하루 종이 크롤링한 url의 갯수가 몇개 가 되는지 
PFADD 커맨드로 값을 저장하고
PFCOUNT 커맨드로 유니크하게 저장된 값을 조회할 수 있다. 
PFMERGE 커맨드로 key들을 머지해서 사용할 수 도 있다.

### Best Practice - Messaging
### Lists
- Blocking 기능을 이용해 Event Queue로 사용

client A
```
redis> BRPOP myqueue 0 
```

client B
```
redis> LPUSH myqueue "hi"
(integer) 1
```
client A
```
1) "myqueue"
2) "hi"
```

- client A가 BRPOP 커맨드를 통해 myqueue에서 데이터를 꺼내오려고 하는데 현재 리스트 안에 데이터가 없어 대기를 하는 상황
- client B가 hi라는 값을 넣어주면 
- client A에서 바로 값을 확인할 수 있다. 


- 키가 있을 때만 데이터 저장 가능 - LPUSH/ RPUSHX
    - 이미 캐싱되어 있는 피드에만 신규 트윗을 저장
    - 트위터에서는 각 유저의 타임라인에 보일 트윗을 캐싱하기 위해 redis의 리스트를 사용하는데 이때 RPUSHX 커맨드를 사용한다.
    - 이를 이용해서 트위터를 자주 이용하던 유저의 타임라인에만 새로운 데이터를 미리 캐시해놓을 수 있으며 자주 사용하지 않는 유저는 caching key 자체가 존재하지 않는다.

### Streams
- 로그를 저장하기 가장 적절한 자료구조
- append-only (중간에 데이터가 바뀌지 않는다)
- 시간 범위로 검색/ 신규 추가 데이터 수신/ 소비자별 다른 데이터 수신(소비자 그룹)


## Redis에서 데이터를 영구 저장하려면?
redis는 in-memory 데이터 스토어
- 서버 재시작 시 모든 데이터 유실
- 복제 기능을 사용해도 사람의 실수 발생 시 데이터 복원 불가
- redis를 캐시 이외의 용도로 사용한다면 적절한 데이터 백업이 필요

### RDB(snapshot)
- 스냅샷 방식으로 동작하기 때문에 저장 당시의 메모리에 있는 데이터 그대로를 사진 찍듯 찍어서 파일로 저장한다. 

### AOF(append only file)
- 데이터를 변경하는 커맨드가 들어오면 커맨드를 그대로 모두 저장한다. 

### RDB vs AOF 선택 기준
- 백업은 필요하지만 어느 정도의 데이터 손실이 발생해도 괜찮은 경우
    - RDB 단독 사용
    - redis.conf 파일에서 SAVE 옵션을 적절히 사용 

- 장애 상황 직전까지의 모든 데이터가 보장되어야 할 경우
    - AOF 사용 
    - APPENDFSYNC 옵션이 everysec인 경우 최대 1초 사이의 데이터 유실 가능(기본 설정)

- 제일 강력한 내구성이 필요한 경우
    - RDB & AOF 동시 사용

## redis 주의사항
- 하나의 컬렉션(Collection)에 너무 많은 아이템을 담으면 좋지 않다.
- key 조회 비용을 생각하여 너무 길게 사용하는 것은 좋지 않다. 
- 메모리 관리가 필수
    - 가용 메모리보다 큰 데이터를 저장한다면 swapping이 발생한다. 사이즈가 큰 데이터에 접근한다면 page swap in, out이 발생하여 검색 속도에 영향이 미친다.
    - 다양한 사이즈의 데이터를 저장하기 때문에 메모리 파편화 문제가 발생할 수 있다. → 가급적 비슷한 사이즈의 데이터를 저장하는게 바람직하다
- Expire 설정하기
    - 저장 공간이 가득찬 경우 사용자가 직접 삭제될 데이터 기준을 설정하는게 바람직하다 → 데이터의 사용 기한 Expire 를 설정하여 key에 대한 timeout(만료 시간)을 지정해준다. 만료 시간이 경과하면 key가 자동으로 삭제된다.
- O(n) 명령어 피하기
    - Redis는 싱글 스레드 방식으로 연산을 수행한다. 따라서 요청 처리 시간이 오래 걸리면 뒤에 오는 나머지 요청들은 계속 대기해야 한다.
