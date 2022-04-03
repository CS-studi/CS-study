# Deadlock

## 데드락

`데드락(교착상태)`란 일련의 프로세스들이 각자 자원을 점유하고 있는 상태에서 서로가 서로의 자원에 접근하기 위해 blocking된 상태(무한정 대기 상태)이다.

- 자원은 하드웨어, 소프트웨어 자원을 포함하는 개념이다. (IO device, CPU cycle, Memory Space, Semaphore 등)

## 데드락 발생 4가지 조건

1. `상호 배제(Mutual Exclusion)` : 자원은 매 순간 하나의 프로세스만 사용할 수 있어야 한다. (독점)
2. `비선점(No Preemption)` : 프로세스는 자원을 스스로 반납할 뿐 강제로 빼앗기지 않는다.
3. `점유 대기(Hold and Wait)` : 자원을 가진 프로세스가 이를 반납하지 않고 또 다른 자원을 기다릴 수 있다.
4. `순환 대기(Circular Wait)` : 자원을 기다리는 프로세스들 간에 사이클이 형성된다. (p0는 p1이 가진 자원을 기다리고, p1은 p2가 가진 자원을 기다리고, ... , pn은 p0가 가진 자원을 기다린다)

## 데드락 처리 방법

1. `Deadlock Prevention` (데드락 예방)
2. `Deadlock Avoidance` (데드락 회피)
3. `Deadlock Detection and recovery` (데드락 감지 및 회복)
4. `Deadlock Ignorance` (데드락 무시)

### 📌 `1. Deadlock Prevention`

데드락 발생 4가지 조건 중 하나를 제거함으로 데드락을 해결하는 방법

1. `상호 배제 부정` : 여러 프로세스가 공유 자원을 사용하도록 한다.
    - 단, 공유해서는 안되는 자원은 상호 배제 조건을 만족시켜야 한다.
2. `비선점 부정` : 보유 자원을 가진 프로세스가 다른 자원을 기다리는 경우, 해당 보유 자원이 다른 프로세스에 의해 선점되도록 한다.
    - 단, 프로세가 상태를 save & restore할 수 있는 자원만 선점되도록 해야 한다
3. `보유 대기 부정` : 프로세스가 자원을 요청할 때 다른 어떤 자원도 가지고 있지 않아야 한다.
    - 방법1 : 프로세스 시작 시에 해당 프로세스가 필요한 “모든” 자원을 할당받게 하는 방법 (비효율적)
    - 방법2 : 자원이 필요한 경우 보유 자원을 모두 놓고 대기하는 방법
4. `순환 대기 부정` : 모든 자원 유형에 할당 순서를 정하여 순서대로만 자원을 할당하도록 한다.
    - 프로세스가 순서가 3인 자원을 보유하고 있을 때, 순서가 1인 자원을 할당받으려면 순서가 3인 자원을 반납해야 한다. (1→ 3의 순서)
    

**예방 방법을 사용하게 될 때 발생하는 문제점 ⇒ utilization 저하(system 입장), throughput 감소(system 입장), starvation 문제(process 입장)**

### 📌 `2. Deadlock Avoidance`

데드락의 발생 가능성이 없는 경우에만 자원을 할당하는 방법. 

- 특정 자원을 할당해주어도 시스템이 여전히 safe state한지 확인하므로써 데드락의 가능성을 확인할 수 있다.

```java
1. 프로세스가 자원a를 요구한다.
2. 시스템은 자원a를 할당해줄 경우, 시스템의 상태가 safe한지 확인한다.
	2-1. safe하다면 자원a를 할당해준다.
	2-2. unsafe하다면 다른 프로세스들이 자원을 반환할 때까지 자원a를 할당해주지 않는다.
```

> safe state란
> 

시스템 내의 프로세스들이 safe sequence를 이루는 상태이다.

- 시스템이 safe state하다면 데드락이 발생하지 않는다.
- 시스템이 unsafe state하다면 데드락이 발생한다.

> safe sequence란
> 

프로세스의 sequence<P1, P2 ,... Pn>가 safe한 경우 해당 프로세스 sequnce를 “safe sequence하다”라고 말한다.

- safe sequence를 만족하려면 프로세스 Pi(1 ≤ i ≤ n)의 자원 요청이 `가용 자원 + 모든 프로세스 Pj (j < i)의 보유 자원` 보다 적어야 한다
- 다시 말해, `Pn 프로세스가 현재 필요한 자원` < `전체 가용자원 + P1 ~ Pn-1 프로세스가 나중에 반납할 자원` 을 만족하는 프로세스 sequence를 “safe sequence”라고 할 수 있다.

> Deadlock Avoidance 알고리즘
> 
1. `자원 유형 당 인스턴스가 1개`인 경우 ⇒ `Resource Allocation Graph 알고리즘`을 사용한다.
    
    ![https://raw.githubusercontent.com/CS-studi/CS-study/master/CS/OS/img/deadlock/RAG.png](https://raw.githubusercontent.com/CS-studi/CS-study/master/CS/OS/img/deadlock/RAG.png)
    
    `화살표 의미`
    
    - 점선 화살표(claim edge) : 프로세스가 적어도 한 번 이상 자원을 사용한다는 의미
    - 프로세스 → 자원으로 가는 실선 화살표(request edge) : 현재 프로세스가 자원을 요구하는 상태
    - 자원 → 프로세스로 가는 실선 화살표(asignment edge) : 자원이 프로세스에게 할당된 상태
    
    `시간 복잡도`
    
    - cycle 생성 여부 조사시 프로세스의 수가 n일 때 O(n^2) 시간이 걸린다.
    
    가장 오른쪽 그림을 보면 Cycle이 형성되어있지만 데드락이 발생한건 아니다. 만약 P1이 R2를 요구한다면 점선이 실선으로 바뀌고 데드락이 형성된다. 즉, P1에게 자원 R2를 할당해주면 시스템이 unsafe한 상태가 되므로 할당해주지 않는다.
    
2. `자원 유형 당 인스턴스가 여러 개`인 경우 ⇒ `Banker’s 알고리즘`을 사용한다
    
    ![https://raw.githubusercontent.com/CS-studi/CS-study/master/CS/OS/img/deadlock/bankers.png](https://raw.githubusercontent.com/CS-studi/CS-study/master/CS/OS/img/deadlock/bankers.png)
    
    `Banker's 알고리즘의 가정`
    
    - 모든 프로세스는 최대로 사용할 자원의 양을 미리 명시한다 (Max)
    - 프로세스가 자원을 할당받을 경우 유한 시간내에 다시 반납한다
    
    `Banker's 알고리즘 방식`
    
    - 시스템이 safe state를 유지할 경우에만 자원을 할당해준다.
        - `프로세스의 총 요청 자원의 수(Need) > 시스템 속 가용 자원의 수(Available)` 하다면 unsafe한 상태이다. → 해당 프로세스에게 자원을 할당해주지 않는다.
        - `프로세스의 총 요청 자원의 수(Need) ≤ 시스템 속 가용 자원의 수(Available)` 하다면 safe한 상태이다. → 해당 프로세스에게 Need만큼의 자원을 할당해준다.
    
    Banker’s 알고리즘은 프로세스가 자원을 최대로 요청한 자원이 가용자원으로 충족되지 않는다면 자원을 할당해주지 않는 보수적인 알고리즘이다.
    
### 📌 `3. Deadlock Detection and Recovery`

데드락이 발생하는 것을 미연에 방지하지는 않지만, detection 루틴으로 데드락을 감지한다. 만약 감지되었다면 recovery한다.

> Deadlock Detection 알고리즘
> 
1. `자원 유형 당 인스턴스가 1개`인 경우 ⇒ `Wait-for Graph 알고리즘`을 사용한다.
    
    ![https://raw.githubusercontent.com/CS-studi/CS-study/master/CS/OS/img/deadlock/Wfg.png](https://raw.githubusercontent.com/CS-studi/CS-study/master/CS/OS/img/deadlock/Wfg.png)
    
    `시간 복잡도`
    
    - cycle 생성 여부 조사시 프로세스의 수가 n일 때 O(n^2) 시간이 걸린다.
    
    Resource Allocation Graph에서 자원을 빼버리고 실선을 이으면 Waif-for graph가 된다.
    
2. `자원 유형 당 인스턴스가 여러 개`인 경우 ⇒ `Banker’s 알고리즘과 유사한 방법`을 활용한다.
    
    ![https://raw.githubusercontent.com/CS-studi/CS-study/master/CS/OS/img/deadlock/bdetect.png](https://raw.githubusercontent.com/CS-studi/CS-study/master/CS/OS/img/deadlock/bdetect.png)
    
    `Detection에서 Banker's 알고리즘 방식`
    
    - 프로세스가 자원을 요청하면 바로 할당해준다 (Max, Need를 알 필요 없음)
    - 프로세스가 보유하고 있는 자원을 언젠가 반납할 것이라고 “낙관적”으로 판단 한다.
        - `프로세스의 요청 자원 (Request) ≤ 시스템 내 가용 자원 + 나머지 프로세스들의 보유 자원(Allocation)` 이라면 safe한 상태
    
    현재 시스템 내 가용 자원(Available)이 (0,0,0)이지만 프로세스들이 보유하고 있는 자원(Allocation)이 언젠가 반환될 것이라고 판단하기 때문에 데드락이라고 판단하지 않는다. 
    
    (모르겠음.. 스터디원 찬스!)
    
    | 프로세스 | Request | Allocation + Avaliable | state |
    | --- | --- | --- | --- |
    | P0 | (0, 0, 0) |  | safe |
    | P1 | (2, 0, 2) |  | safe |
    | P2 | (0, 0, 0) |  | safe |
    | P3 | (1, 0, 0) |  | safe |
    | P4 | (0, 0, 2) |  | safe |

> Deadlock Recovery 방법
> 
1. `Process Termination 프로세스 종료`
    - 방법1 : 모든 데드락 상태인 프로세스를 강제 종료
    - 방법2 : 데드락 사이클이 없어질 때까지 하나씩 프로세스를 강제 종료
2. `Resource Preemption 자원 선점`
    - 비용을 최소화할 victim 프로세스를 선정
    - safe state로 롤백하여 프로세스를 재시작
    - Starvation 문제 (자세히 다루지 않음)
        - 동일한 프로세스가 계속 victim 프로세스로 선정되는 경우
        - 비용 요소에 롤백 횟수도 같이 고려해야 함 (단순히 비용만 최소화하면 안 된다는 뜻)

### 📌 `4. Deadlock Ignorance`

데드락이 일어나지 않는다고 생각하고 아무런 조치도 취하지 않음

- 데드락이 매우 드물게 발생하므로 데드락에 대한 조치 자체가 더 큰 오버헤드일 수 있음
- 만약, 시스템에 데드락이 발생한 경우 시스템이 비정상적으로 작동하는 것을 사람이 느낀 후 직접 프로세스를 죽이는 등의 방법으로 대처함
- UNIX, Windows 등 **대부분의 OS에서 채택하였음**