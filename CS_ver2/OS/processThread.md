# Process & Thread

## Process
> 실행되고 있는 프로그램

우선 디스크에 실행파일 형태로 존재하는 __프로그램__ 이 메모리에 적재되어 실행되면 생명력을 가진 프로세스가 된다.

<br>

### Process Context
> 여러 프로세스가 실행중인 시분할 환경에서 자주 cpu 점유권을 빼앗기고 획득하게 된다. cpu 점유권을 다시 획득한 프로세스는 이전의 상태를 재현하기 위해 "process context"를 이용한다.

- CPU의 수행 상태 => pc값, 레지스터 정보 (하드웨어)
    - 현재 실행중인 프로세스의 명령어의 위치를 저장하는 pc값을 저장
- 프로세스 주소 공간 => code, data, stack
- 프로세스 관련 커널 자료구조 => PCB, Kernel stack
    - PCB
        - 운영체제 커널의 자료구조. 프로세스의 정보를 저장하고 있고, 실행중인 프로세스를 관리하기 위해 운영체제가 참고하는 자료이다.
    - Kernel stack
        - 프로세스가 시스템 콜을 하면 PC가 kernel의 주소 공간의 code를 가리키며 명령어를 수행한다.

<br>

### Process state
> 단일 CPU 기준 여러 프로세스를 수행하며 프로세스의 상태가 변한다.

- Running
    - CPU를 잡고 명령어를 수행
- Ready
    - 메모리에 적재되어 바로 실행될 수 있는 조건을 만족한 프로세스가 CPU 점유권을 얻지 못해 대기중인 상태
- Blocked
    - CPU를 할당받더라도 당장 명령을 실행할 수 없는 상태
- Suspended (Stopped)
    - 외부적인 이유로 프로세스의 수행이 정지된 상태
- New
    - 프로세스가 생성 중인 상태
- Terminated
    - 프로세스의 수행이 끝난 상태

<br>

### Process Context Switch

<br>