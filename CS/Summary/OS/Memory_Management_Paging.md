# Memory Management

### 가상 주소 vs 물리적 주소
- 가상(논리적) 주소(virtual address):
    - 프로그램이 실행을 위해 메모리에 적재되면 그 프로세스를 위한 독자적인 주소 공간이 생성된다.
    - 각 프로세스마다 0번지부터 시작한다.
    - CPU가 보는 주소이다.
- 물리적 주소(physical address):
    - 메모리에 실제로 올라가는 위치이다.
    - 보통 물리적 메모리의 낮은 주소 영역에는 운영체제가 올라가고, 높은 주소 영역에는 사용자 프로세스들이 올라간다.

> MMU
논리적 주소를 물리적 주소로 매핑해주는 하드웨어 장치

### MMU 기법
MMU를 사용해 가장 기본적인 방식으로 주소 변환을 수행하는 MMU 기법

- CPU가 특정 프로세스의 가상 주소를 참조하려고 할 때 MMU 기법은 그 주소값에 기준 레지스터(base register)의 값을 더해 물리적 주소값을 얻어낸다.
- 기준 레지스터는 해당 프로세스의 물리적 메모리 시작 주소를 가지고 있다.
- 문맥교환으로 CPU에서 수행 중인 프로세스가 바뀔 때마다 기준 레지스터의 값을 그 프로세스에 해당되는 값으로 재설정함으로써 각 프로세스에 맞는 위치에 접근하는 것을 지원한다.
- 한계 레지스터는 프로세스가 자신의 주소 공간을 넘어서는 메모리 참조를 하려고 하는지 체크하는 용도로 사용되며, 현재 CPU에서 수행 중인 프로세스의 논리적 주소의 최댓값, 즉 그 프로세스의 크기를 담고 있다.

## 물리적 메모리 할당 방식
- 물리적 메모리는 운영체제 상주 영역과 사용자 프로세스 영역으로 나뉜다.
    - 운영체제 상주 영역은 인터럽트 벡터와 함께 낮은 주소 영역을 사용한다.
    - 사용자 프로세스 영역은 높은 주소 영역을 사용한다.
- 사용자 프로세스 영역의 할당 방법
    - 연속 할당
        - 각각의 프로세스가 메모리의 연속적인 공간에 적재되도록 하는 것이다.
        - 고정 분할 방식과 가변 분할 방식이 존재한다.
    - 불연속 할당
        - 하나의 프로세스가 메모리의 여러 영역에 분산되어 올라가는 것이다.
        - Paging, Segmentaing, Paged Segmentation 방식이 존재한다.

### 연속 할당

### 고정 분할 방식
- 메모리를 주어진 개수만큼의 영구적인 파티션으로 미리 나누어두고 각 파티션에 하나의 프로세스를 적재해 실행한다.
- 동시에 메모리에 올릴 수 있는 프로그램의 수가 고정되어 있으며 수행 가능한 프로그램의 최대 크기 또한 제한된다는 점에서 융통성이 떨어진다.
- 외부 조각과 내부 조각이 발생할 수 있다.

### 가변 분할 방식
- 메모리에 적재되는 프로그램의 크기에 따라 파티션의 크기, 개수가 동적으로 변하는 방식이다.
- 고정 분할 방식과 달리 미리 메모리 영역을 나누어 놓지 않는다.
- 프로그램이 실행될 때마다 차곡차곡 메모리에 올리므로 내부 조각이 발생하지 않는다.
    - 다만 중간에 프로그램이 종료되어 메모리에서 빠져 나가고, 그 공간에 새로운 프로그램이 메모리에 할당될 경우 외부 조각이 발생할 수 있다.

### Hole
- 가용 공간을 의미한다.
- 가용 공간이란 사용되지 않은 메모리 공간으로서 메모리 내의 여러 곳에 산발적으로 존재할 수 있다.
- 프로세스가 도착하면 수용 가능한 hole을 할당해야 한다.
- 운영 체제는 이미 사용 중인 메모리 공간인 할당 공간과 사용하고 있지 않은 가용 공간에 대한 정보를 유지하고 있다.

## 동적 메모리 할당 문제
- 가변 분할 방식에서 주소 공간의 크기가 n인 프로세스를 메모리에 올릴 때 물리적 메모리 내의 가용 공간 중 어떤 위치에 올릴 것인지 결정하는 문제이다.
- First-fit
    - size가 n 이상인 것 중 최초로 찾아지는 hole에 할당한다.
- Best-fit
    - size가 n 이상인 가장 작은 hole을 찾아서 할당한다.
    - Hole들의 리스트가 크기 순으로 정렬되지 않은 경우 모든 hole을 탐색해야 한다.
    - 많은 수의 아주 작은 hole이 생성된다.
- Worst-fit
    - 가장 큰 hole에 할당한다.
    - 역시 hole을 탐색해야 한다.
    - 상대적으로 아주 큰 hole이 생성된다.
- First-fit과 Best-fit이 Worst-fit보다 속도와 공간 이용률 측면에서 효과적이다.

## 페이징 기법
- 프로세스의 주소 공간을 동일한 크기의 페이지 단위로 나누어 물리적 메모리의 서로 다른 위치에 페이지를 저장하는 방식이다.
- 각 프로세스의 주소 공간 전체를 물리적 메모리에 한꺼번에 올릴 필요가 없고, 일부는 backing storage, 일부는 물리적 메모리에 혼재하는 것이 가능하다.
- 물리적 메모리를 페이지와 같은 동일한 크기의 프레임으로 미리 나누어 둔다.
메모리에 올리는 단위가 동일한 크기의 페이지 단위이므로 외부 조각이 발생하지 않고, 동적 메모리 할당 문제도 고려할 필요가 없다.
- 페이지 테이블을 사용하여 논리적 주소를 물리적 주소로 변환하는 작업이 필요하다.
- 프로그램의 크기가 항상 페이지 크기의 배수가 된다는 보장이 없으므로 프로세스의 주소 공간 중 제일 마지막에 위치한 페이지에서는 내부 조각이 발생할 수 있다.

### 주소 변환 기법
- 페이징 기법에서는 CPU가 사용하는 논리적 주소를 페이지 번호(p)와 페이지 오프셋(d)으로 나누어 주소 변환에 사용한다.
- 페이지 번호는 각 페이지별 주소 변환 정보를 담고 있는 페이지 테이블 접근 시 인덱스로 사용되고, 해당 인덱스의 항목에는 그 페이지의 물리적 메모리 상의 기준 주소, 즉 시작 위치가 저장된다.
- 따라서 특정 프로세스의 p번째 페이지가 위치한 물리적 메모리의 시작 위치를 알고 싶다면 해당 프로세스의 페이지 테이블에서 p번째 항목을 찾아보면 된다.
- **f (물리적 주소의 시작 위치) + d**를 취하면 처음에 CPU가 요청한 논리적 주소에 대응하는 물리적 주소가 된다.


### 페이지 테이블 구현
- 페이지 테이블은 페이징 기법에서 주소 변환을 하기 위한 자료 구조로, 물리적 메모리인 Main Memory에 상주한다.
- 현재 CPU에서 실행 중인 프로세스의 페이지 테이블에 접근하기 위해 2개의 레지스터를 사용한다.
    - 페이지 테이블 기준 레지스터(page-table base register)
        - 메모리 내에서 페이지 테이블의 시작 위치를 가리킴
    - 페이지 테이블 길이 레지스터(page-table length register)
        - 페이지 테이블의 크기를 보관함
- 페이징 기법에서 모든 메모리 접근 연산은 총 2번씩 필요하다.
    - 주소 변환을 위해 페이지 테이블에 접근
    - 변환된 주소에서 실제 데이터에 접근
    - 이러한 오버헤드를 줄이고 메모리의 접근 속도를 향상하기 위해 **TLB(Translation Look-aside Buffer)**라고 불리는 고속의 주소 변환용 하드웨어 캐시를 사용한다.

## 계층적 페이징
- 현대의 컴퓨터는 주소 공간이 매우 큰 프로그램을 지원한다
- 그러나 대부분의 프로그램은 4G의 주소 공간 중 지극히 일부분만 사용하므로 페이지 테이블 공간이 심하게 낭비된다.
- 위 문제로 인하여 페이지 테이블 자체를 페이지로 구성하는 2단계 페이징 기법을 사용한다.

### 2단계 페이징 기법
- 주소 변환을 위해 외부 페이지 테이블과 내부 페이지 테이블의 두 단계에 걸친 페이지 테이블을 사용한다.
- 사용하지 않는 주소 공간에 대해서는 외부 페이지 테이블의 항목을 NULL로 설정하며, 여기에 대응하는 내부 페이지 테이블을 생성하지 않는다.
- 페이지 테이블을 위해 사용되는 메모리 공간을 줄이지만, 페이지 테이블의 수가 증가하므로 시간적인 손해가 뒤따른다.

### 역페이지 기법
- 페이지 테이블로 인한 메모리 공간의 낭비가 심한 이유
    - 모든 프로세스의 모든 페이지에 대한 페이지 테이블 항목을 구성해야 함.
- 이 문제를 해결하기 위해 역페이지 기법을 사용할 수 있다.
    - 물리적 메모리의 페이지 프레임 하나당 페이지 테이블에 하나씩의 항목을 두는 방식이다.
    - 논리적 주소에 대해 페이지 테이블을 만드는 것이 아니라, 물리적 주소에 대해 페이지 테이블을 만드는 것이다.
    - 시스템 전체에 페이지 테이블을 하나만 둔다.
    - 페이지 테이블의 각 항목은 어느 프로세스의 어느 페이지가 이 프레임에 저장되었는 지의 정보를 보관하고 있다.
        - 페이지 테이블의 각 항목은 프로세스 번호(pid)와 그 프로세스 내의 논리적 페이지 번호(p)를 담고 있다.
    - 페이지 전체를 탐색해야 하는 단점이 있다.
        - 따라서 일반적으로 연관 레지스터를 사용하여 병렬 탐색을 수행한다