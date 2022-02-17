# 🔒 Deadlock

## 📚 Table of Content

> 1.Deadlock

>> Resource

>> Deadlock 예시

> 2.Deadlock 발생 조건 

> 3.Deadlock 처리 방법

>> Deadlock Prevention

>> Deadlock Avoidance

>>> Resource Alloc Algorithm, Banker's Algorithm

>> Deadlock Detection and Recovery

>> Deadlock Ignorance

<br><br><br>

## 🔒 1. Deadlock

⇒ 일련의 프로세스들이 서로가 가진 자원을 기다리며 block된 상태, 교착상태

### Resource

- 하드웨어, 소프트웨어 등을 포함하는 개념
    - IO device, CPU cycle, memory space, semaphore
- 프로세스가 자원을 사용하는 절차
    - request, allocate, use, release

### Deadlock 예시

- first case
    - 시스템에 2개의 tape drive가 있다.
    - 프로세스 p1과 p2 각각 하나의 다른 tape drive를 보유한 채 다른 하나를 기다리고 있다.
- second case
    - 이진 세마포어 A와 B가 있다.
    - 프로세스 P1이 자원을 얻은 상태에서 context switch가 발생하여 프로세스 P2에게 cpu 제어권이 넘어갔고, 프로세스 p2가 B 자원을 얻었다.
    - 다시 context switch가 발생하여 프로세스 p1에게 cpu 제어권이 넘어갔고, 프로세스 p1이 b 자원을 얻고 싶어하지만 이미 프로세스 p2가 B자원을 가지고 있다.
    - 마찬가지로 프로세스 P2가 A자원을 얻고 싶어하지만 이미 프로세스 P1이 A자원을 가지고 있다.
    - 무한히 서로를 기다린다.

<br><br><br>

## 🔒 2. Deadlock 발생의 4가지 조건

- 상호 배제, Mutual exclusion
    - 동시에 두 개의 프로세스가 공유 자원에 접근할 수 없다.
- 비선점, No preemption
    - 프로세스는 자원을 내놓을 뿐 강제로 빼앗기지 않음
- 점유 대기, Hold and wait
    - 자원을 가진 프로세스가 다른 자원을 기다릴 때 보유 자원을 놓지 않고 계속 가지고 있음
- 순환 대기, Circular wait
    - 자원을 기다리는 프로세스 간에 사이클이 형성되어야 함.
    - 프로세스 {P0, P1, ... Pn}가 있다면 P0 → P1 → P2 → ..Pn → P0

**위 4가지 조건이 동시에 성립할 때 발생합니다.**

<br><br>

## 🔒 3. 교착 상태 처리 방법

- Deadlock Prevention, 교착 상태 예방
    - 자원 할당 시 데드락의 4가지 필요 조건 중 어느 하나가 만족되지 않도록 하는 것
- Deadlock Avoidance, 교착 상태 회피
    - 자원 요청에 대한 부가정인 정보를 이용해서 데드락의 가능성이 없는 경우에만 자원을 할당
    - 시스템 상태가 원래 상태로 돌아올 수 있는 경우에만 자원 할당
- Deadlock Detection and recovery, 교착 상태 탐지
    - 데드락 발생은 허용하되 그에 대한 탐지 루틴을 두어 데드락 발견시 회복
- Deadlock Ignorance, 교착 상태 무시
    - 데드락을 시스템이 책임지지 않음
    - unix를 포함한 대부분의 os가 채택

⇒ *예회탐회*

### 3-1. Deadlock Prevention, 교착 상태 예방

> 교착 상태 발생 조건 중 하나를 제거함으로 해결하는 방법. 자원의 낭비가 심하다는 단점이 존재한다.
> 
1. **Mutual exclusion**, 상호 배제 부정 여러 프로세스가 공유 자원을 사용하도록 한다.
    - 공유해서는 안되는 자원의 경우 반드시 성립해야 함.
2. **Hold and wait**, 점유 대기 부정 프로세스가 실행되기 전 필요한 모든 자원을 할당한다.
    - 프로세스가 자원을 요청할 때 다른 어떤 자원도 가지고 있지 않아야 함.
        1. 프로세스 시작 시 모든 필요한 자원을 할당 받게 하는 방법
        2. 자원이 필요할 경우 보유 자원을 모두 놓고 다시 요청
3. **No preemption**, 비선점 부정 자원 점유 중인 프로세스가 다른 자원을 요구할 때 점유 중인 자원을 반납하고 요구한 자원을 사용하기 위해 기다리게 한다.
    - 프로세스가 어떤 자원을 기다려야 하는 경우 이미 보유한 자원이 선점됨
    - 모든 필요한 자원을 얻을 수 있을 때 그 프로세스는 다시 시작됨
    - 상태를 쉽게 저장하고 복구할 수 있는 자원에서 주로 사용
4. **Circular wait**, 순환 대기 부정 자원에 고유한 번호를 할당하고 번호 순서대로 자원을 요구하도록 한다.
    - 모든 자원 유형에 할당 순서를 정하여 정해진 순서대로만 자원을 할당
        - ex) 순서가 3인 자원 Ri를 보유중인 프로세스가 순서가 1인 자원 Rj를 할당 받기 위해서는 우선 Ri를 반납해야 함

**예방 방법을 사용하게 될 때 발생하는 문제점 ⇒ utilization 저하(system 입장), throughput 감소(system 입장), starvation 문제(process 입장)**

### 3-2. Deadlock Avoidance, 교착 상태 회피

> 교착 상태가 발생하면 피해나가는 방법. 은행원 알고리즘은 다익스트라가 제안한 방법으로 은행에서 모든 고객의 요구가 충족되도록 현금을 할당하는데서 유래된 기법이다.
> 
- 프로세스가 자원을 요구할 때, 시스템은 자원을 할당한 후에도 안정 상태로 남아있게 되는지를 검사하여 교착 상태를 회피하는 기법이다.
- 안정 상태에 있으면 자원을 할당하고 그렇지 않으면 다른 프로세스들이 자원을 해제할 때까지 대기한다.
- 시스템이 안전 상태에 있으면 데드락이 아니고, 불안전 상태에 있으면 데드락의 가능성이 있다. *데드락 회피*는 시스템이 불안전 상태에 들어가지 않는 것을 보장.

```markdown
자원 유형 1개 당 1개의 인스턴스 존재 → 자원 할당 그래프 알고리즘

자원 유형 1갱 당 2개 이상의 인스턴스 존재 → 은행원 알고리즘, banker’s algorithm
```

교착 상태 회피를 하기 위해 알아햐 하는 것들

- safe state, 안전 상태
- safe sequence, 안전 순서열

교착 상태 회피 알고리즘

- 자원 할당 그래프 알고리즘
- 은행원 알고리즘

### 3-3. Deadlock detection and recovery, 교착 상태 탐지 및 회복

> 자원 할당 그래프를 통해 교착 상태를 탐지할 수 있다.
> 

### 3-4. Deadlock Ignorance, 교착 상태 무시

> 교착 상태를 일으킨 프로세스를 종료하거나 할당된 자원을 해제함으로써 회복하는 것을 의미한다.
>