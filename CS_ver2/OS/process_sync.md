# Process_Synchronization
> 동기화 문제: 서로 다른 thread가 메모리 영역을 공유하기 때문에 여러 thread가 동일한 자원에 동시에 접근하여 엉뚱한 값을 읽거나 수정하는 문제

> 공유 데이터(Shared data)에 두 개 이상의 프로세스가 동시에 접근하면 data inconsistency가 발생할 수 있다. 

- 데이터 일관성 유지: 협력 프로세스들이 바른 순서로 실행(orderly execution)하는 것을 보장하는 매커니즘이 필요


<br><br>

# Race Condition
> 여러 개의 프로세스가 공유 자원에 접근하는 상황. 데이터의 최종 연산 결과는 마지막에 그 데이터를 다룬 프로세스에 따라 달라진다.

<br>

## Race Condition이 발생할 때
- kernel 수행 중 인터럽트 발생 시
- Process가 system call을 하여 kernel mode로 수행 중인데 context switch가 일어나는 경우
- Multiprocessor에서 shared memory 내의 kernal data

<br><br>

# Critical Region(Critical Section; CS)
> 프로세스(쓰레드)가 공유 자원을 변경할 수 있는 __코드 부분__

## Critical region 문제를 해결하기 위한 세 가지 조건

1. Mutual Exclusion(상호배제): 프로세스가 CS 부분을 수행 중이면 다른 모든 프로세스들은 그들의 CS에 들어가면 안된다. 

2. Progress(진행): 아무도 CS에 있지 않은 상태에서 CS에 들어가고자 하는 프로세스가 있으면 CS에 들어가게 해주어야 한다.

3. Bounded Waiting(한정 대기): 프로세스가 CS 진입을 요청한 후에 요청이 허용될 때까지 다른 프로세스가 CS 진입이 허용되는 횟수에 제한이 있어야 한다. -> 그 어떤 프로세스도 CS 진입을 영원히 기다리지 않아야 한다.

<br>

# 해결책

> Higher-level software tools: Mutex Locks, Semaphores, Monitors 등

<br>

 # Mutex(Mutual Exclusion) Locks
 > 한 개의 thread만이 공유 자원에 접근할 수 있도록 하여, race condition을 방지하는 기법

 > CS에 들어가기전에 lock을 획득하고, 나올때는 lock을 반환한다.

- 특징

    ![m6](../../OS/img/processSynchronization/m6.png)

    <br>

    - Locking 메커니즘으로 오직 하나의 프로세스나 스레드만이 동일한 시점에 mutex를 얻어 CS에 들어올 수 있다. 그리고 해당 프로세스나 스레드만이 CS를 벗어날 때 mutex를 해제할 수 있다.

    - acquire()과 release()는 atomic 해야한다.

    - Busy Waiting: CS에 들어가기 위해서 계속해서 acquire()를 호출하는 반복문을 실행한다. -> lock이 가용해지기를 기다리면서 프로세스가 계속 반복 회전하고 있기 때문에 spinlock이라고 부르기도 한다.

    - but 멀티프로세서 시스템에서 프로세스가 계속 일을 하고 있기 때문에 Context Switching을 할 필요가 없다는 것은 장점이 될 수도 있다. -> 프로세스들이 짧은 시간 동안 Lock을 소유하는 경우라면 mutex가 유용하게 사용될 수 있다.

    <br>

    - Busy Waiting이 없는 Mutex Lock: Test and Set을 사용
        - Test and Set: 하드웨어적으로 atomic하게 수행할 수 있도록 한다.
        - lock을 획득하지 못하면 프로세스는 lock을 기다리는 대기상태로 전환하고 CPU를 내어 놓음


<br>

# Semaphore(세마포어)
> S개의 thread만이 공유 자원에 접근할 수 있도록 제어하는 동기화 기법

- semaphore S: 사용 가능한 특정 자원의 개수. 정수형 변수.

- Semaphore 연산
    - P operation(wait(S), acquire(S))
    - V operation(signal(S), release(S))

    ![s5](../../OS/img/processSynchronization/s5.png)

- Signaling 메커니즘으로 lock을 걸지 않은 스레드도 Signal을 보내 lock을 해제할 수 있다.

- 용도

    - 상호 배제(mutual exclusion) -> binary semaphore
    - 유한 개수의 자원 접근, 한정된 concurrency -> counting semaphore

## Binary semaphore(mutex lock과 유사)

- semaphore 값: 0 또는 1(1로 초기화); S=1: unlock, S=0: lock

- 임계구역 접근 제어에 사용


![s6](../../OS/img/processSynchronization/s6.png)

## Binary semaphore와 mutex lock의 차이점

![s10](../../OS/img/processSynchronization/s10.png)

## Counting Semaphore(한정된 concurrency, 유한 개수 자원 접근)

- semaphore 값: 가용 자원 개수를 의미(최대 가용 자원 개수로 초기화)
- 유한 개수의 자원의 접근 제어에 사용
- Busy Waiting이 있다.(spin lock)

![s7](../../OS/img/processSynchronization/s7.png)

<br><br>

- semaphore를 이용한 동기화

![s8](../../OS/img/processSynchronization/s8.png)

<br>

### Semaphore 구현
- Block & Wakeup 방식(sleep lock)
- semaphore 값을 먼저 감소 -> 음수가 가능하다.
- 음수의 크기는 semaphore를 대기하는 프로세스들의 개수이다.
- 두 프로세스가 같은 semaphore에 대해 P 또는 V 연산이 중첩되어 실행되지 않도록 해야한다.(atomic 실행)
