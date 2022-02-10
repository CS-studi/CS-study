# CPU Scheduler & Scheduling Algorithm

> 프로세스 생성

> CPU Scheduler & Dispatcher

> Scheduling Algorithm

<br>

# 프로세스 생성

- 운영체제가 최초의 프로세스를 생성하면, 이미 존재하는 프로세스가 다른 프로세스를 복제 생성한다. 이때 기존 프로세스를 부모 프로세스라 하고, 새로 생긴 프로세스를 자식 프로세스라고 부른다. 

-> 사용자 프로세스가 실제로 복제 생성할 수는 없기 때문에 운영체제에 요청을 한다.(fork() 시스템 콜)
- 프로세스의 트리 (계층 구조)를 형성한다.
- 프로세스는 자원을 필요로 한다.
    - 운영체제에게 받는다.
    - 부모와 공유한다.
- 자원의 공유
    - 부모와 자식이 모든 자원을 공유하는 모델
    - 일부를 공유하는 모델
    - 전혀 공유하지 않는 모델
- 수행 (Execution)
    - 부모와 자식이 공존하며 수행하는 모델 (이때는 부모와 자식이 CPU를 획득하기 위해 경쟁하는 관계가 됨)
    - 자식이 종료(terminate)될 때까지 부모가 기다리는(wait) 모델

### 주소 공간 (Address space)

- 자식은 부모의 공간을 복사한다. (프로세스 ID를 제외한 운영체제 커널 내의 정보와 주소 공간의 정보)
- 자식은 그 공간에 새로운 프로그램을 올린다.
- 유닉스의 예
    - `fork()` 시스템 콜이 새로운 프로세스를 생성한다. 부모를 그대로 복사하고 주소 공간을 할당한다.
    - `fork()` 다음에 이어지는 `exec()` 시스템 콜을 통해 새로운 프로그램으로 주소 공간을 덮어 씌운다.

## 프로세스 종료 (Process Termination)

- 프로세스가 마지막 명령을 수행한 후 운영체제에게 이를 알려준다. (`exit()`)
    - 자식이 부모에게 output data를 보낸다.
    - 프로세스의 각종 자원이 운영체제에 반납된다.
- 부모 프로세스가 자식의 수행을 종료한다. (`abort()`)
    - 자식이 할당 자원의 한계치를 넘어선다.
    - 자식에게 할당된 task가 더 이상 필요하지 않는다.
    - 부모가 종료(exit)한다.
        - 운영체제는 부모 프로세스가 종료하는 경우 자식이 더 이상 수행되도록 주지 않는다.
        - 가장 자식인 프로세스부터 단계적인 종료

`exit()` 는 프로그램이 끝날 때 운영체제에게 자신이 끝났음을 알리는 자발적 종료에 해당하는 시스템 콜이고, `abort()` 는 부모 프로세스가 자식 프로세스의 수행을 중단하는 비자발적 종료에 해당하는 시스템 콜이다.

## 각종 시스템 콜

### `fork()` 시스템 콜

**개념**

- 운영체제는 자식 프로세스의 생성을 위해 `fork()` 시스템 콜을 제공한다.
- 프로세스가 해당 시스템 콜을 호출하면 CPU의 제어권이 커널로 넘어가고, 커널은 `fork()` 를 호출한 프로세스를 복제하여 자식 프로세스를 생성한다.
- `fork()` 를 수행하면 부모 프로세스의 주소 공간을 비롯해 프로그램 카운터 등 레지스터 상태, PCB 및 커널 스택 등 모든 문맥을 그대로 복제해 자식 프로세스의 문맥을 형성한다.
- 따라서 자식 프로세스는 부모 프로세스의 처음부터 수행하지 않고, 부모 프로세스가 현재 수행한 시점부터 수행하게 된다.
- 다만 자식 프로세스와 부모 프로세스의 식별자는 다르다. (운영체제가 프로세스를 관리해야 하기 때문

**소스 코드**

![Untitled](https://www.notion.so/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2F9d32e02b-ed27-4c0c-8c11-167a32dfd4a0%2FUntitled.png?table=block&id=18f3c4bd-3733-4408-abba-022136318566&spaceId=b453bd85-cb15-44b5-bf2e-580aeda8074e&width=1530&userId=80352c12-65a4-4562-9a36-2179ed0dfffb&cache=v2)

- 부모 프로세스가 메인 함수의 첫 번째 줄부터 한 줄씩 코드를 수행하다가 `fork()` 라인에 이르면 자신과 똑같은 프로세스를 하나 생성한다.

![Untitled](https://www.notion.so/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2F384b319d-84b6-4356-883f-39af89b29e6e%2FUntitled.png?table=block&id=f55a85b7-beee-46e8-bc01-30c2f53392c5&spaceId=b453bd85-cb15-44b5-bf2e-580aeda8074e&width=1530&userId=80352c12-65a4-4562-9a36-2179ed0dfffb&cache=v2)

- 이때 `fork()` 라인까지 수행했다는 기억조차도 똑같은 자식 프로세스가 생성된다.
- 자식 프로세스가 복제된 프로세스라는 사실을 알 수 있는 단서가 있는데, `fork()` 의 결과 값으로 0을 반환한다. (진짜 부모 프로세스는 0보다 큰 값을 호출한다.)

### `exec()` 시스템 콜

**개념**

- `fork()` 시스템 콜만으로는 같은 코드에 대해 조건을 분기하는 정도로 밖에 사용할 수 없다.
- 자식 프로세스에게 부모 프로세스와는 독자적인 프로그램을 수행할 수 있는 메커니즘이 바로 `exec()` 시스템 콜이다.
- 이 시스템 콜은 프로세스의 주소 공간에 새로운 프로그램을 덮어 씌운 후, 새로운 프로그램의 첫 부분부터 다시 실행하도록 한다.
- 따라서 새로운 프로그램을 수행하기 위해서는 `fork()` 시스템 콜로 복제 프로세스를 생성한 뒤, `exec()` 시스템 콜로 해당 프로세스의 주소 공간을 새롭게 수행하려는 프로세스의 주소 공간으로 덮어 씌우면 된다.

**소스 코드**

![Untitled](https://www.notion.so/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2F8c9c09f4-1fdf-49b2-8837-8f127dd28b9c%2FUntitled.png?table=block&id=2af3a246-6210-4f70-941d-bf777deb785e&spaceId=b453bd85-cb15-44b5-bf2e-580aeda8074e&width=1530&userId=80352c12-65a4-4562-9a36-2179ed0dfffb&cache=v2)

- `execlp()` 라인에 이르면 새로운 프로그램으로 덮어 씌운다.
- 그렇기 때문에 `execlp()` 아래의 코드는 무시된다.

### `wait()` 시스템 콜

- 자식 프로세스가 종료되기를 기다리며 부모 프로세스가 봉쇄 상태에 머무르도록 할 때 사용한다.
- 부모 프로세스가 `fork()` 후에 `wait()` 을 호출하면 자식 프로세스가 종료될 때까지 부모 프로세스를 봉쇄 상태에 머무르게 하고, 자식 프로세스가 종료되면 부모 프로세스를 준비 상태로 변경한다.
- `vi` 명령어를 예로 들 수 있다.

![Untitled](https://www.notion.so/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2F36912aa1-802e-47c0-9bca-4e54803ee23e%2FUntitled.png?table=block&id=6cba9aeb-f604-4ef0-9b37-bd0c0be4fc46&spaceId=b453bd85-cb15-44b5-bf2e-580aeda8074e&width=1530&userId=80352c12-65a4-4562-9a36-2179ed0dfffb&cache=v2)

### `exit()` 시스템 콜

- 프로세스를 자발적으로 종료할 때 사용한다.
- 마지막 statement 수행 후 `exit()` 시스템 콜을 수행한다.
- 프로그램에 명시적으로 적어주지 않아도 main 함수가 리턴되는 위치에 컴파일러가 넣어준다.

### `abort()` 시스템 콜

- 프로세스를 비자발적으로 종료할 때 사용한다.
- 부모 프로세스가 자식 프로세스를 강제로 종료한다.
    - 자식 프로세스가 한계를 넘어서는 자원 요청
    - 자식에게 할당된 task가 더 이상 필요하지 않음
- 키보드로 kill, break 등을 입력한 경우
- 부모가 종료하는 경우
    - 부모 프로세스가 종료하기 전에 자식들이 먼저 종료됨.

## 프로세스 간의 협력

### 독립적 프로세스 (Independent process)

- 프로세스는 각자의 주소 공간을 가지고 수행되므로 원칙적으로 하나의 프로세스는 다른 프로세스의 수행에 영향을 미치지 못한다.

### 협력 프로세스 (Cooperating process)

- 프로세스 협력 메커니즘을 통해 하나의 프로세스가 다른 프로세스의 수행에 영향을 미칠 수 있다.

### 프로세스 간 협력 메커니즘 (IPC: Interporcess Communication)

- IPC는 프로세스들 간의 통신과 동기화를 이루기 위한 메커니즘을 의미한다.
- 메시지 전달 방식 (커널을 통해 메시지 전달)과 공유 메모리 방식 (일부 주소 공간을 공유)이 있다.

### 메시지 전달 방식 (Message Passing)

- 프로세스 간에 공유 데이터를 일체 사용하지 않고 메시지를 주고 받으면서 통신하는 방식이다.
- 메시지 통신을 하는 시스템은 커널에 의해 send와 receive 연산을 제공받는다.

**직접 통신 (Direct Communication)**

- 통신하려는 프로세스의 이름을 명시적으로 표시한다.
- 각 쌍의 프로세스에게는 오직 하나의 링크만 존재한다.

![Untitled](https://www.notion.so/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2F510ce6c1-18bf-4145-a03f-74473e4ec36c%2FUntitled.png?table=block&id=4a275080-d756-4264-bd3c-d56073fd7946&spaceId=b453bd85-cb15-44b5-bf2e-580aeda8074e&width=1530&userId=80352c12-65a4-4562-9a36-2179ed0dfffb&cache=v2)
 
**간접 통신 (Indirect Communication)**

- 메시지를 메일 박스 또는 포트로부터 전달받는다.
- 각 메일 박스에는 고유의 ID가 있으며 메일 박스를 공유하는 프로세스만 통신할 수 있다.
- 하나의 링크가 여러 프로세스에게 할당될 수 있다.

![Untitled](https://www.notion.so/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2F9781ca48-a722-44ab-9fac-56591d8d7e54%2FUntitled.png?table=block&id=83c865b0-6d0f-4350-8ccd-8a8d20be0064&spaceId=b453bd85-cb15-44b5-bf2e-580aeda8074e&width=1530&userId=80352c12-65a4-4562-9a36-2179ed0dfffb&cache=v2)

### 공유 메모리 방식 (Shared Memory)

![Untitled](https://www.notion.so/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2F5b5de537-a950-4e0e-8037-fa8d5b94be76%2FUntitled.png?table=block&id=2f280d7c-20ab-424b-8a5c-0c285bfdb6aa&spaceId=b453bd85-cb15-44b5-bf2e-580aeda8074e&width=1530&userId=80352c12-65a4-4562-9a36-2179ed0dfffb&cache=v2)

- 공유 메모리 방식은 프로세스들이 주소 공간의 일부를 공유한다.
- 서로의 데이터에 일관성 문제가 유발될 수 있다.
- 프로세스 간 동기화 문제를 스스로 책임져야 한다.

<br>

# CPU Scheduler & Dispatcher

## CPU burst와 I/O burst

![Untitled](https://www.notion.so/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2F6931e79b-aa89-4619-9b72-ec30cbafa705%2FUntitled.png?table=block&id=02113bf4-a8c1-448e-932a-a23d56fd01dc&spaceId=b453bd85-cb15-44b5-bf2e-580aeda8074e&width=2000&userId=80352c12-65a4-4562-9a36-2179ed0dfffb&cache=v2)

### CPU burst

CPU 버스트는 사용자 프로그램이 CPU를 직접 가지고 빠른 명령을 수행하는 단계이다. 이 단계에서 사용자 프로그램은 CPU 내에서 일어나는 명령 (ex. Add)이나 메모리(ex. Store, Load)에 접근하는 일반 명령을 사용할 수 있다.

### I/O burst

I/O 버스트는 커널에 의해 입출력 작업을 진행하는 비교적 느린 단계이다. 이 단계에서는 모든 입출력 명령을 특권 명령으로 규정하여 사용자 프로그램이 직접 수행할 수 없도록 하고, 대신 운영체제를 통해 서비스를 대행하도록 한다.

이처럼 사용자 프로그램이 수행되는 과정은 CPU 작업과 I/O 작업의 반복으로 구성된다.

## CPU bound process와 I/O bound process

각 프로그램마다 CPU 버스트와 I/O 버스트가 차지하는 비율이 균일하지 않다. 어떤 프로세스는 I/O 버스트가 빈번해 CPU 버스트가 매우 짧은 반면, 어떤 프로세스는 I/O를 거의 하지 않아 CPU 버스트가 매우 길게 나타난다. 이와 같은 기준에서 프로세스를 크게 I/O 바운드 프로세스와 CPU 바운드 프로세스로 나눌 수 있다.

### I/O 바운드 프로세스

I/O 요청이 빈번해 CPU 버스트가 짧게 나타나는 프로세스를 말한다. 주로 사용자로부터 인터랙션을 계속 받아가며 프로그램을 수행하는 대화형 프로그램이 해당된다.

### CPU 바운드 프로세스

I/O 작업을 거의 수행하지 않아 CPU 버스트가 길게 나타나는 프로세스를 말한다. 주로 프로세스 수행의 상당 시간을 입출력 작업 없이 CPU 작업에 소모하는 계산 위주의 프로그램이 해당된다.

![Untitled](https://www.notion.so/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2F7afa61df-843f-4ec4-bbf3-c60882deabab%2FUntitled.png?table=block&id=7f83d7a1-91ac-4490-9bad-cfc20e08b92b&spaceId=b453bd85-cb15-44b5-bf2e-580aeda8074e&width=2000&userId=80352c12-65a4-4562-9a36-2179ed0dfffb&cache=v2)

프로그램이 수행되는 구조를 보면 I/O 바운드 프로세스는 짧은 CPU 버스트를 많이 가지고 있는 반면, CPU 바운드 프로세스는 소수의 긴 CPU 버스트로 구성된다는 것을 알 수 있다.

## CPU 스케줄링

컴퓨터 시스템 내에서 수행되는 프로세스의 CPU 버스트를 분석해보면 대부분의 경우 짧은 CPU 버스트를 가지며, 극히 일부분만 긴 CPU 버스트를 갖는다. 이는 다시 말해서 CPU를 한 번에 오래 사용하기보다는 잠깐 사용하고 I/O 작업을 수행하는 프로세스가 많다는 것이다. 즉 대화형 작업을 많이 수행해야 하는데, 사용자에 대한 빠른 응답을 위해서는 해당 프로세스에게 우선적으로 CPU를 할당하는 것이 바람직하다. 만약 CPU 바운드 프로세스에게 먼저 CPU를 할당한다면 그 프로세스가 CPU를 다 사용할 때까지 수많은 I/O 바운드 프로세스는 기다려야할 것이다. 이러한 이유로 CPU 스케줄링이 필요해졌다.

### CPU 스케줄러

CPU 스케줄러는 준비 상태에 있는 프로세스들 중 어떠한 프로세스에게 CPU를 할당할 지 결정하는운영체제의 코드이다.

**CPU 스케줄러가 필요한 경우**

- Running → Blocked (ex. I/O 요청하는 시스템 콜)
- Running → Running (ex. 할당 시간 만료로 인한 타이머 인터럽트)
- Blocked → Ready (ex. I/O 완료 후 인터럽트)
- Terminated

1, 4번째의 스케줄링은 강제로 빼앗지 않고 자진 반납하는 `nonpreemptive` 방식이고, 그 외의 스케줄링은 CPU를 강제로 빼앗는 `preemptive` 방식이다. 전자를 비선점형 스케줄링, 후자를 선점형 스케줄링이라고 부른다. 참고로 3번째 스케줄링은 Context Switch가 발생할 때만 해당한다.

### Dispatcher

CPU를 누구한테 누구한테 줄 지 결정했으면 해당 프로세스에게 넘겨야 하는데, 디스패처가 이 역할을 수행한다. 이 과정이 Context Switch에 해당한다.

### 스케줄링의 성능 평가 (Scheduling Criteria)

**시스템 입장**

- CPU utilization (이용률)
    - 전체 시간 중에서 CPU가 일을 한 시간의 비율
    - keep the CPU as busy as possible
- Throughput (처리량)
    - 주어진 시간동안 준비 큐에서 기다리고 있는 프로세스 중 몇 개를 끝마쳤는지 나타낸다.
    - 즉 CPU의 서비스를 원하는 프로세스 중 몇 개가 원하는 만큼의 CPU를 사용하고 이번 CPU 버스트를 끝내어 준비 큐를 떠났지 측정한 것이다.
    - 더 많은 프로세스들이 CPU 작업을 완료하기 위해서는 CPU 버스트가 짧은 프로세스에게 우선적으로 CPU를 할당하는 것이 유리하다.
    - number of processes that complete their execution per time until

**프로세스 입장**

- Turnaround time (소요 시간, 반환 시간)
    - 프로세스가 CPU를 요청한 시점부터 자신이 원하는 만큼 CPU를 다 쓰고 CPU 버스트가 끝날 때까지 걸린 시간을 뜻한다.
    - (준비 큐에서 기다린 시간) + (실제로 CPU를 사용한 시간)
    - amount of time to execute a particular process
- Waiting time (대기 시간)
    - CPU 버스트 기간 중 프로세스가 준비 큐에서 CPU를 얻기 위해 기다린 시간의 합을 뜻한다.
    - 시분할 시스템의 경우, 한 번의 CPU 버스트 중에도 준비 큐에서 기다린 시간이 여러 번 발생할 수 있다.
    - 이때 대기 시간은 CPU 버스트가 끝나기까지 준비 큐에서 기다린 시간의 합을 뜻하게 된다.
    - amount of time a process has been waiting in the ready queue
- Response time (응답 시간)
    - 프로세스가 준비 큐에 들어온 후 첫 번째 CPU를 획득하기까지 기다린 시간을 뜻한다.
    - 응답 시간은 대화형 시스템에 적합한 성능 척도로서, 사용자 입장에서 가장 중요한 성능 척도라고 할 수 있다.
    

<br>    







<br>

### 📚 출처

[반효경교수님 운영체제 강의](http://www.kocw.net/home/search/kemView.do?kemId=1226304&ar=relateCourse)

[운영체제 정리 깃헙](https://github.com/pjy1368/operating-system-study/tree/main/학습%20내용)


<br>


***

## Summary

***

<br>

# ⁉️ 면접 예상 질문

> 1. 스케줄링이 무엇인지와 목적을 설명해주세요.

> 2. 비선점방식과 선점방식을 설명해주세요.

> 3. 스케줄링 기법 중 FCFS의 단점과 해결 방법을 설명해주세요.












