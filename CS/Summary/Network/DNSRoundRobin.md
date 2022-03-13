# DNS Round Robin

## Round Robin

`RR(Round Robin)`은 선점형 CPU 스케쥴링 알고리즘의 한 종류이다.

- 각 프로세스가 동일한 크기의 할당 시간 “time quantum”을 가지고 작업을 수행한다. 할당 시간이 만료되면 프로세스는 CPU 점유권을 빼앗긴다.

<br/>

## DNS

`DNS(Domain Name System)`이란 사람이 쉽게 기억할 수 있는 Domain Name를 IP 주소로 변환해주는 시스템이다.

<br/>

## DNS Round Robin

`DNS Round Robin` 이란 **별도의 Load Balancing 장비 없이 오직 DNS만을 이용하여** Domain Record를 조회하는 시점에서 트래픽을 분산하는 기법이다.

1. 클라이언트가 네이버의 Domain Name(`www.naver.com`)을 URL에 입력한다
2. DNS 서버는 Domain Name에 해당하는 서비스(네이버)를 제공하는 웹서버의 IP 주소를 찾는다.
3. 동일한 서비스(네이버)를 제공하는 **웹서버가 여러대 있는 경우 DNS Round Robin 방식으로 고른다**
4. 고른 웹 서버의 IP 주소 리스트를 클라이언트에게 반환한다.
5. 클라이언트의 OS에 따라 IP 주소 리스트 중 하나를 선택한다.

위와 같은 과정을 통해 다수의 클라이언트는 여러 웹 서버에 나뉘어 접속하게 된다.

<br/>

## DNS Round Robin의 문제점

1. `IPv4 주소 고갈 문제`
    
    부하 분산을 위해 웹 서버의 수를 늘리면 그만큼의 공인 IP가 필요하다. (DNS Round Robin을 사용하여 발생하는 문제는 아닌거 같음)
    - 사설 IP를 사용하는게 IPv4 주소 고갈 문제를 해결할 수 있지만, DNS Round Robin을 활용하기 위해선 웹 서버마다 공인 IP 주소를 사용해야 한다.
    
2. `프록시 서버의 DNS 쿼리 결과 캐싱 문제`
    
    DNS 서버가 DNS Round Robin 방식으로 선택한 IP 주소 리스트를 프록시 서버에서 캐싱한다면, 해당 프록시 서버를 경유하는 모든 클라이언트는 동일한 웹 서버에 접속하게 되므로 적절하게 부하 분산되지 않는다.
    
3. `서버의 상태를 감지하지 못한다`
    
    여러 웹서버 가운데 몇몇이 다운되어도 DNS 서버는 이를 감지하지 못한 채 클라이언트에게 IP 주소를 제공한다. 이 때문에 유저들이 간혹 다운된 서버로 연결되기도 한다.
    

<br/>

## DNS Round Robin 문제 해결방법

Round Robin이 아닌 다른 DNS 스케쥴링 알고리즘을 사용하여 해결한다.

- `Weighted Round Robin`
    - 각 웹 서버에 가중치를 부여하여 분산 비율을 조정하는 기법이다. (가중치가 큰 서버가 빈번하게 선택되기 때문에 처리 능력 순으로 가중치를 높에 설정하는게 좋음)
- `Least Connection`
    - 접속된 클라이언트의 수가 가장 적은 웹 서버를 선택하는 기법이다. (Load Balancer에서 실시간으로 connection 수를 관리하거나 각 서버에서 주기적으로 접속된 클라이언트의 수를 알려줘야 한다)
