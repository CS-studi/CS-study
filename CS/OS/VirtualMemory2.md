# Virtual Memory

> Page Replacement  
Page Replacement Algorithm  
Page Frame의 할당  
Page Size의 결정  
> 

<br/>

_"물리적인 메모리의 주소 변환은 운영체제가 관여하지 않는다.하지만 가상 메모리의 경우에는 운영체제가 전적으로 관리한다"_

# *Page Replacement*

page fault가 발생했을 때 disk에 있는 page를 물리적 메모리(page frame)에 적재해야한다. 만약 물리적 메모리에 빈 page frame 공간이 없다면, 메모리에 적재된 page frame들 중 하나를 골라 disk로 swap out하여 메모리에 빈 page frame 공간을 확보하는 것을 `page replacement` 라고 한다.

- page replacement는 운영체제가 담당하고 있다.
- page replacement를 할 때, 곧바로 사용되지 않은 page를 swap out하는 것이 바람직하다.

![Untitled.png](img/VirtualMemory2/Untitled%204.png)

1. swap out할 page frame인 victim을 결정하고 disk로 swap out한다. 
    - 만약 victim으로 선택된 page frame의 내용이 변경되었다면, 변경된 내용을 메모리에서 backing store(swap area)에 반영(write)해야한다.
    - 변경된 내용이 없다면 victim을 swap out만 해준다.
2. swap out된 victim page frame의 page table entry의 bit를 invalid로 변경한다.
3. empty page frame에 새로운 page를 적재한다.
4. 새롭게 메모리에 적재된 page에 대해 page frame number를 page table에 작성하고 bit를 valid 로 변경한다.

<br/>

# *Page Replacement Algorithm*

`Page Replacement Algorithm(페이지 교체 알고리즘)`이란 page replacement를 수행할 때, page fault rate을 최소화할 수 있는 victim page를 선택하는 알고리즘이다.

<br/>

> Replacement 알고리즘의 성능
> 

page reference string을 보고 선택한 replacement 알고리즘의 page fault rate을 구하여 성능을 평가한다.

<br/>

> Page Reference String 이란?
> 

page reference string이란 시간 순서에 따라 참조된 page 번호들의 나열을 의미한다. 

![Untitled](img/VirtualMemory2/Untitled%205.png)

<br/>

## 📌 Optimal Algorithm

미래에 참조될 page 번호(page reference string)를 미리 알고 있다는 가정이 전제된 알고리즘으로 가장 먼 미래에 참조될 page를 교체한다.

- page fault rate이 가장 작은 알고리즘이다.
- 실제 시스템에서 사용될 수 없기 때문에 Offline Optimal Algorithm으로 부른다. (Belady’s optimal algorithm, MIN, OPT 라고도 불린다)

<br/>

> 동작 방식
> 

![Untitled](img/VirtualMemory2/Untitled%206.png)

1. 1, 2, 3, 4번 페이지를 참조할 경우 page fault가 발생한다. (빨간색은 page fault가 났음을 의미한다)
2. 1, 2번이 다시 참조된다 → hit! (연보라색은 참조할 page가 메모리에 있음을 의미한다)
3. 5번이 참조되지만 메모리에 없고(page fault), 비어있는 page frame이 없다
    
    현재 시점 이후 **미래의 page reference string (`1 2 3 4` )을 참고하여 가장 먼 미래에 참조될 page를 교체한다** . → 1, 2, 3, 4 중 가장 먼 미래에 참조되는 4번 page를 swap out하고 그 자리에 5번 page를 적재한다.
    

<br/>

> Optimal 알고리즘은 페이지 교체 알고리즘 성능의 upper bound를 제공한다.
> 

Optimal 알고리즘의 경우 주어진 page reference string에 대해 총 6번의 page fault가 발생한다. 동일한 page referene string에 대해 모든 페이지 교체 알고리즘을 사용해도 page fault가 6번 보다 많이 발생한다

- Optimal 알고리즘은 다른 페이지 교체 알고리즘들의 성능에 대한 `upper bound` 를 제공한다. (Optimal 알고리즘보다 성능이 좋은 페이지 교체 알고리즘을 만들 수 없다는 의미)

<br/>

## 📌 FIFO(First In First Out) Algorithm

먼저 swap in된 page를 먼저 swap out하는 page 교체 알고리즘이다.

<br/>

> 동작 방식
> 

![Untitled](img/VirtualMemory2/Untitled%207.png)

<br/>

> FIFO Anomaly : More Frames ≠ Less Page Fault
> 

page frame을 늘려주면 보통의 페이지 교체 알고리즘의 성능은 좋아지만 FIFO 알고리즘은 오히려 성능이 저하된다 (9 page fault → 10 page fault). 이러한 기이한 현상을 `FIFO Anomaly(Belady's Anomaly)`라고 한다.

<br/>

## 📌 LRU(Least Recently Used) Algorithm

가장 덜 최근에 사용된(=**가장 오래 전에 참조된**) page를 먼저 swap out하는 page 교체 알고리즘이다. 

- 가장 최근에 참조된 페이지가 가까운 미래에 참조될 가능성이 높다고 판단하는 알고리즘이다.

<br/>

> FIFO 알고리즘과 LRU 알고리즘의 차이점
> 

`FIFO`는 가장 먼저 들어온 page를 먼저 swap out한다. 1번째로 들어온 page는 최근까지 참조했음에도 불구하고 swap out 대상이 된다.

`LRU`는 가장 오래전에 사용된 page를 swap out한다. 1번째로 들어온 page를 가장 최근에 참조했다면 swap out 대상에서 제외된다(멀어진다)

<br/>

> 동작 방식
> 

![Untitled](img/VirtualMemory2/Untitled%208.png)

1. 1, 2, 3, 4번 페이지를 참조할 경우 page fault가 발생한다.
2. 1, 2번 페이지를 다시 참조한다 → 이미 메모리에 적재되어 있다!(hit)
3. 5번 페이지를 참조한다 → page fault 발생 → victim page를 선택해야 한다.
    
    OPT 알고리즘과 다르게 LRU는 Online Replacement Algorithm이므로 **과거 page reference string(`3 4 1 2`)을 참고하여 vicim page를 선택해야한다.**
    
    가장 최근에 사용된 순서는 **`2 → 1 → 4 → 3`** 이므로 victim page는 3번 페이지이다. (FIFO 알고리즘의 경우 가장 먼저 들어온 1번 페이지가 victim이 된다.)
    

<br/>

## 📌 LFU(Least Frequently Used) Algorithm

참조 횟수(reference count)가 가장 적은 page를 swap out하는 page 교체 알고리즘이다.

- 과거에 참조 횟수가 많은 페이지는 미래에도 참조될 가능성이 높다고 판단하는 알고리즘이다.

<br/>

> 최저 참조 횟수를 가진 page가 여러개 있다면?
> 

LFU 알고리즘 자체는 최저 참조 횟수를 가진 여러 page들 중 **임의로** victim page를 선정한다. 

- 성능 향상을 위해 최저 참조 횟수의 page들 중 가장 오래전에 참조된 page를 victim으로 선정하도록 구현할 수 있다.

<br/>

> 장단점
> 

장점

- LRU 알고리즘은 직전의 참조 시점만 확인하지만 LFU는 장기적인 시간 규모를 확인하여 victim을 선정하기 때문에 page의 인기도(참조 횟수)를 정확히 반영할 수 있다.

단점

- LFU는 참조 시점의 최근성을 반영하지 못한다. (가까운 현재에는 거의 참조되지 않는 page일 지라도 과거에 많이 참조되었다면 victim으로 선정될 가능성이 적다)
- LFU알고리즘은 LRU알고리즘보다 구현이 복잡하다.

<br/>

> LRU vs LFU
> 

![Untitled.png](img/VirtualMemory2/Untitled%209.png)

page frame은 4개가 있고, page reference string은 `1 1 1 1 2 2 3 3 2 4 5` 이다. 이때 1,2,3,4번 페이지에서 victim을 선택해야한다.

- `LRU` : 1번 페이지를 swap out한다
    
    현재 시점을 기준으로 가장 최근에 사용된 페이지의 순서는 `**4 → 2 → 3 → 1`** 이므로 1번 페이지를 swap out 한다.
    
    - 단점 : LRU는 가장 마지막 참조 시점만 고려하기 때문에 페이지의 총 참조 횟수를 고려하지 못한다.
- `LFU` : 4번 페이지를 swap out한다
    
    1, 2, 3, 4번 페이지의 참조 횟수는 각각 4, 3, 2, 1번이다. 따라서 가장 참조 횟수가 적은 4번 페이지를 swap out한다.
    
    - 단점 : 참조 횟수만 고려하기 때문에 미래에 참조될 가능성을 고려하지 못한다.

<br/>

> LRU와 LFU 알고리즘의 구현
> 

![Untitled.png](img/VirtualMemory2/Untitled%2010.png)

- `LRU` → LinkedList : O(1)
    
    페이지들을 참조 시간 순서에 따라 LinkedList 형태로 줄을 세운다. (위에서 아래로 갈 수록 참조 시점이 최근이다.)
    
    - **page가 재참조된다면**
        
        참조 시점이 가장 최근이 되므로 해당 page를 LinkedList의 맨 끝으로 이동시킨다. → `O(1)`
        
    - **victim page를 선택한다면**
        
        LinkedList의 가장 위에 위치한 page를 선택하면 된다. → `O(1)` (LRU 알고리즘에서 victim page를 선택할 때 비교 연산이 필요없다)
        
- `LFU` → Heap : O(logN)
    
    LFU 알고리즘은 LinkedList로 구현할 수 없다. 
    
    - **page가 재참조 된다면**
        
        ![Untitled](img/VirtualMemory2/Untitled%2011.png)
        
        참조 횟수가 1 증가된다. 이후 나머지 page들과 각각 비교하여 LinkedList를 재정렬해야하기 때문에 최악의 경우 `O(N)` 시간복잡도를 가진다.
        
    
    따라서 LFU 알고리즘은 Heap으로 구현한다. (참조 횟수가 가장 적은 page가 root 노드에 위치한다.)
    
    - **page가 재참조된다면**
        
        참조 횟수가 1 증가된다. 이후 heapify를 통해 Heap을 재구성한다 → `O(logN)`
        
    - **victim page를 선택한다면**
        
        root 노드에 위치한 page를 선택하면 된다. → `O(1)`
        
        그리고 heapify를 통해 Heap을 재구성한다 → `O(logN)`
        

<br/>

## 🤨페이징 시스템에서 LRU, LFU 알고리즘을 적용할 수 있을까?

> 캐싱 기법이란?
> 

`캐싱 기법` 이란 한정된 빠른 공간(캐시)에 요청된 데이터를 저장해 두었다가 후속 요청시 캐시에 저장된 데이터를 서비스하는 기법이다.

캐싱 기법은 paging 시스템, cache memory, buffer caching, web caching 등 다양한 분야에서 사용된다.

- `CPU - cache memory - 메인메모리` : CPU와 메인메모리 사이에 존재하는 계층으로 CPU가 메인메모리에 직접 요청하기 전에 cache memory에 요청한 데이터가 존재하는지 확인한다.
- `파일시스템 - buffer caching - Disk` : 파일시스템에서 Read, Write 요청을 빠르게 제공하기 위한 캐싱 기법이다.(paging 시스템과 동일하게 빠른 매체를 cache로 여기고, 느린 매체를 disk로 여긴다)
- `웹 클라이언트- web caching - 웹 서버`: 웹 페이지에 대한 요청을 web cache에 저장해 두어 동일한 요청을 하면 저장한 결과를 제공한다.

<br/>

> 캐시 운영에 시간 제약이 있다.
> 

paging 시스템은 메모리라는 한정되고 빠른 공간에 요청된 page를 저장해두고, 메모리가 가득찼다면 Replacement 알고리즘을 통해 page를 교체한다.

Replacement 알고리즘의 시간 복잡도는 곧 캐시 운영 시간을 결정하고 캐시 운영에는 시간적 제약이 있다. 다시말해, Replacement 알고리즘에서 삭제할 항목을 결정하는 작업이 오래 걸린다면 실제 시스템에서 사용할 수 없다.

- buffer caching, web caching의 경우 `O(1) ~ O(logN)` 범위의 시간복잡도까지 허용한다.

<br/>

> paging 시스템에서 LRU, LFU 알고리즘을 적용할 수 있을까? → NO
> 

![Untitled.png](img/VirtualMemory2/Untitled%2012.png)

CPU가 매순간마다 프로세스A의 명령어을 수행하기 위해 논리적 주소를 물리적 주소로 변환해야 한다.

1. page table에서 명령어가 포함된 page가 valid/invalid한지 확인한다.
2. valid하면 물리적 메모리에 있는 page frame의 정보를 CPU로 읽어 들인다. (이 때 주소변환은 운영체제가 관여하지 않고 오로지 하드웨어적으로 일어난다.)
3. invalid하면 page fault로 인해 trap이 발생한다. CPU 제어권이 프로세스에서 운영체제로 넘어가고, disk IO를 통해 disk에 있는 page를 물리적 메모리에 적재한 뒤, 필요에 따라 replacement 알고리즘이 수행되고 page frame의 정보를 CPU로 읽어 들인다.

**`여기서 replacement 알고리즘을 수행하는 주체는 운영체제이다`**

- LRU 알고리즘을 사용한다면 운영체제는 page들의 참조 시점를 기반으로 victim page를 선택해야한다.
- LFU 알고리즘을 사용한다면 운영체제는 page들의 참조 횟수를 기반으로 victim page를 선택해야한다.

<br/>

**`이 때, 문제점은 운영체제가 page의 참조 시점이나 참조 횟수를 완벽하게 알 수 없다는 것이다.`**

- page fault가 발생하면
    
    CPU 제어권이 운영체제로 넘어가기 때문에 disk에서 page를 메모리로 swap in하기 때문에 page의 접근 시점을 알 수 있다.
    
- 반면에 참조하려는 page가 valid한 경우에는
    
    CPU 제어권이 운영체제로 넘어가지 않고 하드웨어적으로만 주소 변환이 이루어지기 때문에 해당 page에 대한 접근 시점을 운영체제가 알 수 없다.
    

<br/>

**`즉, paging 시스템(virtual memory 시스템)에서 운영체제는 page에 대해 반쪽짜리 정보만 알기 때문에 LRU, LFU 알고리즘을 실제 paging 시스템에서 사용할 수 없다. (buffer caching, web caching에서는 사용 가능)`**

그래서 실제 paging 시스템에서는 LRU, LFU대신 Clock 알고리즘을 사용한다.

<br/>

## 📌 Clock Algorithm

![Untitled](img/VirtualMemory2/Untitled%2013.png)

`Clock 알고리즘`은 reference bit가 0인 page를 찾을 때까지 포인터를 시계방향으로 한칸씩 이동하여 교체할 page를 고르는 알고리즘이다. (LRU의 근사 알고리즘으로서 Second chance 알고리즘, NUR(Not Used Recently), NRU(Not Recently Used) 등으로 불린다.)

- 포인터를 시계방향으로 이동하면서 reference bit가 1인 것을 모두 0으로 바꾼다.
    - page의 reference bit를 수정하는 작업은 운영체제가 아닌 하드웨어로 수행된다.
- 한 바퀴를 되돌아와서도(second chance) 해당 page의 reference bit가 0이면 교체된다.

> Clock 알고리즘의 개선방식 : reference bit와 modified bit(dirty bit)
> 
- `reference bit = 1` : 최근에 참조된(Read된) 페이지라는 의미이다.
- `modified bit = 1` : 최근에 변경된(Write된) 페이지라는 의미이다.

reference bit가 0인 페이지를 swap out할 때, 만약 modified bit가 1이면 backing store(swap area)에 변경된 내용을 반영해야 하기 때문에 Disk IO가 동반된다.

따라서 reference bit가 0이면서 modified bit가 0인 페이지를 우선적으로 swap out하는게 효율적이다.

<br/>

> 동작 방식
> 

![Untitled](img/VirtualMemory2/Untitled%2014.png)

- 각 사각형은 물리적 메모리에 적재된 page frame이다
- CPU가 특정 page를 참조하게 되면 하드웨어에 의해 해당 page의 reference bit가 1로 세팅된다.
- 운영체제는 포인터를 이동하면서 page의 reference bit를 확인한다.
    - reference bit = 1이면, reference bit를 0으로 세팅하고 다음 page의 reference bit를 검사한다.
    - second chance의 경우 (한 바퀴 되돌아온 경우) reference bit = 0이면, 해당 page를 swap out한다.
        
        이 때, modified bit가 0이면 그냥 swap out 하지만, 1이면 변경된 내용을 backing store에 반영해야 하기 때문에 Disk IO가 동반되어 오버헤드가 발생한다. 따라서 modified bit가 1이면 해당 page를 swap out하지 않고 modified bit를 0으로 변경한다.
        

<br/>

# *Page Frame의 할당*

프로그램을 원활하게 실행하기 위해서 page fault를 적게 발생하면서 일련의 페이지들을 메모리에 동시에 적재되는게 중요하다. 결국 각 프로세스에 할당되는 page frame의 수가 원활한 환경을 결정짓는다. 

이 때 프로세스에게 page frame을 얼마만큼 할당할 것인지에 대한 문제를 `Allocation Problem` 이라고 한다.

<br/>

> Allocation(할당)이 왜 필요할까?
> 

CPU는 하나의 명령을 실행할 때 프로세스의 주소 공간 중 code, data, stack이라는 각기 다른 영역을 참조한다. 다시 말해, CPU는 명령을 실행할 때 여러 페이지를 동시에 참조한다. **그렇기 때문에 원활한 명령어 수행을 위해 할당되어야 할 `최소 page frame 수`가 존재한다.**

- 예를 들어, 반복문을 구성하는 페이지들을 하나의 최소 할당량으로 간주하여 한꺼번에 메모리에 할당되는 것이 유리하다. 만약 최소한의 할당량이 정해지지 않는다면, 매 loop마다 page fault가 발생할 수 있다.
    
    for 문을 구성하는 페이지가 3개일 때, page frame을 3개 할당하면 반복문이 실행되는 동안 page fault가 발생하지 않는다. 하지만 2개의 page frame을 할당하면 실행될 때마다 page fault가 발생한다.
    

<br/>

> Page Frame Allocation Algorithm
> 
1. `Equal Allocation(균등 할당)` : 모든 프로세스에게 page frame을 균일하게 할당한다
2. `Proportional Allocation(비례 할당)` : 프로세스 크기에 비례하여 page frame을 할당한다
3. `Priority Allocation(우선순위 할당)` : 프로세스의 우선순위에 따라 page frame을 다르게 할당한다(당장 CPU에서 실행될 프로세스와 그렇지 않은 프로세스로 구분한다)

위와 같은 page frame 할당 알고리즘으로 page fault 발생률이 최소화 될 수 있도록 page frame을 할당해야 한다. 하지만 Replacement 알고리즘을 사용하다 보면 저절로 page fault를 최소화하는 방향으로 page frame이 할당되는 효과가 있다.

> Global Replacement vs Local Replacement
> 
- `Global Replacement` : 페이지 교체 시 다른 프로세스에게 할당된 page frame을 swap out하는 방법이다.
    - 여러 프로세스들과 경쟁하여 page frame을 많이 가질 때도 있고, 적게 가질 때도 있다.
    - FIFO, LRU, LFU, Working Set, PFF 알고리즘을 사용하면 Global Replacement 방식으로 동작한다.
- `Local Replacement` : 페이지 교체 시 프로세스에게 할당된 page frame 내에서 victim page를 골라 swap out하는 방법이다.
    - 프로세스에 page frame이 고정적으로 할당된다.
    - FIFO, LRU, LFU 알고리즘을 프로세스 별로 운영할 때 Local Replacement 방식으로 동작한다.

<br/>

## 🧩 Thrashing

프로세스의 원활한 수행에 필요한 최소한의 page frame을 할당 받지 못한 경우 `Thrashing` 이 발생한다.

![Untitled](img/VirtualMemory2/Untitled%2015.png)

X축은 메모리에 동시 올라와 있는 프로그램의 개수(degree of multiprogramming)이고, Y축은 CPU 이용률(CPU utilization)이다.

1. 메모리에 프로그램 하나만 올라와 있으면 CPU 이용률이 대단히 낮다.
    
    프로그램이 CPU를 사용하다가 IO 작업을 하면 blocked 상태가 되어 CPU 자원은 잉여 상태가 되기 때문에 CPU 이용률이 낮다.
    
2. 메모리에 프로그램이 여러 개 올라와 있다면 CPU 이용률이 높다. 
    
    일반적으로 메모리에 올라와 있는 프로그램의 수에 따라 CPU 이용률이 증가한다.
    
3. 메모리에 올라와 있는 프로그램의 수가 어느 지점을 넘기면 Thrashing이 발생하여 CPU 이용률이 떨어진다.
    
    

<br/>

> Thrashing의 굴레
> 

메모리에 올라와 있는 프로세스가 많아지면 

→ 각 프로세스에게 page frame이 적게 할당되어 page fault가 빈번히 발생한다

→ 이는 CPU 이용률을 낮추고 

→ 운영체제는 낮은 CPU 이용률을 보고 MPD(Multiprogramming Degree)를 높히기 위해 메모리에 더 많은 프로그램을 올리게 된다. 

→ 메모리에 올라온 프로그램이 더 많아지고 

→ 프로세스 마다 할당된 page frame 수가 더욱 감소하여 

→ page fault가 매우 빈번해지고 swap in/swap out이 많아진다 (IO작업이 많아진다) 

→ 결국 CPU 이용률이 낮아지고 

→ low throughput을 야기한다.

위와 같은 악순환을 `Thrashing` 이라고 한다.

<br/>

> Thrashing 예방법 → Working-Set Algorithm, PFF(Page-Fault Frequency) Algorithm
> 

MPD(Multiprogramming Degree)를 조절하여 각 프로그램이 원활한 수행을 위한 최소한의 page frame을 확보할 수 있도록 해줘야 한다. 이를 위해 `Working-Set Algorithm` 또는 `PFF Algorithm` 이 필요하다.

<br/>

## 🧩 Working-Set Algorithm

> Locality of Reference (참조의 지역성)
> 

`참조의 지역성`이란 프로그램이 특정 시간 동안 메모리의 일정 영역만 집중적으로 참조한다는 특성이다. 

- loop를 실행할 때, loop를 구성하는 page만 집중적으로 참조되고 function을 실행할 때, function을 구성하는 page만 집중적으로 참조 된다.

<br/>

> Locality Set, Working Set
> 

특정 시간에 집중적으로 참조되는 page들의 집합을 `Locality Set` 이라고 하고, Working-Set Algorithm에서 Locality Set을 `Working Set` 이라고 부른다. (일반적으로 Working Set이라는 용어를 사용한다)

<br/>

> Working-Set Model
> 

Working Set 모델에서는 프로세스의 Working Set 전체가 메모리에 올라와 있어야 수행되고, 그렇지 않을 경우 모든 page 을 반납한 후 swap out(suspended)하여 Thrashing을 방지한다.

- 특정 프로그램의 Working Set이 5개의 page으로 구성되는 경우에 할당 가능한 page이 3개 뿐이라면, 해당 프로세스는 전체 page를 반납하고 disk로 swap out된다.

<br/>

> Working-Set Algorithm
> 

![Untitled](img/VirtualMemory2/Untitled%2016.png)

특정 프로세스의 Working Set은 과거를 통해 추정해야 한다.

- 시각 Ti에서의 Working Set : `WS(Ti)` = `Time interval [Ti - Δ, Ti] 사이에 참조된 서로 다른 page들의 집합`
    
    프로세스가 Δ (window size)만큼의 과거 시간동안 참조한 page들을 Working Set이라 간주하여 Working Set에 포함된 page들을 메모리에서 유지하고 포함되지 않은 page들은 swap out시킨다. 
    
- Working Set의 크기는 매번 바뀐다 (`WS(t1) = {1, 2, 5, 6, 7}` → `WS(t2) = {3, 4}`)
    
    다시 말해, 참조된 page를 Δ시간 만큼만 유지하고 swap out 시킨다.
    

프로세스들의 Working Set size의 합이 page frame의 수보다 클 경우

- 일부 프로세스를 swap out시켜(MPD를 줄여서) 남은 프로세스의 Working Set을 우선적으로 충족시켜준다.

Working Set에 포함된 page들을 다 할당하고도 page frame이 남는다면

- swap out 되었던 프로세스들에게 Working Set을 할당하여 MPD를 높힌다.

<br/>

## 🧩 PFF(Page-Fault Frequency) Algorithm

![Untitled](img/VirtualMemory2/Untitled%2017.png)

특정 프로그램의 Page Fault Rate을 조사하여 page frame을 동적으로 할당해주는 알고리즘이다.

- 프로세스의 page fault rate이 높다면 해당 프로세스의 Working Set이 메모리에 보장되어있지 않다고 간주하여 page frame을 더 할당해준다.

page fault rate에 대해 상한값(upper bound), 하한값(lower bound)를 둔다.

- 해당 프로세스의 page fault rate이 상한값을 넘기면 page frame을 더 많이 할당해 준다.
- 해당 프로세스의 page fault rate이 하한값 보다 작으면 page frame 수를 줄인다.

메모리에 빈 page frame이 없다면 일부 프로세스를 disk로 swap out 시킨다.

<br/>

# *Page Size의 결정*

paging 시스템에서 프로세스의 주소 공간을 동일한 크기의 page 단위로 자르고, 물리적 메모리도 page와 동일한 크기의 page frame으로 자른다.

> page size가 작으면?
> 
- page의 개수가 많아지고, page table의 크기가 커진다
- 내부 조각의 개수가 감소한다.
- 필요한 정보만 메모리에 올라가기 때문에 메모리 이용이 효율적이다

<br/>

> page size가 크면?
> 
- Locality 활용 측면에서는 page size가 클수록 좋다.
- Disk Transfer의 효율성 측면에서 page size가 클수록 좋다.
    
    한 번의 디스크 헤더 움직임으로 많은 양의 내용을 읽어오는 것이 좋기 때문이다.
    

<br/>

# 참고

[반효경 교수님 운영체제 강의](http://www.kocw.net/home/cview.do?cid=3646706b4347ef09)

<br/>

# 면접 예상 질문

> 페이지 교체가 왜 필요한가요?
> 

> 페이지 교체 알고리즘에는 무엇이 있나요? 각각의 특징은?
> 

> FIFO와 LRU의 차이점은 무엇인가요?
> 

> LRU와 LFU을 비교해보세요. LRU와 LFU 알고리즘은 어떻게 구현되었는지 아시나요? 각 알고리즘의 시간 복잡도를 말해보세요
> 

> 페이징 시스템에서 LRU, LFU 알고리즘을 적용할 수 있나요? 그 이유는? 그럼 어떤 알고리즘을 페이징 시스템에 적용할까요?
> 

> Clock 알고리즘의 동작방식을 말해주세요
> 

> Thrashing이 무엇인가요? 이를 예방하는 방법에 대해 설명해주세요
> 

> Page 사이즈가 작으면 어떻게 될까요? 크면 어떻게 될까요?
>
