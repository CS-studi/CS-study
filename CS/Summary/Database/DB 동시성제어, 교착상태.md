# DB 동시성 제어 & 교착상태

## DB : 동시성 제어

`DB 트랜잭션의 동시성 제어` 란 여러 트랜잭션이 작업을 성공적으로 마칠 수 있도록 트랜잭션의 실행 순서를 제어하는 기법이다.

> 동시성 제어가 필요한 이유
> 

`동시성`이 높을수록 DB의 성능이 좋다. 하지만 `데이터 일관성`이 깨질 수 있기 때문에 동시성 제어가 필요하다.

Trade-Off관계에 있는 동시성과 데이터 일관성은 격리수준으로 제어할 수 있다.

> 트랜잭션간에 동시성 제어를 하지 않을 경우 발생하는 문제점
> 
- `Lost Update`
    
    하나의 트랜잭션이 갱신한 내용을 다른 트랜잭션이 덮어씀으로써 갱신이 무효화되는 문제
    
    ```java
    1. 트랜잭션1이 데이터a를 갱신
    2. 트랜잭션2가 데이터a를 갱신
    
    결국 트랜잭션1의 갱신 결과는 무효화되고 데이터a는 트랜잭션2의 결과로 덮혀씌워진다.
    ```
    
- `Inconsistency`
    
    데이터에 update 연산이 수행되는 도중 한 트랜잭션은 update 전의 데이터를 읽고, 다른 트랜잭션은 update 이후의 데이터를 읽어서 발생하는 데이터 불일치 문제
    
    ```java
    1. 트랜잭션1이 데이터a를 읽음 // 1
    2. 트랜잭션2가 데이터a를 2로 갱신 하고 commit함 // 2
    3. 트랜잭션3이 데이터a를 읽음 // 2
    
    트랜잭션 1과 3이 읽은 데이터a의 값이 서로 다르다(불일치)
    ```
    
- `Cascading Rollback`
    
    트랜잭션1이 데이터를 update하는 중간에 트랜잭션2가 해당 데이터를 읽고 commit한 경우, 트랜잭션1이 rollback한다면 트랜잭션2는 rollback하지 못하는 문제(이미 commit되었기 때문)
    
    ```java
    1. 트랜잭션1이 데이터a를 갱신하고 있음
    2. 트랜잭션2가 갱신되는 데이터a를 읽고 작업을 수행한 뒤 commit
    3. 트랜잭션1이 비정상 종료되어 rollback
    
    트랜잭션2는 결과적으로 잘못된 데이터a로 작업을 수행했지만 commit되었기 때문에 rollback하지 못한다.
    ```
    
- `Incorrect Summary`
    
    특정 레코드에 집계함수를 적용하는 동안 다른 트랜잭션이 집계 예정인 데이터를 수정하여 부정확한 집계 결과를 얻는 문제
    
    ```java
    1. 트랜잭션1이 데이터 a,b,c를 sum하고 있음 //예상 결과 : a+b+c
    2. 트랜잭션2가 데이터b를 k로 update
    3. 트랜잭션1이 sum 연산을 마침 // 실제 결과 : a+k+c
    ```
    
- `Dirty Read`⭐
    
    커밋되지 않는 트랜잭션의 데이터 변경사항을 볼 경우, 무효가 된 데이터를 읽어 발생하는 문제 (Inconsistency와 다름)
    
    ```java
    1. 트랜잭션1이 데이터a를 갱신함
    2. 트랜잭션2가 "갱신된 데이터a"를 읽음
    3. 트랜잭션1이 rollback되어 갱신이 무효화됨
    
    트랜잭션2는 무효화된 "갱신된 데이터a"를 읽게 된다.
    ```
    
- `Non-Repeatable Read`⭐
    
    한 트랜잭션에서 같은 쿼리를 두번 수행할 때, 동일한 결과가 나오지 않는 문제
    
    ```java
    1. 트랜잭션1이 데이터a를 읽음 -> "데이터a : 1"
    2. 트랜잭션2가 데이터a를 갱신함 -> "데이터a : 2"
    3. 트랜잭션1이 다시 데이터a를 읽음 -> "데이터a : 2" // 이전 결과와 다르다
    ```
    
- `Phantom Read`⭐
    
    일정 범위의 레코드를 두번 이상 읽었을 때, 이전에 없던 레코드가 나타나는 현상
    
    ```java
    1. 트랜잭션1이 where 구문으로 특정 범위의 데이터를 읽음 -> "데이터a : 1"
    2. 트랜잭션2가 데이터b를 추가함
    3. 트랜잭션1이 다시 특정 범위의 데이터를 읽음 -> "데이터a : 1, 데이터b : 2" // phatom data 등장!
    ```
    
- `Update 부정합`
    
    ```sql
    START TRANSACTION; -- transaction id : 1
    SELECT * FROM Member WHERE name='hjoon';
    
        START TRANSACTION; -- transaction id : 2
        SELECT * FROM Member WHERE name = 'hjoon';
        UPDATE Member SET name = 'top' WHERE name = 'hjoon';
        COMMIT;
    
    UPDATE Member SET name = 'sun' WHERE name = 'hjoon'; -- 0 row(s) affected
    COMMIT;
    ```
    
    트랜잭션은 update 구문을 실행할 때 실제 레코드가 아닌 Undo 세그먼트 영역에서 실행한다.
    
    1. 트랜잭션1이 `name=hjoon` 인 행을 읽는다. → `ok`
    2. 트랜잭션2가 name=hjoon인 행의 name 속성을 top으로 변경하고 commit한다
        - undo 세그먼트 영역의 데이터에 x-lock을 걸 수 없다. → 따라서 실제 레코드에 x-lock을 건다.
        - 변경을 마치고 commit되면 실제 레코드에 변경 사항이 반영된다
        - → `ok`(실제 레코드에 name=hjoon인 행은 없고 name=sun인 행만 존재한다.)
    3. 트랜잭션1이 `name=hjoon`인 행의 name 속성을 sum으로 변경하려 한다. 
        - undo 세그먼트 영역의 데이터에 x-lock을 걸 수 없다 → 실제 레코드에 x-lock을 걸려고 시도한다.
        - **하지만 실제 레코드에 name=hjoon인 행이 존재하지 않는다.**
        - → `0 row(s) affected`(실제 레코드에 아무런 변경이 일어나지 않음)

<br/>

> DB 트랜잭션의 동시성 제어 방법
> 
- `Locking`⭐ : 트랜잭션이 데이터에 Lock을 설정하면 다른 트랜잭션은 Lock이 걸린 데이터가 Unlock될 때까지 접근, 수정, 삭제하지 못하도록 하는 제어 기법
- `Timestamp` : Timestamp(시스템에서 생성되는 고유 번호)를 트랜잭션에 부여하여 트랜잭션 접근 순서를 미리 정하는 제어 기법
    - 먼저 들어온(Timestamp가 더 빠른) 트랜잭션이 먼저 수행됨
- `Validation 검증` : 트랜잭션을 먼저 수행하고 트랜잭션이 종료될 때 적합성을 검증하여 DB에 최종 반영하는 제어 기법
    - 각 트랜잭션은 메모리 상의 DB 복사본에서 연산을 수행하고 종료 시에 적합성 검증 후 DB에 최종 반영됨

<br/>

## DB : Lock

> DB Lock의 종류
> 
- `Shared Lock(S-Lock)` : 데이터에 읽기 연산을 수행할 때 설정하는 Lock
    - **`"S-Lock이 걸린 데이터에 다른 트랜잭션이 S-Lock을 걸 수 있고, X-Lock은 걸 수 없다"`**
    - Select 쿼리를 수행하는 트랜잭션이 데이터에 S-Lock을 걸면 다른 트랜잭션들은 데이터를 읽을(Select) 순 있지만 변경(Update, Delete)할 수 없다.
- `Exclusive Lock(X-Lock)` : 데이터에 쓰기 연산을 수행할 때 설정하는 Lock
    - **`"X-Lock이 걸린 데이터에 다른 트랜잭션은 S-Lock, X-Lock을 걸 수 없다"`**
    - 하나의 트랜잭션이 데이터에 X-Lock을 걸면, 트랜잭션이 종료될 때까지 X-Lock이 유지된다.
    - Update, Delete쿼리를 수행하는 트랜잭션이 데이터에 X-Lock을 걸면 다른 트랜잭션들은 데이터를 읽을 수도 변경할 수도 없다

> DB Lock의 단위
> 
1. `Row Level` : 테이블 행에 Lock을 설정
2. `Column Level` : 테이블 열에 Lock을 설정(Column Lock 해제 시 많은 리소스가 소모되어 잘 사용 안됨)
3. `Page Level` : 같은 Page에 존재하는 모든 Row에 Lock을 설정(Page : SQL Server의 기본 IO 단위)
4. `Table Level` : Table과 Index에 Lock을 설정 (DDL Lock이라고 불리기도 함)
5. `Database Level` : 데이터베이스 복구, 스키마 변경시 설정되는 Lock

<br/>

## DB : Blocking

DB에서 하나의 데이터에 Lock을 걸려는 트랜잭션간에 Race Condition이 발생한 경우, 특정 트랜잭션이 작업을 진행하지 못하게 막은 상태를 `Blocking` 이라고 한다.

- 데이터의 고유성을 보장하기 위해 Blocking으로 트랜잭션의 개입을 잠시 막는다.

> DB Blocking 예시
> 
- S-Lock이 설정된 데이터를 Read하려는 트랜잭션은 **Blocking 되지 않는다.** (`S-Lock 👈 S-Lock`)
    
    ```java
    1. 트랜잭션1이 데이터a를 읽고 있음 // 데이터a에 S-Lock이 걸림
    2. 트랜잭션2는 데이터a를 읽기 위해 S-Lock을 걸려 함 // 걸 수 있음
    3. 트랜잭션2는 데이터a를 읽음
    ```

- S-Lock이 설정된 데이터를 Update, Delete하려는 트랜잭션은 **Blocking 된다.** (`S-Lock 👈 X-Lock`)
    
    ```java
    1. 트랜잭션1이 데이터a를 읽고 있음 // 데이터a에 S-Lock이 걸림
    2. 트랜잭션2는 데이터a를 변경하기 위해 X-Lock을 걸려 함 // 걸 수 없음
    3. 트랜잭션2는 Blocking 됨 // 트랜잭션1이 S-Lock을 해제할 때까지
    ```

- X-Lock이 설정된 데이터를 Read, Update, Delete하려는 트랜잭션은 **Blocking 된다.** (`X-Lock 👈 S-Lock 또는 X-Lock 👈 X-Lock`)
    
    ```java
    1. 트랜잭션1이 데이터a를 변경하고 있음 // 데이터a에 X-Lock이 걸림
    
    2-1-1. 트랜잭션2가 데이터a를 읽기 위해 S-Lock을 걸려 함 // 걸 수 없음
    2-1-2. 트랜잭션2는 Blocking 됨 // 트랜잭션1이 X-Lock을 해제할 때까지
    
    2-1-1. 트랜잭션2가 데이터a를 변경하기 위해 X-Lock을 걸려 함 // 걸 수 없음
    2-1-2. 트랜잭션2는 Blocking 됨 // 트랜잭션1이 X-Lock을 해제할 때까지
    ```
    

> DB Blocking을 피하는 방법
> 
- 트랜잭션 수행 시간을 짧게 설정하여 Race Condition 발생 가능성을 낮춘다.
- 적절한 쿼리 튜닝을 한다. (쿼리 수행시간이 길면 트랜잭션 수행 시간도 길어지기 떄문)
- 동일한 데이터를 변경하는 트랜잭션이 동시에 수행되지 않도록 설계한다
- 트랜잭션 격리수준을 필요이상으로 상향 조정하지 않는다.

<br/>

## DB : Dead Lock

`DB 트랜잭션 간에 발생하는 교착 상태`란 서로 다른 두 트랜잭션이 각자 데이터에 Lock을 걸어두고, 서로의 데이터에 접근하기 위해 Unlock될 때까지 무한정 대기하는 상태이다.

> 예시
> 

```java
1. 트랜잭션1이 데이터a에 X-Lock을 걸어둠
2. 트랜잭션2가 데이터b에 X-Lock을 걸어둠
3. 트랜잭션1은 데이터b에 접근하려하고, 트랜잭션2는 데이터a에 접근하려 함
```

트랜잭션1과 2는 각자 접근하려는 데이터가 Unlock되기를 기다리기 때문에 Blocking 상태가 된다. 결국 트랜잭션1과 2는 다음 연산(쿼리)을 진행하지 못한 채 무한정 대기 상태에 빠진다.

> 참고 예시

```java
1. 트랜잭션1이 데이터a에 S-Lock을 걸어두고 sleep 상태가 됨
2. 트랜잭션2가 데이터a에 X-Lock을 걸려고 함
```

트랜잭션2는 트랜잭션1이 S-Lock을 해제할 때까지 무한정 대기 해야함

> DB Dead Lock 해결 방안
> 
1. DB Dead Lock이 감지되면 둘 중 하나의 트랜잭션을 강제로 종료시킨다.
2. 트랜잭션간의 접근 순서 규칙을 정한다.

<br/>

## DB Blocking vs DB Dead Lock

| DB Blocking | DB Dead Lock |
| --- | --- |
| Blocking된 트랜잭션은 목적 데이터에 Lock이 풀리면 Blocking 상태에서 벗어난다. | 두 트랜잭션이 서로에 의해 Blocking 되었으므로 그 누구도 먼저 Blocking 상태에서 벗어나지 못한다. 결국 무한정 대기 상태에 빠진다. |
