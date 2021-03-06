# 네트워크 시스템의 Layer and Architecture

<br/>

## 인터넷 프로토콜 스택 5계층

`Protocol` : 컴퓨터나 원거리 통신 장비 사이에서 메시지를 주고 받는 양식과 규칙
- 서로 다른 환경에서 데이터의 형태를 일정하게 규정하여 충돌 및 지연 등의 문제를 방지한다

`Protocol Stack` : 다양한 계층의 프로토콜들을 모두 합친 것
- 하나의 프로토콜에서 모든 작업을 수행하지 않고 다수의 통신 프로토콜을 사용한다. (데이터 처리 시스템에서 통신의 복잡도를 높히기 때문)

`인터넷 프로토콜 스택 5계층`

1. `응용 계층` : 사용자 관점에서 여러 하위 통신 프로토콜들에 대한 사용자 인터페이스를 제공하는 계층
    - `PDU` : 메시지
    - `프로토콜` : HTTP(웹 문서 및 request 전송), SMTP(전자 메일 전송), FTP(파일 전송)
    - 전송 계층 프로토콜을 기반으로 호스트 간 연결을 확립

2. `전송 계층` : Endpoint 간에 응용 계층의 메시지를 신뢰성 있게 전달하기 위해 논리적 통신 서비스를 제공하는 계층 (Endpoint는 클라이언트와 서버를 의미)
    - `PDU` : 세그먼트(TCP), 데이터그램(UDP)
    - `프로토콜` : TCP, UDP
    - Port 번호에 해당하는 응용 프로세스에 데이터를 전달함(Port 번호로 동일 호스트 내에 프로세스를 구분함)

3. `네트워크 계층` : 논리적 주소 체계에 따라 라우팅을 통해 호스트들 사이에 패킷 전달을 제공하는 계층
    - `PDU` : 데이터그램 (IP 패킷이라고 많이 부르지만, IP 데이터그램이라고 부르는 것이 더 정확함, [참고](https://itwiki.kr/w/IP_%EB%8D%B0%EC%9D%B4%ED%84%B0%EA%B7%B8%EB%9E%A8))
    - `프로토콜` : IP
    - 📌 `논리적 주소 체계` : IP 주소를 의미함.
    - `포워딩` : 다음 라우터에게 IP 데이터그램을 넘겨주는 것.
    - `라우팅` : IP 데이터그램을 한 번에 하나의 라우터 씩 이동시키면서 목적지 호스트까지의 최적의 경로를 찾는 방식.
4. `데이터링크 계층` : 장치 간 전기 신호를 전달하는 물리 계층을 이용하여 동일 네트워크 상의 주변 장치들끼리 전송 에러 없이 통신할 수 있도록 하는 계층
    - `PDU` : 프레임
    - `프로토콜` : Ethernet, WiFi
    - 상위 계층(네트워크 계층)이 불안정한 물리 계층을 신뢰할 수 있도록 전송 에러를 없앤다
        - 프레이밍(데이터를 프레임으로 나누어 그룹화), 흐름 제어, 에러 제어
5. `물리 계층` : 비트 데이터를 전기적 신호로 변환하여 물리적인 전송 매체를 통해 다음 노드로 전송하는 계층
    - `PDU` : 비트(0과 1은 전기적 on/off 상태를 의미)
    - 물리적인 전송 매체 : 광케이블 등
    - `인코딩` : 비트 데이터를 아날로그 신호로 변환하는 것
    - `디코딩` : 아날로그 신호를 비트 데이터로 변환하는 것
    - 데이터 전송시에 잡음, 간섭, 왜곡, 지연 등의 영향을 받는다. (데이터링크 계층의 필요성)

<br/>

![https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FdF3tiC%2Fbtrj9kybwlX%2FC8YHjghkXQszunfEyKyasK%2Fimg.png](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FdF3tiC%2Fbtrj9kybwlX%2FC8YHjghkXQszunfEyKyasK%2Fimg.png)

`PDU(Protocol Data Unit)` : 각 프로토콜 스택 계층의 데이터 단위 (PDU = SDU + PCI)

- `SDU(Service Data Unit)` : 각 프로토콜 스택 간에 전송하는 데이터 단위
- `PCI(Protocol Control Information)` : 프로토콜 제어 정보(헤더와 같은 의미)
- `캡슐화` : 송신측의 각 계층에서 데이터에 PCI(헤더)를 덧붙히는 과정
- `역캡슐화` : 수신측의 각 계층에서 캡슐화된 데이터에 PCI(헤더)를 벗겨내는 과정

<br/>

**`계층을 구조화한 이유` : 복잡한 구조의 통신 시스템을 기능별로 나누어 명확하고 구체적으로 구분하여 각각의 모듈로써 효과적으로 분업시키기 위함이다. 또한 어떤 계층에서 문제가 생긴다면 다른 계층은 건드리지 않고 한 계층의 문제만 해결할 수 있기 때문에 유지 보수 측면에서 강점을 가지고 있다.**

**`상하구조인 이유` : 상하 구조를 통해서 상위 계층이 정삭적으로 동작하기 위해서 그 하위 계층이 모두 정상 동작을 해야하기 때문이다.**

<br/>

## OSI 7계층

인터넷 프로토콜 스택 5계층의 응용 계층을 `응용 계층` ,`표현 계층`, `세션 계층`으로 나눈 모델이다. (응용 계층의 부담을 덜기 위함)

1. 응용 계층
2. `표현 계층` : 네트워크의 데이터 번역을 통해 데이터 형식의 차이를 다루는 계층
    - MIME 인코딩, 암호화가 이루어진다
3. `세션 계층` : 두 호스트 프로세스 간 세션 설정/유지/종료하는데 필요한 기능을 제공하는 계층
    - 두 호스트 프로세스 간의 연결이 손실되면 연결을 복구 시도를 하는 계층이다.
    - `세션` : 네트워크 상에서 종단간의 일회용 논리적 연결을 의미함
4. 전송 계층
5. 네트워크 계층
6. 데이터링크 계층
7. 물리 계층

<br/>

`현재 인터넷은 OSI 7계층이 아닌 TCP/IP 계층 모델을 따른다`

- 현대의 인터넷이 TCP/IP 모델을 따르는 이유는 OSI 모델이 TCP/IP 모델과의 시장 점유 싸움에서 졌기 때문이다.

<br/>

## TCP/IP 계층

![https://raw.githubusercontent.com/CS-studi/CS-study/master/CS/Network/img/Network_Layer/2.png](https://raw.githubusercontent.com/CS-studi/CS-study/master/CS/Network/img/Network_Layer/2.png)

TCP/IP 계층은 2가지 버전이 있다 → `TCP/IP Original(4계층)`, `TCP/IP Updated(5계층)`

- 오늘날에는 `TCP/IP Updated(5계층)` 모델이 더 많이 사용된다
- 현재 인터넷 프로토콜 스택 5계층은 `TCP/IP Updated(5계층)` 모델을 따른다.

<br/>

## 논리적 주소 체계

`IP 주소` : 인터넷에 연결된 모든 컴퓨터들이 논리적으로 갖는 고유 주소

- `Domain Name` :  IP 주소를 전부 입력하기 힘들기 때문에 도메인 네임이 등장
- `DNS` : 도메인 네임을 IP 주소로 매핑시켜주는 시스템

<br/>

`IP` : TCP/IP 기반의 인터넷 망을 통해 IP 데이터그램을 전달하는 프로토콜

- `IP 데이터그램` : IP 헤더 + 데이터
- `IP 네트워크` : IP 프로토콜 기반으로 구축된 네트워크. 현재의 인터넷을 지칭함

<br/>

## IPv4 와 IPv6

기존 IPv4의 주소 고갈문제를 해결하고자 IPv6가 생겼다.

`IPv4 와 IPv6의 헤더`

![http://www.ktword.co.kr/img_data/5185_1.JPG](http://www.ktword.co.kr/img_data/5185_1.JPG)

|  | IPv4(Internet Protocol Version 4) | IPv6(Internet Protocol Version 6) |
| --- | --- | --- |
| 주소 크기 | 32비트 | 128비트 |
| 헤더 종류 | 1종류 | 2종류(기본 헤더 & 확장 헤더) |
| 헤더 크기 | 가변 크기 헤더(옵션에 따라 달라짐) | 고정 크기 헤더(기본 40바이트) |
| 주소 표기법 | . 으로 구분하며 4자리의 10진 표기법(192.168.10.1) | : 으로 구분하며 8자리의 16진 표기법(2001:0DB8:1000:0000:0000:0000:1111:2222) |
| 패킷 크기 제한 | 64kbyte | 없음 |
| 인증 및 보안 기능 | 없음 | 확장 헤더를 통해 적용 가능 |

<br/>

`IPv6가 IPv4에 비해 가지는 이점`

- 패킷을 단편화 하지 않아 효율적인 라우팅이 가능하다
- 확장 헤더를 통해 인증 및 보안 기능을 적용할 수 있어서 데이터 무결성을 제공한다
