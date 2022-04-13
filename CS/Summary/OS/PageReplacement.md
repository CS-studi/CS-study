# Page Replacement

## Page Replacement(페이지 교체)

page fault가 발생하였고 empty page frame가 없을 때, 메모리에 적재된 page들 중 디스크로 swap out하여 empty page frame을 확보하는 과정을 `page replacement`라고 한다.

1. `page replacement algorithm으로 victim page를 선택하여 디스크로 swap out한다.` (victim page의 내용이 변경되었다면, 변경된 내용을 backing store에 write해야 함)
2. `page table에 swap out된 page의 valid/invalid bit를 “invalid”로 변경한다.`
3. `새로운 page를 empty page frame에 swap in한다.`
4. `page table에 swap in된 page의 valid/invalid bit를 “valid”로 변경하고 page frame 번호를 작성한다.`

## Page Replacement Algorithm(페이지 교체 알고리즘)

페이지 교체 시 page fault를 최소화할 수 있는 victim page를 선택하는 알고리즘이다.

- 페이지 교체 알고리즘의 성능은 페이지 부재율이 낮을수록 좋다
- Page Reference String(시간 순서에 따라 참조된 page 번호들의 나열)을 참고하여 victim page를 선정한다

```java
1. Optimal Algorithm
2. FIFO(First In First Out) Algorithm
3. LRU(Least Recently Used) Algorithm
4. LFU(Least Frequently Used) Algorithm
5. Clock Algorithm
```

### `Optimal Algorithm`

미래에 어떤 페이지가 참조되는지 미리 알고있는 가정하에 victim page를 선택하는 알고리즘

- 페이지 부재율이 가장 낮은 알고리즘이다. (페이지 교체 알고리즘 성능의 upper bound를 제공함)
- 실제 시스템에서 적용할 수 없다

<div align=center>
<img src="https://raw.githubusercontent.com/CS-studi/CS-study/master/CS/OS/img/VirtualMemory2/Untitled%206.png" width="700"/>
</div>



1. `1,2,3,4`  →1,2,3,4번 페이지를 최초로 참조할 경우 page fault 발생
2. `1,2` → 1,2번 페이지를 다시 참조할 경우 hit
3. `5` → 5번 페이지를 참조할 경우 page fault 발생 & empty page frame이 없음

현재 시점(5) 이후의 `미래 Page Reference String(1, 2, 3, 4, 5)을 참고`하여 가장 먼 미래에 참조될 4번 페이지를 victim page로 선정하여 교체한다.

### `FIFO(First In First Out) Algorithm`

먼저 swap in된 페이지를 victim page로 선택하는 알고리즘

<div align=center>
<img src="https://raw.githubusercontent.com/CS-studi/CS-study/master/CS/OS/img/VirtualMemory2/Untitled%207.png" width="700"/>
</div>

- `FIFO Anomaly(Belady's Anomaly)` : page frame을 늘려주면 페이지 부재율이 오히려 증가하는 이상현상

### `LRU(Least Recently Used) Algorithm`

가장 오래전에 참조된 페이지를 victim page로 선택하는 알고리즘

- `LRU` → 가장 최근에 참조한 페이지라면 victim page 대상에서 제외된다.
- `FIFO` → 최근까지 참조한 페이지일지라도 먼저 swap in되었다면 victim page의 대상이 된다.

<div align=center>
<img src="https://raw.githubusercontent.com/CS-studi/CS-study/master/CS/OS/img/VirtualMemory2/Untitled%208.png" width="700"/>
</div>


1. `1,2,3,4`  →1,2,3,4번 페이지를 최초로 참조할 경우 page fault 발생
2. `1,2` → 1,2번 페이지를 다시 참조할 경우 hit
3. `5` → 5번 페이지를 참조할 경우 page fault 발생 & empty page frame이 없음

현재 시점(5) 이전의 `과거 Page Reference String(3, 4, 1, 2)을 참고`하여 가장 오래전에 참조된 3번 페이지를 victim page를 선정하여 교체한다.

### `LFU(Least Frequently Used) Algorithm`

참조 횟수가 가장 적은 페이지를 victim page로 선택하는 알고리즘

- 최저 참조 횟수를 가진 페이지가 여러 개 있다면 임의로 victim page를 선택한다. (최저 참조 횟수를 가진 페이지 중 가장 오래전에 참조된 페이지를 victim으로 선택하도록 구현할 수 있다)

> LRU와 LFU 비교
> 

<div align=center>
<img src="https://raw.githubusercontent.com/CS-studi/CS-study/master/CS/OS/img/VirtualMemory2/Untitled%209.png" width="700"/>
</div>

| LRU | LFU |
| --- | --- |
| 직전의 page reference string만 참고하기 때문에 페이지의 인기도(참조횟수)를 정확이 반영하기 힘들다 | 장기적인 시간 규모의 page reference string을 참고하기 때문에 페이지의 인기도를 반영할 수 있다 |
| 참조 시점의 최근성을 잘 반영한다 | 참조 시점의 최근성을 반영하기 힘들다. (과거에 많이 참조되었고 최근에 거의 참조되지 않았다면 victim으로 선택될 가능성이 적다) |

> LRU와 LFU 알고리즘 구현 방식
> 

<div align=center>
<img src="https://raw.githubusercontent.com/CS-studi/CS-study/master/CS/OS/img/VirtualMemory2/Untitled%2010.png" width="700"/>
</div>

LRU 알고리즘

- `LinkedList` → rear에 가장 최근에 참조된 페이지, head에 가장 오래 전에 참조된 페이지
- `page가 재참조된다면` → 재참조된 페이지를 rear로 이동시킨다. `O(1)`
- `victim page를 선택한다면` → head에 위치한 page를 선택한다. `O(1)`

*만약 LFU 알고리즘을 LinkedList로 구현한다면, page가 재참조될 경우 page들의 참조 횟수 정보를 모두 비교하면서 위치를 지정해야하기 때문에 O(N)의 시간복잡도를 갖게된다. 따라서 LFU는 Heap으로 구현한다.*

LFU 알고리즘

- `Heap` → 참조횟수가 가장 적은 페이지를 root node에 위치시킨다
- `page가 재참조된다면` → 해당 페이지의 참조 횟수를 1증가하고 heapify를 진행한다. `O(logN)`
- `victim page를 선택한다면` → root node에 위치한 page를 선택한다. 이후 heapify를 진행한다. `O(logN)`

> 실제 페이지 시스템에서 LRU, LFU 알고리즘을 적용할 수 없다.
> 

운영체제는 `LRU 알고리즘을 사용한다면 page들의 참조 시점를 기반으로`/`LFU 알고리즘을 사용한다면 page들의 참조 횟수를 기반으로` victim page를 선택해야한다.

***문제는 운영체제가 page의 참조 시점이나 참조 횟수를 완벽하게 알 수 없다는 점이다.***

page fault가 발생하면 운영체제가 CPU 제어권을 가지고 새로운 page를 swap in하기 때문에 “참조 시점”을 알 수 있다. 반면에 page fault가 발생하지 않는다면 CPU 제어권이 운영체제로 넘어가지 않고 하드웨어(MMU)를 통해 주소 변환이 이뤄지므로 valid page의 “참조 시점”을 알 수 없다.

결국 실제 페이징 시스템에서 LRU, LFU 알고리즘을 적용하지 못한다. 그 대신 Clock 알고리즘을 사용한다.

### `Clock Algorithm`

포인터를 시계방향으로 한 칸씩 이동하며 victim page를 선택하는 알고리즘

- `reference bit = 1` : 최근에 참조된(Read된) 페이지라는 의미
- `modified bit = 1` : 최근에 변경된(Write된) 페이지라는 의미

<div align=center>
<img src="https://raw.githubusercontent.com/CS-studi/CS-study/master/CS/OS/img/VirtualMemory2/Untitled%2014.png" width="700"/>
</div>

1. CPU가 페이지를 참조하면 해당 페이지의 reference bit를 1로 설정한다.
2. 운영체제는 포인터를 이동시키면서 페이지의 reference bit가 1인 것을 0으로 변경한다.
3. 두 번째 바퀴에서(second chance)
    - `reference bit = 1, modified bit = 0` → swap out
    - `reference bit = 1, modified bit = 1` → modified bit를 0으로 변경하고 포인터를 한 칸 이동한다. (변경된 내용을 disk에 write해야 하기 때문에)

## Page Frame 할당

CPU는 명령을 실행할 때 여러 페이지(프로세스의 code, data, stack)를 동시에 참조한다. 따라서 ***원활한 명령어 수행을 위해*** 페이지 부재율이 적게 발생할 만큼의 page frame(physical memory)을 할당해주어야 한다.

1. `Equal Allocation(균등 할당)` : 모든 프로세스에게 page frame을 균일하게 할당한다
2. `Proportional Allocation(비례 할당)` : 프로세스 크기에 비례하여 page frame을 할당한다
3. `Priority Allocation(우선순위 할당)` : 프로세스의 우선순위에 따라 page frame을 다르게 할당한다(당장 CPU에서 실행될 프로세스와 그렇지 않은 프로세스로 구분한다)

> Thrashing(스레싱)
> 

프로세스의 원활한 수행에 필요한 최소한의 page frame을 할당 받지 못한 경우 `Thrashing` 이 발생한다.

1. 메모리에 올라와 있는 프로세스가 많아지면
2. 각 프로세스마다 할당되는 page frame 수가 적어지고
3. 결국 페이지 부재가 빈번히 발생하여 CPU 이용률이 낮아진다. (잦은 Context Switch)
4. 운영체제는 낮은 CPU 이용률을 보고 degree of multiprogramming을 높히기 위해 메모리에 더 많은 프로세스를 올린다.
5. 프로세스마다 할당되는 page frame 수가 더욱 감소하고 → 페이지 부재율이 더 높아지고 → CPU 이용률이 더 낮아진다.

이를 해결하기 위해 각 프로그램마다 필요한 최소한의 page frame을 할당해주어야 한다. → `Working-Set Algorithm` 또는 `PFF Algorithm`