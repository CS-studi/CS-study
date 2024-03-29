# 🌳 Data Structure
- [Array Vs Linked List](DataStructure/ArrayVSLinkedList.md) 👈
- [Stack and Queue](DataStructure/StackAndQueue.md)
- [Tree](DataStructure/Tree.md)
    - Binary Tree
    - Full Binary T
    - Complete Binary Tree
    - BST
- [Heap](DataStructure/Heap.md)
    - min Heap
    - max Heap
- [Red-Black Tree](DataStructure/Red-BlackTree.md) 👈
    - 정의
    - 특징
    - 삽입
    - 삭제
- [Hash Table](DataStructure/HashTable.md)
    - Hash Function
    - Resolve Collision
        - Open Addressing
        - Separate Chaining
    - Resize
- [Graph](DataStructure/Graph.md)
    - Graph 용어 정리
    - Graph 구현
    - Graph 탐색
    - [Minimum Spanning Tree](DataStructure/MinimumSpanningTree.md) 👈
        - Kruskal algorithm
        - Prim algorithm
<br>

# 🌌 Network 
- [HTTP Method](Network/HTTPMethod.md) 👈
- [Rest and RestAPI](Network/Rest.md)
- [HTTP 와 HTTPS 의 차이점 & HTTP 의 문제점들](Network/HttpHttps.md)
- [SSL, 공개키/비공개키](Network/SSL.md)
- [TCP 3-way-handshake](Network/TCP_3way_handshake.md)
- [TCP 와 UDP 의 차이점 + (QUIC)](Network/TCP_UDP_QUIC.md)
- [DNS round robin 방식](Network/DNSRoundRobin.md)
- [웹 통신의 큰 흐름](Network/웹통신의큰흐름.md)
- [네트워크 시스템의 Layer and Architecture](Network/Network_Layer_Architecture%20.md) 👈
- [쿠키, 세션, jwt 토큰](Network/CookieSessionJWT.md)
- [프록시](Network/Proxy.md) 
- [소켓 통신](Network/socket.md)
- [CORS (Cross Origin Resource Sharing)](Network/CORS.md)
- [OAuth](Network/oauth.md)

<br>

# 💿 Database 
- [데이터베이스](Database/Database.md) 👈
    - 데이터베이스를 사용하는 이유
    - 데이터베이스 성능
- [RDBMS vs. NoSQL](Database/RDBMSvsNOSQL.md)
- [ElasticSearch](Database/ElasticSearch.md) 👈
- [Redis](Database/Redis.md)
- [SQL](Database/SQL.md)
    - SQL
    - JOIN
    - SQL Injection
- [DB 클러스터링, 리플리케이션](Database/ClusteringReplicationShardingPartitioning.md)
- [Transaction](Database/Transaction.md) 👈
    - Transaction을 사용하는 이유
    - ACID
    - 트랜잭션 격리 수준
    - 트랜잭션 격리 수준을 설정할 때 발생하는 문제점들
- [DB 교착상태 & 동시성 제어](Database/DB_DeadLock_ConcurrencyControl.md)
- [Regularization(정규화)](Database/Regularization.md)
- [Index & Hint](Database/Index.md)
    - 순차 I/O, 랜덤 I/O
    - B-Tree Index, Hash Index, InnoDB Adaptive Hash Index
    - 인덱스 레인지 스캔, 인덱스 풀 스캔
    - 클러스터링 인덱스, 논 클러스터링 인덱스
    - Hint
<br>

# 🫁 OS 
- [Process, Thread](OS/ProcessThread.md)👈
    - 프로세스의 개념, 프로세스의 상태(Process State)
    - Process Control Block(PCB), 문맥교환(Context Switch)
    - 프로세스를 스케줄링하기 위한 큐
    - Single and Multithreaded Processes
    - 프로세스 생성
- [CPU 스케줄러 & 스케쥴링 알고리즘](OS/CPU_Scheduler_Algorithm.md)
    - CPU Scheduler & Dispatcher
    - FCFS, SJF, Priority Scheduling, Round Robin
- [프로세스 동기화](https://github.com/CS-studi/CS-study/blob/master/CS/OS/processSynchronization.md)
    - Critical Section
    - 해결책
        - Lock
        - Semaphores
        - 모니터
- [동기와 비동기 & 블로킹과 논블로킹](OS/SyncAsyncBlockNonblock.md) 👈
- [Deadlock](OS/deadlock.md)
    - Deadlock 발생의 4가지 조건
    - Deadlock의 처리 방법
    - Deadlock Avoidance
    - Banker's Algorithm
- [메모리 관리 전략(1)](OS/Memory_Management_Paging.md)
    - 배경
    - paging
        - Dynamic Relocation
        - Multilevel Paging
        - Shared Page
- [메모리 관리 전략(2)](OS/Memory_Management_Segmentation.md)
    - Segmentation
        - Segmentation Architecture
        - Segmentation with Paging
- Virtual Memory(1) 👈
    - Demand Paging, Memory에 없는 Page의 Page Table, Page Fault
    - Page Replacement
- [Virtual Memory(2)](OS/VirtualMemory2.md)
    - Page replacement Algorithm
        - Optimal, FIFO , LRU, LFU
    - LRU와 LFU 알고리즘의 구현
- [캐시의 지역성](OS/Cache.md) 
    - Locality
    - Caching line
- [File system](OS/fileSystem.md)
    - Allocation of File Data in Disk
    - UNIX 파일시스템의 구조
    - Page Cache and Buffer Cache
    
<br>

# 🍦Software Engineering Curriculum
- [Agile, waterfall, CICD + devOps](SoftwareEngineering/Agile_Waterfall_CICD_Devops.md)
- [TDD](SoftwareEngineering/TDD.md)
- [Monolithic vs. MSA](SoftwareEngineering/MicroserviceArchitecture.md)
- [MVVM, MVC 패턴](SoftwareEngineering/MVC_MVVM.md)
- [Docker & Kubernetes](SoftwareEngineering/dockerKubernetes.md)
- [AWS, cloudflare, GCP](SoftwareEngineering/AWS_GCP_Cloudflare.md)

<br>

# 🖥Computer Architecture
- ARM & AMD 프로세서
- Floating Point
- 컴퓨터의 구조
- CPU 작동 원리