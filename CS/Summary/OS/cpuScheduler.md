# CPU Scheduler & Scheduling Algorithm


## CPU busrt
=> `CPU`로 일반 명령어`를 수행하는 작업

CPU burst는 사용자 프로그램이 cpu를 직접 가지고 빠른 명령을 수행하는 단계이다. 이 단계에서 사용자 프로그램은 cpu내에서 일어나는 명령이나 메모리에 접근하는 일반 명령어를 사용할 수 있다.

## I/O burst
=> `운영체제`의 도움을 받아 작업을 수행해야하는 작업

I/O burst는 커널에 의해 입출력 작업을 진행하는 비교적 느린 단계이다. 이 단계에서는 모든 입출력 명령을 특권 명령으로 규정하여 사용자 프로그램이 직접 수행할 수 없고(user level X, kernal level O), 운영체제를 통해 서비스를 대행하도록 한다.

__예시__

```python
1. 키보드 입력을 받는 작업
2. 컴퓨터에서 처리된 결과를 화면에 출력하는 작업
3. 디스크에서 파일 데이터를 읽어오는 작업 
...
```

<br>

## CPU bound process & I/O bound process
각 프로그램마다 cpu버스트와 io버스트가 차지하는 비율이 균일하지 않다. 어떤 프로세스는 io작업이 빈번하면서 cpu작업이 적고 이 반대도 있다. 어떤 버스트가 더 자주 일어나는지에 따라서 io, cpu bound process로 나눌 수 있다.

### I/O bound process
=> `I/O 버스트`가 긴 프로세스
I/O요청이 빈번해 CPU 버스트가 짧게 나타나는 프로세스를 말한다. 주로 사용자로부터 interaction을 계속 받아가며 프로그램을 수행하는 대화형 프로그램이 이에 해당한다.

### CPU bound process
=> `CPU 버스트`가 긴 프로세스
I/O작업을 거의 수행하지 않아 CPU 버스트가 길게 나타나는 프로세스를 말한다. 주로 프로세스 수행의 상당 시간을 입출력 작업 없이 CPU 작업에 소모하는 계산 위주의 프로그램이 해당된다.

<br><br>

## CPU Scheduling
=> __CPU 사용을 효율적__ 으로 하기 위해 운영체제가 `CPU 점유권`을 우선적으로 할당해야할 프로세스를 선정하는 과정 (schduling - OS가 수행)

컴퓨터 시스템 내에서 수행되는 프로세스의 CPU 버스트를 분석해보면 대부분의 경우 짧은 CPU 버스트를 가지며, 극히 일부분만 긴 CPU 버스트를 갖는다. 이는 다시 말해서 CPU를 한 번에 오래 사용하기 보다는 잠깐 사용하고 I/O 작업을 수행하는 프로세스가 많다는 것이다. 즉 대화형 작업을 많이 수행한다는 의미이다. 사용자에 대한 빠른 응답을 하기 위해 해당 프로세스에게 우선적으로 CPU를 할당하는 것이 바람직하다. 만약 CPU 바운드 프로세스에게 먼저 CPU를 할당한다면 그 프로세스가 CPU를 사용할 때까지 많은 I/O 바운드 프로세스는 기다리게 된다. 

<br>

## CPU Scheduler
=> ready 상태에 있는 프로세스들 중 어떠한 프로세스에게 CPU를 할당할 지 결정하는 `운영체제의 코드`

__선점형(preemptive)__ : 프로세스가 CPU를 계속 사용하기 원해도 강제로 빼앗을 수 있는 스케줄링 방법
__비선점형(non-preemptive)__ : CPU를 획득한 프로세스가 스스로 CPU 점유권을 포기하기 전까지 CPU를 빼앗지 않는 스케줄링 방법

__CPU Scheduler가 필요한 경우__
- `Running -> Blocked` = I/O요청하는 시스템콜 (non-preemptive, 알아서 내놓기 때문)
- `Runing -> Ready` = 퀀텀을 모두 사용한 timer interrupt (preemptive, 강제 뺏기)
- `Blocked -> Ready` = I/O 작업 완료 후 interrupt
- `Terminated` = 작업을 마친 프로세스를 free (non-preemptive, 알아서 내놓기 때문)

__Dispatcher__
- 새롭게 선택된 프로세스가 CPU를 할당받고 작업을 수행할 수 있도록 환경 설정을 하는 운영체제 코드
- CPU를 누구한테 줄 지 결정하여 프로세스에게 넘겨주는 역할을 수행 (Context switch)
- 디스패치 지연시간(Dispatch latency) : dispatcher가 하나의 프로세스를 정지시키고 다른 프로세스에게 cpu를 전달하기 까지 걸리는 시간 (context switch overhead)

<br>

### Scheduling Criteria(스케줄링 성능 평가 기준)
__시스템 입장__
- CPU Utilization(이용률)
    - 전체 시간 중에서 CPU가 일을 한 시간의 비율
    - CPU를 최대한 바쁘게 둬라
- Throughput(처리량)
    - 주어진 시간동안 준비큐에서 기다리고 있느 프로세스 중 몇 개를 끝마쳤는지 나타낸다

__프로세스 입장__
- Turnaround time (소요시간, 반환 시간)
    - CPU를 다 쓰고 CPU 버스트 끝난 시점 - CPU 요청한 시점
    - 준비큐에서 기다린 시간 + 실제로 cpu 사용한 시간
- Waiting time(대기 시간)
    - CPU 버스트 기간준 준비 큐에서 기다린 시간
- Response time(응답 시간)
    - 프로세스가 준비 큐에 들어온 후 첫 번째 cpu를 획득하기까지 기다린 시간

<br><br>

## Schduling Algorithm
```python
1. FCFS (First come first served, 선입 선출 스케줄링)
2. SJF (Shortest Job First, 최단 작업 우선 스케줄링)
3. Priority (우선 순위 스케줄링)
4. RR (Round Robin, 라운드 로빈 스케줄링)
5. Multi-Level Queue (멀티 레벨 큐)
6. Multi-Level Feedback Queue(멀티 레벨 피드백 큐)
7. Multi-Processor (다중 처리기 스케줄링)
8. Real-time (실시간 스케줄링)
9. Thread (스레드 스케줄링)
```

#### FCFS (First come first served, 선입 선출 스케줄링)
=> `먼저 온 순서대로 처리`
    - CPU를 오래 쓰는 프로세스가 먼저 오면 엄청난 비효율 발생 (= 어떤 프로세스가 우선적으로 수행되는지에 따라 대기 시간에 많은 영향을 끼침)
- nonpreemptive

#### SJF (Shortest Job First, 최단 작업 우선 스케줄링)
=> `CPU 버스트가 가장 짧은 프로세스에게 먼저 CPU를 할당하는 방식`
- Preempive
    - SRTF (Shortest Remaining Time First) - 가장 남은 시간이 적은 작업 우선 실행
    - CPU를 잡아도 더 짧은 프로세스 들어오면 점유권 빼앗김
- nonpreemptive
    - 일단 CPU를 잡으면 더 짧은 프로세스 들어와도 CPU 버스트 끝날 때까지 점유권 빼앗기지 않음
- 문제점
    - Starvation : 짧은 프로세스로 인해 긴 프로세스는 영원히 cpu를 점유하지 못할 수 있음
    - CPU 버스트 시간을 미리 알 수 없음, 과거 CPU 사용시간을 이용하여 추정

#### Priority (우선 순위 스케줄링)
=> `우선순위가 제일 높은 프로세스에게 CPU 할당` (우선순위가 작은게 높은 우선권)
- nonpreemptive
    - cpu를 잡으면 더 높은 우선 순위를 가진 프로세스가 들어와도 CPU 버스트 완료될 때까지 점유권 빼앗기지 않음
- preemptive
    - CPU를 잡았더라도 더 높은 우선 순위를 가진 프로세스가 들어오면 점유권 빼앗김
- 문제점
    - Starvation : 우선순위 낮은 프로세스는 영원히 cpu를 차지 못할 수 있음
- 해결 방안
    - Aging : 아무리 우선 순위가 낮은 프로세스라도 시간이 지나면 우선순위 높여주는 기법

#### RR (Round Robin, 라운드 로빈 스케줄링)
=> `time quantum을 각 프로세스에게 부여하여 정해진 time quantum을 사용하면 ready queue에 뒤로 가서 줄을 서고, queue의 맨 앞부터 차례대로 수행하는 기법`
- preemptive
    - 정해진 시간만큼 cpu 버스트를 사용하고 다음 프로세스에게 점유권을 넘겨주기 위해 점유권 빼앗기 때문
- 짧은 응답 시간 (모든 n개 프로세스, q time, (n-1)*q 미만으로 대기)
- Quantum이 크면 FCFS, Quantum이 작으면 context switch overhead(dispatcher만 불쌍해짐 ㅠ)
- Job의 작업 시간이 분산되어 있으면 효율적, 모두 동일한 작업시간 비효율적

#### Multi-Level Queue (멀티 레벨 큐)
=> `Round-robin + FCFS + priority scheduling`
- 우선 순위에 따라 ready queue 분할
    - 전위큐 - 빠른 응답(io, RR), 후위큐 - 계산(cpu, FCFS)
- 각각의 큐도 scheduling 필요
    - 고정 우선 순위 - 전위큐 우선적으로 cpu할당, 전위큐 비면 후위큐 cpu 할당 (starvation 유발 가능)
    - 타임 슬라이스 - 각 큐에 cpu time을 정절한 비율로 할당 (8:2 = 전위:후위)

#### Multi-Level Feedback Queue(멀티 레벨 피드백 큐)
=> `Multi-level queue + feedback 기능`
- 프로세스가 여러 개로 분할된 ready queue 내에서 다른 큐로 이동 가능
- aging 구현 가능
- 멀티 레벨 피드백 큐 정의하는 요소 : 큐의 수, 각 큐 스케줄링 알고리즘, 승격+강등 기준, 프로세스 도착 시 어느 레벨로 들어갈지 결정

#### Multi-Processor (다중 처리기 스케줄링)
=> `CPU가 여러 개인 시스템 환경에서 사용하는 기법`
- 프로세스를 ready queue에 한 줄로 세워서 각 프로세스가 알아서 다음 프로세스를 꺼내어 가도록 한다.
- 대칭형 다중 처리
    - 각 CPU가 각자 알아서 스케줄링을 결정
- 비대칭형 다중 처리
    - 하나의 CPU가 다른 모든 CPU의 스케줄링 및 데이터 접근을 책임지고 나머지 CPU는 거기에 따라 움직이는 방식


#### Real-time (실시간 스케줄링)
=> `deadline이 있는 프로세스들의 cpu 점유권을 결정하는 기법`
- hard real-time system
    - 정해진 시간 안에 반드시 작업이 끝나도록 스케줄링해야 함
- soft real-time system
    - 데드라인이 존재하기는 하지만 지키지 못했다고 해서 위험한 상황이 생기지 않는다. (나의 취준..?)

#### Thread (스레드 스케줄링)
=> `thread간 점유권을 어떻게 할당할 것인지 결정하는 기법`
- local scheduling
    - 운영체제 입장 스레드 존재 모름
    - Process가 cpu 점유권 thread에게 부여
- global scheduling
    - 커널 레베 스레드 일반 프로세스처럼 커널의 단기 스케줄러가 어떤 스레드를 스케줄할 지 결정