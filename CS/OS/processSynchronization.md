# 🫁 Process Synchronization

> Intro

> 동기화(Synchronization)

> Race Conditions

> Critical Region

> 해결책

>> Lock

>> Semaphores

>> Monitor

## 🫁 Intro

### Process란 무엇인가?
> 프로세스란 실행 중인 프로그램이다.

프로그램을 수행하기 위해서 Processor(CPU)가 필요하다. 운영체제는 사용자에게 동시에 여러개의 프로세스가 실행되고 있다는 착각을 일으킨다. 사실 CPU는 process to process로 switch를 하면서 여러개의 프로그램을 수행하는 것이다.

각각의 프로세스는 address space(core image), process table entry를 가진다.

### Process의 세가지 상태
1. Running,실행
    - 실제로 CPU를 사용하고 있는 상태
2. Ready,준비
    - Runnable, 일시적으로 다른 프로세스가 실행되도록 멈춰있는 상태
    - CPU는 process간에 공유되어야 한다.
3. Blocked
    - 중단된 상태, disk가 읽히거나 입력이 들어와서 일시적으로 run할 수 없는 상태

![stateD](img/processSynchronization/state.png)


## 🫁 Synchronization
> 공유 데이터(Shared data)에 두 개 이상의 프로세스가 동시에 접근하면 data inconsistency(데이터 모순성)가 발생 할 수 있다.

### Interprocess Communication, 프로세스간 통신
- 세 가지 이슈
    - 어떻게 한 프로세스가 다른 프로세스에게 정보를 넘길 것인가?
    - 두개 이상의 프로세스가 하나의 데이터에 동시에 접근하지 않도록 할 것인가?
    - 의존 관계가 존재할 때 어떻게 적절한 순서 부여를 할 것인가?
        - 만약 process A가 데이터 C 생산하고, B가 C를 출력하는 역할을 수행한다면, B는 A가 수행된 후 끝날 때까지 기다려야 한다.

## 🫁 Race Conditions
> Race condition(경쟁 상황)은 두 개 또는 이상의 프로세스가 shared data를 동시에 읽거나 쓰는 상황에서 마지막 결과는 정확히 어떤 프로세스가 언제 수행되었는지에 따라서 결정된다.


## 🫁 Critical Region(Critical section)
- Mutual Exclusion(상호배제)
    - 한 프로세스가 Critical region에 들어오면 다른 프로세스는 critical region에 들어갈 수 없다.
- Critical Region(임계구역)
    - 멀티 프로세스 환경에서 둘 이상의 프로세스가 동시에 접근해서는 안되는 공유 자원의 __코드 영역__ 이다.

critical region은 시간이 지나면 종료되며, 어떤 프로세스가 임계구역에 접근하기 위해서는 지정된 시간만큼 대기해야 한다. 이때 스레드나 프로세스가 배타적인 사용권을 보장받기 위해서 세마포어 같은 동기화 메커니즘을 사용한다.

#### Critical region 문제를 해결하기 위한 네 가지 조건
1. Mutual Exclusion 
    - 두개의 프로세스는 동시에 critical region에 들어갈 수 없다.
2. CPU의 갯수나 성능에 관하여 어떠한 가정도 해서는 안된다.
3. Critical region 밖에 있는 프로세스가 다른 process를 block해서는 안된다.
4. Bounded Waiting(한정 대기)
    - 그 어떤 프로세스도 critical region에 들어가기 위해 영원히 기다려서는 안된다.

저는 위 네 가지로 배웠지만 다른 곳에서 세 가지로 정리한게 많아 세 가지도 적어보겠습니다.

#### Critical region 문제를 해결하기 위한 네 가지 조건

1. Mutual Exclusion(상호배제) - 하나의 프로세스가 임계구역에 있다면 다른 프로세스는 들어갈 수 없다.
2. Progress(진행) - 임계구역에 들어간 프로세스가 없다면 어느 프로세스가 들어갈 것인지 적절히 선택해줘야 한다.
3. Bounded Waiting(한정 대기) - 기아상태를 방지하기 위해, 한 번 들어갔다 나온 프로세스는 다음에 들어 갈 때 제한을 준다.


## 🫁 해결책
임계문제를 해결하기 위한 방식은 많지만 크게 정리를 해보면 
- Disabling Interrupt
- Lock
- Peterson's solution
- TSL instruction(h/w)
- Sleep and Wakeup
- Producer-consumer
- Semaphore
- Monitor 
등등 많습니다.
하지만 주로 다루는 것은 정해져 있기 때문에 궁금하신 분들은 개인적으로 찾아보시면 되고 제 생각에는 면접을 보기 위해서 꼭 알아야 하는 개념은 아래 세가지 정도인 거 같습니다.

### Lock


### Semaphore

### Monitor


### 📚 참고

[Critical Region](https://dduddublog.tistory.com/25)