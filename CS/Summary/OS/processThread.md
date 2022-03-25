# Process

⇒ 실행중에 있는 프로그램

`디스크에 실행 파일 등의 형태로 존재하는 프로그램 메모리에 적재 -> 실행 -> 생명력을 가진 프로세스`

<br>

## Process Context

⇒ 여러 프로세스가 실행 중인 시분할 환경에서 CPU의 점유권이 계속적으로 바뀐다. CPU의 점유권을 빼앗겼다가 다시 되찾은 프로세스의 이전 상태를 재현하기 위해서 필요한 정보 = `Process Context`

<br>

### Context 종류

⇒ `프로세스 context 가지고 있으면 다시 점유권을 획득했을 때 이전의 상태에 이어서 작업을 수행할 수 있다.`

1. CPU의 수행 상태를 나타내는 하드웨어 문맥
    - pc값 - 현재 프로세스 명령어를 포인팅하고 있으며 명령어 주소 저장, 레지스터 정보
    - CPU의 수행상태를 나타내는 정보, 각종 레지스터에 저장하고 있는 값들을 의미한다.
2. 프로세스 주소 공간
    - code, data, stack
3. 프로세스 관련 커널 자료 구조
    - PCB
        - 운영 체제 커널의 자료구조로서 프로세스에게 할당할 자원을 설정하고 감시하는 데 필요한 프로세스 정보
    - Kernal Stack
        - 프로세스가 시스템 콜을 하면 PC가 kernal 주소공간의 code를 가리키며 명령어를 수행한다. 이 때 code를 수행하는데 필요한 정보를 kernal stack에 쌓아둔다.

<br>

## Process State

⇒ Running, Ready, Blocked

1. Running
    - CPU를 잡고 명령어를 수행 중인 상태
    - Running - User level, kernel level
        - 시스템 콜 → kernel level
2. Ready
    - 메모리에 적재되어 바로 실행될 수 있는 조건을 만족한 프로세스가 CPU 점유권을 얻지 못해 대기중인 상태
        - Ready 상태의 프로세스들이 CPU 점유권을 획득하고 빼앗으면서 시분할 환경을 구성한다.
3. Blocked(wait, sleep)
    - CPU를 할당받더라도 당장 명령을 실행할 수 없는 상태
        - io 작업과 같이 요청한 event가 바로 수행 결과를 반환하지 못할 때 프로세스는 blocked 된다.
        - 
4. Suspended(stopped)
    - 외부적인 이유로 프로세스의 수행이 정지된 상태이다.
    - Suspended 상태의 프로세스는 통째로 디스크에 swap out된다.
        - 중기 스케줄러에 의해 메모리를 뺴았긴 프로세스 상태이다 ← 사용자가 프로그램을 일시 정지 시키거나, 시스템 메모리에 프로세스가 너무 많이 올라와있어 잠시 중단 시킬 경우 suspended 상태가 된다.
    - Suspended Blocked, Suspended Ready
        - io작업 중이거나, blocked 상태에서 메모리를 빼앗겨 디스크로 swap out → suspended blocked
        - io 작업을 마치면 → Suspended ready
5. New
    - 프로세스가 생성중인 상태
6. Terminated
    - 프로세스의 수행이 끝난 상태

### 프로세스 상태 전이

1. new → ready
    - 프로세스가 생성중이다가 생성되면 ready 상태로 바뀐다
2. ready → running
    - ready 상태의 프로세스가 cpu 점유권을 얻으면 running 상태로 변한다.
3. running → terminated
    - 프로세스 수행이 끝날 경우, cpu점유권을 반환하고 terminated 상태로 바뀐다.
4. running → waiting
    - 오래 걸리는 작업(io 작업)을 수행할 경우, cpu점유권을 반환하고 waiting 상태로 바뀐다.
5. running → ready
    - timer interrupt(=할당된 시간이 끝날경우)가 들어와서 cpu점유권을 반환하고 ready 상태로 바뀐다.
    
<br>

## PCB(Process Control Block)

⇒ `운영체제가 각 프로세스를 관리하기 위해 프로세스당 유지하는 정보이다. pcb는 구조체 형태이다.`

1. 운영체제가 관리상 사용하는 정보
    - process state, process id
    - scheduling infromation, priority
2. CPU 수행 관련 하드웨어 값
    - PC, register
3. 메모리 관련 
    - code, data, stack
4. 파일 관련
    - open file descriptors

## Process Context Switch

⇒ `cpu 점유권이 현재 프로세스에서 다른 프로세스로 넘어가는 과정`

⇒ 프로세스는 cpu 점유권을 빼앗기는 시점의 문맥을 pcb를 통해 기록해둔다. 이후 점유권을 다시 획득했을 때, pcb를 통해 문맥을 다시 찾아 빼앗겼던 시점에서 수행되던 명령어를 이어서 수행한다.(PCB는 kernal data영역에 존재)

1. CPU 점유권을 빼앗기는 상황
    - 뺏기던 시점의 문맥을 기억하기 위해 해당 프로세스의 PCB에다가 CPU의 pc값과 레지스터 값을 저장한다.
2. CPU 점유권을 얻는 상황
    - 운영체제는 Kernal data에 저장된 해당 프로세스의 PCB에서 CPU의 PC값 및 레지스터 값을 복원시키고 CPU 점유권을 다시 해당 프로세스에게 넘겨주면서 실행한다.

<br>

### Context switch 판단 기준

⇒ context switch가 발생하는지 판단하는 기준은 `CPU 점유권 기준` 보다 `프로세스 기준` 으로 판단하는 것이 옳다.

→ CPU 점유권이 system call이나 interrupt로 인하여 OS로 넘어가는 것은 context switch를 판단하는 기준이 되기 힘들다.

→ Context, 즉 문맥 수행하던 프로세스 A에서 cpu 점유권이 프로세스 B로 넘어가면 문맥이 바뀐 것.

<br>

## Scheduler

⇒ `어떤 프로세스에게 자원을 할당할지를 결정하는 운영체제 커널의 코드를 지칭한다.`

→ 장기 스케줄러(Job), 단기 스케줄러(CPU), 중기 스케줄러(Swapper)

1. Long-Term Scheduler (장기 스케줄러, job scheduler)
    - 메모리 및 각종 자원을 어떤 new 상태의 프로세스에게 할당할지 결정하는 스케줄러
        - 프로세스가 생성 중이다가(new) 메모리 할당을 받으면 비로소 ready 상태가 되어 ready queue에서 대기한다. 이때 new 상태의 프로세스에 메모리를 할당해주는 역할을 하는게 long-term 스케줄러이다.
        - degree of multiprogramming 제어 - 메모리에 동시에 올라갈 프로세스의 수를 제어한다.
    - 보통의 시분할 시스템에서 장기 스케줄러 없음
        - degree of multiprogramming 제어 → 중기 스케줄러
2. Short-Term Scheduler(단기 스케줄러, CPU 스케줄러)
    - CPU 점유권을 어떤 프로세스에게 할당할지 결정하는 스케줄러
        - 스케줄링이 일어나는 단위가 ms → 자주 일어남
3. Medium-Term Scheduler (중기 스케줄러, swapper)
    - 메모리에 많은 프로그램 동시에 올라갈 때 → 중기 스케줄러가 swap out 시킴
        - degree of multiprogramming 제어
            - 중기 스케줄러에 의해 메모리를 뺴앗긴 프로세스 Suspended state


<br><br>

# Thread

⇒ `프로세스가 할당받은 자원을 이용하는 실행 단위`

→ 프로세스 생성 시 kernel data 영역에 pcb가 생성되는데, 동일한 작업을 수행하기 위해 여러 프로세스를 생성하면 메모리 주소 공간 낭비 많아짐 → 그래서 `동일 단순 작업은 thread로 하자`

→ 하나의 프로세스 has 하나의 task + 여러 개의 thread

- cpu 연산을 수행을 위해서 실행할 명령어의 위치를 담는 pc값, 레지스터 값이 필요 → 스레드마다 별도로 pc, 레지스터 값을 가지고 있음
- thread가 code 부분을 수행하다 함수 호출이 일어나면 함수에 관련된 정보를 stack에 쌓아둔다 → 스레드마다 별도로 stack 가지고 있음 (code, data영역은 스레드끼리 공유)

⇒ `PC값, 레지스터 값, Stack 영역 per thread`

⇒ `code, data 영역 shared`

<br><br>

# Multi-Process & Multi-Thread

## Multi-Process

⇒ `하나의 프로그램을 여러 프로세스로 구성하여 각 프로세스가 병렬적으로 작업 수행`

→ 각 프로세스 간 메모리 구분이 필요하거나 독립된 주소 공간을 가져야 할 경우 사용한다.

<br>

### Multi-Process 장점

- 안정성
    - 독립된 구조로 하나의 프로세스에 문제가 발생해도 다른 프로세스에 영향을 끼치지 않는다.

### Multi-Process 단점

- 오버헤드
    - 독립된 메모리 영역이기 때문에 context switching이 자주일어나는데, context  switching과정에서 캐시 메모리 초기화 등 무거운 작업이 진행되고 시간 소모가 크기 때문에 오버헤드가 발생한다.
- 프로세스 간의 통신 방식이 따로 필요하다(w/ ICPC)

<br>

## Multi-Thread

⇒ `하나의 프로세스에 여러 스레드로 자원을 공유하며 작업을 나누어 수행하는 것이다.`

<br>

### Multi-Thread 장점

- 응답성
    - 다중 스레드로 구성된 task 구조에서 하나의 스레드가 blocked 상태인 동안에도 동일한 task 내의 다른 스레드가 running 상태로 실행되어 빠른 처리가 가능하다.
    - 웹 브라우저가 다중 스레드로 수행될 때, 하나의 스레드는 이미지를 다운로드하기 위해 서버에 네트워크 요청을 걸어 blocked 상태가 되지만(오래 걸리는 작업), 다른 스레드는 running 상태에서 받아오는 html 파일을 화면에 출력해줄 수 있다.
- 자원공유
    - 하나의 프로세스 안에 CPU 수행 단위인 쓰레드를 두게 되면 code, data, resource 자원을 공유하여 효율적으로 자원 활용이 가능하다.
- 경제성
    - 동일한 일을 수행하는 다중 스레드가 협력하여 높은 처리율, 성능 향상을 얻을 수 있다.
    - 새로운 프로세스 하나를 만드는 것 보다 기존의 프로세스에 쓰레드를 추가하는 것이 오버헤드가 훨씬 적다. *(스레드 간의 Context Switching은 프로세스의 Context Switching과 달리 캐시 메모리를 초기화할 필요가 없고 Code, Data, Resource 영역을 공유하므로 Stack 영역만 처리하면 되기 때문)*
- 멀티 프로세서 아키텍처에서의 이용성
    - Multi-Processer(CPU가 여러개) 아키텍쳐에서 각각의 쓰레드가 서로 다른 CPU에서 병렬적으로 작업을 진행할 수 있기 때문에 훨씬 효율적으로 작업을 수행할 수 있다.

### Multi-Thread 단점

- 자원을 공유하기 때문에 동기화 문제가 발생할 수 있음 (병목현상, 데드락)
    - 동일한 자원에 대하여 동시에 작업이 들어가는 경우 각 스레드의 결과에 영향을 미친다. 이를 해결하기 위해 임계 영역에 대하여 뮤텍스(mutex), 또는 세마포어(Semaphore) 방식을 활용한다.
- 하나의 thread에 문제가 발생하면 전체 프로세스 영향 받음 ↔ 멀티 프로세스의 경우, 한 프로세스에 문제가 생겨도 다른 프로세스의 실행에는 영향이 없다.