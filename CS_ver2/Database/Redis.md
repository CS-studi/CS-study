## Redis 
### Remote(외부) Dictionary(HashMap:Key-Value) Server(서버)


> Cache
Temporary Location For Speed
- 데이터의 원래 소스보다 더 빠르고 효율적으로 액세스할 수 있는 임시 데이터 저장소

>  In-memory Database
Database보다 더 빠른 Memory에 더 자주 접근하고 덜 자주 바뀌는 데이터를 저장하자

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
- cache miss가 발생한 경우에 cache에 데이터를 끌어오게 된다. 
- cache 내의 데이터와 db 내의 데이터가 다를 수 있다는 단점이 있다. 

2. write through 전략
- db에 데이터를 저장할때 cache에도 함께 저장하는 방법이다. 
- cache는 항상 최신 정보를 가지고 있다는 장점이 있지만 저장할때마다 두단계 스텝을 거쳐야 하기 때문에 상대적으로 느리다고 볼 수 있다. 
- 저장하는 데이터가 재사용되지 않을 수도 있는데 무조건 캐시에 넣어버리기 때문에 일종의 리소스 낭비라고도 볼 수 있다.
- 따라서 이렇게 데이터를 저장할때에는 몇분 혹은 몇시간 동안만 데이터를 보관하겠다는 의미인 `expire time`을 설정해주는 것이 좋음


## Redis의 데이터 타입
- String
- Bitmaps
- Lists
- Hashes
- Sets
- Sorted Sets
- HyperLogLogS
- Streams

### Best Practice - Counting
String
- 단순 증감 연산
- INCR/INCRBY/INCRBYFLOAT/HINCRBY/HINCRBYFLOAT/ZINCRBY

```
redis> SET score: a 10
"OK"
redis> INCR score:a
(integer) 11
redis> INCR score:a 4
(integer) 15
```
score a라는 키에 10을 저장한뒤 INCR 함수를 써서 증가 시킨다. 


### Collection