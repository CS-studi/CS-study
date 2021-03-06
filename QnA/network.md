# ⁉️ Network QnA

## HTTP Method
<details>
<summary>GET과 POST의 차이점에 대해서 설명하시오.</summary>
<div markdown="2">       
GET 메서드는 정보를 조회하기 위한 메서드입니다.  요청하는 데이터가 HTTP Request Message의 Header 부분에 url이 담겨서 전송되는데 이 때 url 상에 쿼리 스트링으로 데이터를 붙여 request를 보냅니다. 이러한 방식은 url 공간에 담겨가기 때문에 전송할 수 있는 데이터의 크기가 제한적입니다.  하지만 현재 주요 웹 브라우저에서 사용할 수 있는 URL 주소의 최대 길이는 익스플로러를 제외하면, 제한을 두지 않고 있습니다.

POST 메서드는 서버의 값이나 상태를 바꾸기 위한 용도의 메서드입니다. POST 방식의 request는 HTTP Request Message의 Body 부분에 데이터가 담겨서 전송이 되기 때문에 데이터를 전송할 때 길이 제한이 없습니다.

또 HTTP 메서드의 속성에는 계속해서 메서드를 호출해도 리소스를 변경하지 않는 안전, 메서드를 여러번 호출해도 결과가 달라지지 않는 멱등, 그리고 캐시 3가지 속성이 있는데 GET 메서드는 안전, 멱등의 특성을 가지고 캐시가 가능한 반면 POST는 안전, 멱등의 특성을 가지지 않고 캐시되지 않습니다.

조회하기 위한 용도로 POST가 아닌 GET 방식을 사용하는 이유에 대해서 설명하시오.

GET은 리소스를 조회한다는 점에서 여러 번 요청하더라도 응답이 똑같을 것입니다. 반대로 POST는 리소스를 새로 생성하거나 업데이트할 때 사용되기 때문에 POST 요청이 발생하면 서버가 변경될 수 있기 때문에  조회에는 GET 방식을 사용합니다.
</div>
</details>

<details>
<summary>PUT 메서드와 PATCH 메서드의 차이점에 대해 설명하시오.</summary>
<div markdown="2">       
PUT과 PATCH는 요청된 자원을 수정할 때 사용한다는 공통점을 가지지만 PUT은 리소스의 모든 것을 업데이트하고 PATCH는 일부만을 업데이트 합니다.
가령 한 사용자에 대해 여러 정보를 객체로 수집하여 서버로 보내는 경우, PUT은 보내지지 않은 정보에 대해서는 null값으로 업데이트하지만, PATCH는 기존 데이터를 유지하는 방식으로 대응합니다.
</div>
</details>

<details>
<summary>HTTP Statue Code(HTTP 상태 코드) 의 종류에 대해 설명하시오.</summary>
<div markdown="2">       
먼저 100번대는 서버가 요청을 받았으며 서버에 연결된 클라이언트는 작업을 계속 진행하라는 의미입니다.
200번대는 서버가 요청을 성공적으로 받았음을 알려주고 300번대는 클라이언트의 요청에 대해 적절한 위치를 제공하거나 대안의 응답을 제공합니다. 그리고 400번대는 클라이언트에서 서버에 잘못된 요청을 보내 서버가 요청을 해결할 수 없을 때 발생하는 코드이며 클라이언트측에서 발생하는 코드입니다. 500번대는 클라이언트의 요청을 받고 서버에서 처리하지 못할때 발생하는 코드이며 서버측에서 발생하는 코드입니다.
</div>
</details>


## Rest and RestAPI
<details>
<summary>REST의 특징에 대해서 설명하시오.</summary>
REST는 HTTP 프로토콜을 활용하기 때문에 웹의 장점을 활용할 수 있다는 특징이 있습니다. 그 예로 Uniform Interface, Stateless, Cacheable, client-server 구조, 계층형 구조 등이 있습니다.

첫번째로 Uniform Interface는 URI로 지정한 리소스에 대한 조작을 통일되고 한정적인 인터페이스로 수행하는 아키텍처 스타일입니다. HTTP 메서드 인터페이스로 한정지어서 해당하는 Resource를 접근하면 URI 길이가 짧아질 뿐 아니라 하나의 URI가 많은 표현을 나타낼 수 있다.

두번째로 Stateless 합니다. 작업을 위한 상태정보를 따로 저장하고 관리하지 않습니다. 세션 정보나 쿠키정보를 별도로 저장하고 관리하지 않기 때문에 API 서버는 들어오는 요청만을 단순히 처리하면 됩니다. 때문에 서비스의 자유도가 높아지고 서버에서 불필요한 정보를 관리하지 않음으로써 구현이 단순해집니다.

세번째로 cacheable은 HTTP가 가진 캐싱 기능이 적용 가능합니다. HTTP 프로토콜 표준에서 사용하는 Last-Modified 태그나 E-Tag를 이용하면 캐싱 구현이 가능합니다.

네번째로 클라이언트-서버 구조로 REST 서버는 API 제공, 클라이언트는 사용자 인증이나 컨텍스트(세션, 로그인 정보)등을 직접 관리하는 구조로 각각의 역할이 확실히 구분되기 때문에 클라이언트와 서버에서 개발해야 할 내용이 명확해지고 서로간 의존성이 줄어들게 됩니다.

마지막으로 REST 서버는 다중 계층으로 구성될 수 있으며 보안, 로드 밸런싱, 암호화 계층을 추가해 구조상의 유연성을 둘 수 있고 PROXY, 게이트웨이 같은 네트워크 기반의 중간매체를 사용할 수 있게 합니다.
</details>

<details>
<summary>REST를 사용하는 이유에 대해서 설명하시오.</summary>
분산 시스템을 위해 필요합니다. 거대한 애플리케이션을 모듈, 기능별로 분리하고 쉽고 REST API를 서비스하기만 하면 어떤 다른 모듈 또는 애플리케이션들이라도 REST API를 통해 상호간에 통신을 할 수 있습니다.
또 WEB브라우저 외의 클라이언트를 위해서도 필요합니다. 웹 페이지를 위한 HTML 및 이미지등을 보내던 것과 달리 이제는 데이터만 보내면 여러 클라이언트에서 해당 데이터를 적절히 보여주기만 하면 되기 때문입니다.
</details>

<details>
<summary>RESTful하게 API를 디자인한다는 것은 무엇인지 설명하시오.</summary>
RESTful하다는 것은 REST의 원리를 따르는 API를 의미합니다. REST의 특징들인 클라이언트-서버 구조, Stateless, Cacheable, Layered system, Uniform Interface를 따라야 하고 REST API 설계시 URI는 정보의 자원을 표현해야하고 자원에 대한 행위는 HTTP Method로 표현합니다.
</details>


## HTTP와 HTTPS의 차이점 & HTTP문제점
<details>
<summary>HTTP와 HTTPS의 차이점을 설명하시오.</summary>
<div markdown="2">       
http는 client의 browser와 서버가 통신을 주고받게 해주는 프로토콜입니다. 서버로 데이터를 요청하거나 전송받을 때 http를 사용합니다. 하지만, http는 보호막을 씌우지 않은 형태이므로 보안에 취약합니다. 그래서 나온 것이 기존에 TCP로 통신을 하던 http위를 SSL이나 TLS로 감싼 형태가 HTTPS입니다.
</div>
</details>


<details>
<summary>HTTP의 문제점을 제시하시오.</summary>
<div markdown="2">       
`Http는 평문 통신`이기 때문에 도청이 가능합니다. 두번 째 문제점은 통신 상대를 확인하지 않기 때문에 `위장`이 가능하다는 점입니다. 마지막으로`완전성`을 증명할 수 없기 때문에 `변조`가 가능하기 때문에 위 세가지 문제점을 해결하기 위해 HTTPS를 이용합니다.
</div>
</details>

<details>
<summary>HTTP의 단점을 해결하기 위해서 어떻게 해야 하나요?</summary>
<div markdown="2">       
첫 번째로 SSL이나 TLS를 사용하여 다른 프로토콜과 조합하여 HTTP 통신 내용을 암호화 합니다. 즉 SSL를 사용해 안전한 통신로를 확립하고 나서 그 통신로를 사용해 HTTP 통신을 하는 것입니다.또 다른 방법으로 HTTP를 사용해서 운반하는 내용만을 암호화 하는 것입니다. 이 방법은 http 자체를 암호화 하는 것이 아닌 컨텐츠만 암호화하는 것인데 다만 이경우는 클라이언트에서 http 메시지를 암호화해서 출력하는 추가 처리가 요구됩니다. 세 번째 방법으로 위 암호화 방법으로 언급된 ssl을 통해서 상대를 확인할 수 있습니다. ssl은 상대를 확인하는 수단으로 증명서를 제공합니다. 마지막으로 md5, sha-sum 등의 해시 값을 확인하는 방법과 파일의 디지털 서명을 확인하는 방법이 존재하지만 확실히 확인할 수 있는 것은 아닙니다. 확실히 방지하기 위해서는 https를 사용해야 합니다.
</div>
</details>

<details>
<summary>HTTPS를 구체적으로 설명하시오.</summary>
<div markdown="2">       
https는 http+ ssl/tls 입니다. 엄밀히 말하면 http의 소켓 부분을 ssl과 tls로 대체하는 것이 https입니다. 기존의 http는 원래 tcp와 직접 통신했지만 https는 http의 부분은 ssl과 통신하고 ssl이 TCP와 통신을 하게 됩니다. https의 SSL에서는 공통키 암호화 방식과 공개키 암호화 방식을 혼합한 하이브리드 암호 시스텝을 상요합니다. 공통키를 공개키 암호화 방식으로 교환한 다음에 다음부터의 통신은 공통키 암호를 사용하는 방식입니다.
</div>
</details>

## 웹 통신의 큰 흐름

<details>
<summary>웹 통신의 큰 흐름에 대해 설명해보세요</summary>
<div markdown="2">       
  
1. 사용자가 브라우저에 url을 입력하면 브라우저 내부 규칙에 따라 이를 파싱하여 도메인 네임을 얻습니다. 
2. 도메인 네임에 해당하는 IP 주소를 찾기 위해 총 4 단계의 DNS 캐시를 살펴봅니다.

- 브라우저 DNS 캐시 → OS의 DNS 캐시 → 라우터의 DNS 캐시 → ISP의 DNS 캐시를 살펴보고 찾고자 하는 IP 주소가 없다면 ISP DNS 서버가 DNS 쿼리를 날립니다.

3. ISP DNS는 여러 다른 DNS 서버들에게 DNS 쿼리를 날리면, 서로 다른 DNS 서버사이를 오고가며 재귀적으로 IP 주소를 검색합니다. 이 과정을 Recursive Search라고 합니다.
4. 도메인에 해당하는 IP 주소를 얻고나면, 클라이언트는 서버와 3 way handshake를 통해 TCP connection을 수립합니다.
    - 클라이언트는 서버와 통신을 요청하는 SYN 패킷을 보내고
    - 서버는 클라이언트의 SYN 패킷을 받은뒤, 이에 대한 응답으로 ACK 패킷과 자신도 클라이언트와 통신을 원한다는 의미의 SYN 패킷을 보냅니다.
    - 클라이언트는 서버의 SYN, ACK 패킷을 받고 서버의 SYN 패킷에 대한 응답으로 ACK 패킷을 보내므로써 TCP connection이 수립됩니다.
5. TCP connection이 수립되었다면 클라이언트는 서버에게 데이터를 전송합니다.

    캡슐화를 통해 7계층에서 1계층으로 데이터에 헤더를 붙이고 데이터를 전기 신호로 변환해 스위치로 전송합니다.

    스위치에서 데이터를 2계층까지 역캡슐화합니다. 그로부터 얻은 Ethernet 헤더의 라우터 MAC 주소를 확인하여 해당 라우터와 연결된 포트로 전송합니다.

    라우터는 데이터를 2계층까지 역캡슐화합니다. 이후 MAC 주소가 자신의 MAC 주소와 같다면 3계층까지 역캡슐화합니다. 그로부터 얻은 IP 헤더의 목적지 IP 주소를 알아내고 최적의 경로로 라우팅을 진행합니다.

    목적지 라우터에서는 데이터의 IP 헤더 속 출발지 IP 주소를 목적지 라우터 내부 IP 주소로 변경합니다. 그리고 목적지 스위치로 전송하기 위해 데이터의 Ethernet 헤더 속 MAC 주소를 변경합니다.

    목적지 스위치에서 전기 신호의 데이터는 웹서버로 전달됩니다.

6. 구글 서버는 데이터를 전달받습니다. 

    이 때, 전달받은 데이터를 1계층에서 7계층 순서로 역캡슐화합니다.

    2계층에서 `Ethernet 프레임의 목적지 MAC주소` 와 자신의 MAC 주소를 비교하여 같다면 3계층으로 올립니다

    3계층에서 `IP 프로토콜 헤더의 목적지 IP 주소` 와 웹 서버의 IP 주소를 비교하여 같다면 4계층으로 올립니다

    4계층에서 `TCP 헤더의 목적지 포트 번호` 를 확인하여 어떤 어플리케이션으로 데이터를 전달할지 정합니다. 만약 데이터에 오류가 있다면 재전송을 요청합니다. 이상이 없다면 5,6,7계층 순서로 전달합니다.

    5계층에서 데이터를 받고 이에 대한 응답 메시지를 만들어 클라이언트로 전달합니다. 전달되는 과정은 위 과정의 역순으로 진행됩니다.

</div>
</details>


<details>
<summary>클라이언트와 서버가 데이터를 주고 받을 때 캡슐화, 역캡슐화의 진행과정을 설명해보세요</summary>
<div markdown="2">       
        
- 클라이언트의 캡슐화 과정
  1. 7계층(응용 계층)에서 `HTTPS의 헤더`가 먼저 붙는다. 6,5계층의 헤더가 붙고
  2. 4계층(전송 계층)에서 `TCP 헤더` 가 붙는다.

      TCP 헤더에는 신뢰할 수 있고 정확한 데이터 송수신을 위해 `출발지 포트번호` 와 `목적지 포트번호` 정보가 기록된다. 여기까지의 데이터를 `세그먼트(segment)` 라고 한다

  3. 3계층(네트워크 계층)에서 `IP 프로토콜의 헤더` 가 붙는다.

      IP 헤더에는 `출발지 IP주소` 와 `목적지 IP주소` 가 기록된다. 여기까지의 데이터를 `패킷(packet)` 이라고 한다

  4. 2계층(데이터 링크 계층)에서 `Ethernet`의 헤더가 붙는다.

      Ethernet 헤더에는 목적지로 가기위해 거쳐야할 IP주소를 가지고 있는 장비인 `라우터의 MAC 주소` 가 기록된다. 여기까지의 데이터를 `프레임(frame)` 이라고 한다.

  5. 1계층(물리 계층)에서 `LAN 카드` 라는 장비를 거쳐 데이터는 전기신호로 변환된다. 그리고 `케이블(UDP)` 와 물리적으로 연결되어있는 `스위치(switch)` 장비로 향한다. (보라색 선)

- 서버의 역캡슐화 과정
  1. 2계층에서 `Ethernet 프레임의 목적지 MAC주소` 와 자신의 MAC 주소를 비교하여 같다면 3계층으로 올립니다
  2. 3계층에서 `IP 프로토콜 헤더의 목적지 IP 주소` 와 웹 서버의 IP 주소를 비교하여 같다면 4계층으로 올립니다
  3. 4계층에서 `TCP 헤더의 목적지 포트 번호` 를 확인하여 어떤 어플리케이션으로 데이터를 전달할지 정합니다. 만약 데이터에 오류가 있다면 재전송을 요청합니다. 이상이 없다면 5,6,7계층 순서로 전달합니다.
  4. 5계층에서 데이터를 받고 이에 대한 응답 메시지를 만들어 클라이언트로 전달합니다. 전달되는 과정은 위 과정의 역순으로 진행됩니다.
  
</div>
</details>


## TCP와 UDP의 차이점 + QUIC
<details>
<summary>TCP와 UDP의 특징과 차이점을 설명해주세요.</summary>
<div markdown="2">
TCP는 연결 지향형 프로토콜로 가상 회선을 만들어 데이터를 전송합니다.  UDP는 비 연결 지향적 프로토콜로 데이터를 데이터그램 단위로 전송합니다. 

TCP는 흐름 제어, 혼잡 제어, 오류 제어를 통해 신뢰성을 보장합니다. 반면에 UDP는 신뢰성을 보장하기 위한 절차가 없어서 TCP 보다 속도가 빠른 편입니다. 

TCP는 파일 전송과 같은 신뢰성이 중요한 서비스에 사용되고, UDP는 스트리밍, RTP(Real-time Transport Protocol=실시간 전송 프로토콜)와 같이 연속성이 더 중요한 서비스에 사용됩니다.

+) UDP 자체는 신뢰성을 보장하지 않지만, 추가적인 정의를 통해 신뢰성을 보장받을 수 있습니다.  QUIC은 UDP를 기반으로 신뢰성을 제공합니다.

</div>
</details>

<details><summary>TCP를 사용하는 대표적인 프로토콜은 무엇인가요?
</summary>
<div markdown="2">

TCP를 사용하는 응용 계층의 프로토콜에는 FTP(File Trasfer Protocol), SMTP(Simple Mail Transfer Protocol), HTTP, TELNET 등이 있습니다.
</div>
</details>


<details><summary>UDP가 어디에 사용되고 왜 사용되나요?</summary>
<div markdown="2">

UDP는 인터넷 전화, 온라인 게임, 멀티미디어 스트리밍 서비스 처럼 실시간으로 데이터를 송/수신해야하는 서비스에 주로 사용됩니다. 

UDP를 사용하는 이유는 신뢰성을 보장하지는 않지만 TCP에 비하여 빠른 전송 속도를 제공하기 때문입니다. 비연결을 지향하기 때문에 데이터 재전송, 흐름 제어 등이 필요하지않고, 전송에 필요한 헤더 사이즈도 작기 때문에 송/수신 과정이 매우 빨라집니다. 이러한 이유들 때문에 신뢰성 보장보다 연속성과 선능이 더욱 중요시되는 서비스에서 UDP가 사용됩니다. 

+) UDP를 프로토콜로 사용하는 서비스에는 DNS(Domain Name System), SNMP(Simple Network Management Protocol), RIP(Routing Information Protocol), 라우터가 있습니다. 

</div>
</details>

<details>
<summary>TCP/UDP의 장단점은 무엇인가요?</summary>
<div markdown="2">

TCP의 장점은 흐름제어, 오류제어, 혼잡제어 등으로 신뢰성있는 데이터 전달을 할 수 있습니다. 단점으로는 전송속도가 느리고 데이터를 보내기 전에 반드시 연결이 형성되어야만 보낼 수 있습니다. 또한 1:1 통신만 가능하다는 점입니다. 

UDP의 장점은 TCP 보다 데이터 전송 속도가 빠르다는 점입니다.

단점은 데이터의 신뢰성이 떨어진다는 것입니다. 패킷의 분실 확인이나 전달 순서를 보장해주지 않습니다. 또한 TCP와 다르게 데이터를 분할하여 전송하지 않아 애플리케이션 단에서 패킷을 분할해야 합니다.
</div>
</details>

<details> 
<summary> 신뢰성있는 TCP를 두고 UDP를 사용하는 이유는 무엇인가요?</summary>
<div markdown="2">

UDP를 사용하는 이유는 신뢰성을 보장하지는 않지만 TCP에 비하여 빠른 전송 속도를 제공하기 때문입니다. 비연결을 지향하기 때문에 데이터 재전송, 흐름 제어 등이 필요하지않고, 전송에 필요한 헤더 사이즈도 작기 때문에 송/수신 과정이 매우 빨라집니다. 이러한 이유들 때문에 신뢰성 보장보다 연속성과 선능이 더욱 중요시되는 서비스에서 UDP가 사용됩니다. 
</div>
</details>

<details> <summary> TCP에서 신뢰성을 보장하는 방법을 설명해주세요.</summary>
<div markdown="2">

송신 측에서 보낸 패킷을 수신 측에서 받지 못하면 재전송합니다. 이때 Timeout 방식은 일정 시간동안 수신자에게 ACK를 받지 못하면 손실됐다고 판단해 재전송을 하는 방식이고, Duplicated ACK 방식은 송신 측에서 동일한 ACK를 3개 이상 받았을 경우 해당 패킷은 손실됐다고 판단해 재전송을 하는 방식입니다. 

또한 흐름 제어, 혼잡 제어, 오류 제어를 통해 신뢰성을 높입니다. 
</div>
</details>
<details> <summary> TCP 통신에서 흐름 제어 기법이 왜 사용되나요?</summary>
<div markdown="2">

흐름제어는 송신측과 수신측의 TCP 버퍼 크기 차이로 인해 생기는 데이터 처리 속도 차이를 해결하기 위해 사용됩니다. 
</div>
</details>

<details> <summary> TCP 흐름 제어 기법은 무엇이 있을까요?</summary>
<div markdown="2">

stop and wait방식과 sliding window 방식이 있습니다.

stop and wait 방식은 매번 전송한 패킷에 대한 확인 응답을 받아야만 그 다음 패킷을 전송할 수 있기 때문에 비효율적입니다.

sliding 방식은 3-way handshaking을 통해 알게된 수신 호스트의 receive window size에 송신측의 send window size를 맞추어 설정한 윈도우에 포함되는 모든 패킷을 전송하고, 그 패킷들의 전달이 확인되는대로  윈도우를 옆으로 옮김으로서 그 다음 패킷들을 전송하는 방식입니다. 

아직 확인을 받지 않고도 여러 패킷을 보내는 것이 가능하기 때문에 stop and wait보다 네트워크를 효율적으로 사용할 수 있습니다. 
</div>
</details>

<details> <summary> TCP 혼잡 제어 기법은 왜 사용되나요?</summary>
<div markdown="2">

데이터의 양이 라우터가 처리할 수 있는 양을 초과하게 되면 초과된 데이터는 라우터가 처리하지 못하기 때문에  송신측에서 네트워크의 상태에 따라서 전송 속도를 조절하여 네트워크의 오버플로우를 방지하기 위해 사용됩니다. 
</div>
</details>

<details>
<summary> TCP 혼잡 제어 기법에는 무엇이 있을까요?</summary>
<div markdown="2">

AIMD, Slow Start, 혼잡 회피, 빠른 재전송, 빠른 회복 기법이 있습니다.

**`AIMD`**는 윈도우 크기를 선형적으로 증가시키는 기법입니다. 혼잡이 감지되면 윈도우 크기를 반으로 줄이게 됩니다. AIMD는 네트워크의 모든 대역을 활용하여 제대로 된 속도로 통신하기까지 시간이 오래 걸린다는 단점이 있습니다.

이를 보안한 방법으로 Slow Start가 있습니다. **`Slow Start`**는 윈도우 크기를 지수적으로 증가시키다가 혼잡이 감지되면 윈도우 크기를 1로 줄이게 됩니다. 윈도우 크기를 지수적으로 증가시키다보면 크기가 기하급수적으로 늘어나기 때문에 임계점을 설정해놓기도 합니다.(혼잡 현상이 발생하였던 윈도우 크기의 절반) 혼잡 윈도우의 크기가 임계치에 도달하게 되면 `혼잡 회피 단계`를 수행하게 됩니다.

+) `혼잡회피`는 임계점에 도달하게 되면 선형적으로 윈도우를 증가시키는 기법입니다.  혼잡이 감지되면 윈도우 크기는 1로 돌아가며, 임계점은 혼잡 윈도우 사이즈의 1/2로 변경합니다.

`빠른 재전송`은 패킷을 받는 쪽에서 먼저 도착해야할 패킷이 도착하지 않고 다음 패킷이 도착한 경우 순서대로 잘 도착한 마지막 패킷의 다음 패킷 순번을 ACK 패킷에 실어서 보낸다. 따라서 중복 ACK를 받게 되는데 3번을 받으면 타임 아웃 시간이 지나지 않았어도 재전송을 하게됩니다.

또한 혼잡한 상황이라고 판단하고, 윈도우 크기는 1로 돌아가면 임계점은 혼잡 윈도우 크기의 1/2로 변경됩니다.

`빠른 회복` 은 혼잡한 상태가 되면 창 크기를 1로 줄이지 않고 반으로 줄이고 선형 증가 시키는 방법입니다. 
</div>
</details>

<details> 
<summary> TCP 혼잡 제어 정책에는 무엇이 있을까요?</summary>
<div markdown="2">

대표적인 정책에는 Tahoe와 Reno가 있습니다.

`Tahoe`는 처음에 Slow Start를 사용하여 윈도우크기를 지수적으로 증가시키다가 임계쩜을 만난 이후부터는 AIMD를 사용하여 선형적으로 증가시킵니다. 그러다가 ACK Duplicated나 Timeout 상황이 발생하면 혼잡 윈도우 크기를 1로 줄이고, 임계점은 혼잡 상황이 발생할때의 혼잡 윈도우 크기의 1/2로 변경합니다. 

`Reno`는 `Tahoe`와 다르게 ACK Dulpicated 상황과 Timeout 상황을 구분합니다. 

**3 ACK가 발생 했을 때**는 윈도우 크기를 1로 줄이는 것이 아니라 반으로 줄이고 임계점도 줄어든 윈도우 값으로 설정하게 됩니다. (빠른 회복)

그러나 **Timeout이 발생했을 때**는 윈도우 크기를 1로 줄이고, slow start를 진행합니다. 이때 임계점은 변경하지 않습니다. 

즉 `Reno`는 ACK 중복보다 Timeout이 더 큰 혼잡 상황이라고 가정한다는 점에서 혼잡 상황의 우선 순위를 둔 정책이라고 할 수 있습니다.
</div>
</details>

## DNS round robin 방식
<details> 
<summary>DNS Round Robin 원리에 대해 설명해주세요</summary>
<div markdown="2">
하나의 웹 서비스를 제공하는 웹 서버가 여러대 있을 때, 클라이언트의 요청을 균등하게 분산시켜 트래픽을 분산하는 기법이다.
클라이언트가 URL에 도메인을 입력하면 DNS 서버는 도메인에 해당하는 IP 주소를 찾습니다. 만약 웹 서비스를 제공하는 웹 서버가 여러대 있다면, 하나의 도메인에 해당되는 IP주소 또한 여러개가 존재합니다.
여러 IP 주소들을 라운드 로빈 방식으로 선택하여 트래픽을 분산하는 방식입니다.
이는 웹 뿐만아니라 FTP, SMTP처럼 도메인을 사용하는 모든 서비스에서 사용이 가능한 기법입니다. 또한 이 기법을 사용하면 트래픽을 분산시키기 때문에 로드 밸런서가 필요 없게 됩니다.
</div>
</details>

<details> 
<summary>DNS Round Robin의 문제점에 대해 설명해주세요</summary>
<div markdown="2">
DNS Round Robin은 클라이언트의 요청을 여러 웹 서버에 분산시키는 기법으로 트래픽을 분산하는데 이점이 있습니다.
하지만 웹 브라우저는 DNS response 를 캐싱하기 때문에, DNS Round Robin의 분산을 거치지 않고, 이전에 접속했던 웹 서버로 접속하게 됩니다. 이는 트래픽을 균등하게 분산시키지 못하게 합니다.
그리고 모바일에서는 웹서버에 접속하기 위해 프록시 서버를 경유합니다. 프록시 서버는 DNS response를 일정동안 캐시합니다. 그렇기 때문에 여러 모바일 클라이언트가 같은 프록시 서버를 경유하게 되면, 항상 같은 서버로 접속됩니다. 이 또한 트래픽을 균등하게 분산시키지 못하게 합니다.
DNS response를 캐시에 저장하는 시간이 TTL값을 짧게 설정하면 DNS 캐시로 인한 불균등 분산 문제를 어느정도 해결할 수 있지만 완벽한 방법이 아닙니다.
</div>
</details>

## 네트워크 시스템의 Layer and Architecture
<details>
<summary>OSI 7 계층에 대해 설명하시오.</summary>
인터넷 프로토콜 스택과 비슷하지만, Presentation layer와 Session layer가 추가되었습니다.
5계층 Session layer(세션 계층)은 데이터가 통신하기 위한 논리적인 연결을 담당합니다. TCP/IP 세션을 만들고 없애는 책임을 지고
통신하는 사용자들을 동기화하고 오류복구 명령들을 일괄적으로 다룹니다.

6계층 Presentation layer(표현 계층)은 데이터를 어떻게 표현할지 정하는 역할을 하는 계층입니다.  MIME 인코딩이나 암호화 등이 이 계층에서 이루어지고, 해당 데이터가 text 인지, gif, jpg 인지 구분하는 역할을 담당한다.
</details>

<details>
<summary>TCP/IP 5 Layer에 대해 설명하시오.</summary>
1계층 - 물리 계층(physical layer)
물리적으로 연결된 두 대의 컴퓨터가 비트의 나열을 주고 받을 수 있게 해주는 계층이고, 물리 계층의 PDU는 비트입니다.

2계층 - 데이터 링크 계층(Data link layer)
같은 네트워크에 있는 여러 대의 컴퓨터들이 데이터를 주고 받기 위해서 필요한 계층이고 이더넷, 와이파이가 속해있습니다. 물리계층을 통해 송수신되는 정보의 오류와 흐름을 관리하여 안전한 정보의 전달을 수행할 수 있도록 도와주는 역할을 합니다. 네트워크 내에서 컴퓨터를 구분하기 위해 각 컴퓨터는 주소 값을 할당받는데, 이 주소를 MAC 주소라고 합니다. 데이터 링크 계층의 PDU는 프레임입니다.

3계층 - 네트워크 계층(Network layer)
라우터를 통해서 서로 다른 네트워크가 통신할 수 있도록 하는 계층입니다. IP주소를 제공하고 가장 일반적인 프로토콜은 IP 입니다. 경로를 선택하고 주소를 정하고 경로에 따라 패킷을 전달해주는 것이 이 계층의 역할입니다. 네트워크 계층의 PDU는 데이터그램입니다.

4계층 - 전송 계층(Transport layer)
EndPoint 간 신뢰성 있는 데이터 전송을 담당하는 계층입니다. 전송 계층의 PDU는 세그먼트입니다.

5계층 - 애플리케이션 계층(Application layer)
응용 프로세스와 직접 관계하여 일반적인 응용 서비스를 수행합니다. 대표적으로 HTTP, FTP 등의 프로토콜이 속하고 PDU는 메시지입니다.

</details>

## 쿠키, 세션, jwt 토큰
<details> 
<summary>사용자 인증 방식의 종류를 말해보세요</summary>
<div markdown="2">
사용자 인증 방식에는 HTTP 기본 인증방식, 서버 기반 인증 방식, 토큰 기반 인증 방식이 있습니다.
</div>
</details>


<details> 
<summary>서버 기반 인증에 대해 설명해보세요</summary>
<div markdown="2">
  
서버 기반 인증방식은 쿠키와 세션을 통해 이루어집니다.
  
1. 서버에서 사용자 상태 정보를 세션에 저장합니다.
2. 그리고 고유한 세션 ID를 쿠키로 클라이언트에게 전달하면 클라이언트는 이를 로컬에 저장합니다.
3. 이후 클라이언트에서 매 요청시 해당 쿠키를 헤더에 담아 요청을 합니다.
4. 서버는 클라이언트로부터 받은 쿠키속 세션 ID의 유효성을 세션 저장소를 통해 확인한 뒤 응답합니다. (세션 저장소는 WAS의 세션, RDB, In-memory DB가 될 수 있습니다)

</div>
</details>

<details> 
<summary>토큰 기반 인증에 대해 설명해보세요</summary>
<div markdown="2">

토큰 기반 인증 과정은 다음과 같습니다.
  
  1. 클라이언트가 로그인 정보를 서버에게 전달하면
  
  2. 서버는 로그인 정보를 검증하고, 정확할 경우 사용자 식별 정보를 기반으로 토큰을 발급합니다. 토큰은 response 헤더에 담아 전달합니다.
  
  3. 클라이언트는 서버로부터 받은 토큰을 로컬에 저장하고, 매 요청시마다 Authorization 헤더에 담아 전달합니다.

  4. 서버는 클라이언트로부터 받은 토큰의 유효성 검사를 한 뒤, 응답을 보냅니다.
토큰 기반 인증 방식은 다음과 같은 이점이 있습니다.

  5. 서버가 아닌 클라이언트에 토큰을 저장하기 때문에 stateless합니다.
stateless한 서버 구조를 가지면 scale out으로 서버 확장시 사용자 정보로 인한 제약이 없습니다.
  
  6. 여러 플랫폼과 도메인에서 사용이 가능한 인증 방식입니다.
토큰으로는 주로 사용자에 대한 정보를 저장하는 Claim 기반의 웹 토큰인 JWT 를 사용합니다.
JWT는 헤더 + 페이로드 + 서명으로 구성되어있습니다.
  
- 헤더에는 JWT 토큰 타입과 사용되는 해시 알고리즘 정보가 들어있습니다
- 페이로드에는 클라이언트에 대한 정보가 들어있습니다. 디코딩할 수 있기 때문에 페이로드에 민감한 정보를 넣으면 안됩니다.
- 서명은 헤더와 페이로드를 더한뒤 서버의 secret key로 해싱하여 생성합니다. 서버의 secret key로만 복호화할 수 있으므로 서명을 통해 해당 JWT 토큰의 유효성을 확인할 수 있습니다.

</div>
</details>

<details> 
<summary>서버 기반 인증의 단점과 JWT를 이용한 토큰 기반 인증의 장단점을 설명해보세요</summary>
<div markdown="2">
  
서버 기반 인증의 단점은 다음과 같습니다.

  1. 사용자 수가 많아질 수록 서버에 저장할 사용자 정보가 많아집니다
만약 세션 정보를 메모리나 데이터베이스에 저장하면 메모리 부하 또는 디스크 부하를 일으킵니다.

  2. 사용자 정보를 서버 측에 저장하기 때문에 stateless하지 않는 구조입니다
이말은 곧 서버 확장이 자유롭지 못하다는 의미입니다. 웹 서버를 증설할 때마다 세션 정보를 증설된 서버에 옮기는 과정이 필요하기 때문입니다.

  3. CORS 방식을 사용한다면 서버 기반 인증 방식이 바람직하지 않습니다
웹 브라우저에서 사용되는 쿠키는 단일 도메인, 서브 도메인에서만 사용할 수 있습니다. 따라서 쿠키를 여러 도메인에서 관리하기 번거롭고 이는 결국 세션을 관리하기 어렵게 만듭니다.

  - JWT를 이용한 토큰 기반 인증 방식에서 Refresh Token의 용도가 무엇인가요?

  JWT을 사용하면 토큰에 유효기간을 설정합니다. 만약 유효기간안에 토큰이 탈취된다면 보안상 위험하기 때문에 유효기간을 짧게 설정하는 방식으로 이를 대처합니다. 하지만 이 방법은 사용자가 로그인을 자주 해야한다는 불편함이 존재합니다.

  따라서 서버는 Refresh Token을 사용하여 이를 해결합니다. 클라이언트에게 유효기간이 짧은 Access Token과 유효기간이 긴 Refresh Token을 함께 발급하고, Access Token이 만료될 경우 클라이언트는 자신의 Refresh Token을 통해 재로그인 없이 새로운 Access Token을 발급받습니다.
Refresh Token을 사용하므로써 Access Token의 탈취 위험성으로부터 벗어날 수 있고, 자주 로그인해야한다는 불편함을 없앨 수 있습니다. 하지만 Refresh Token을 서버 측에 저장해야하기 때문에 stateless하지 않는다는 단점이 있습니다.
  
</div>
</details>

## TCP 3 way-handshake

<details> 
<summary>TCP 3 way handshake란?</summary>
<div markdown="2">
TCP가 통신하기 앞서 논리적인 접속을 성립하기 위해 3 way handshake를 진행합니다
</div>
</details>


<details> 
<summary>3 way handshake 동작방식</summary>
<div markdown="2">
  
먼저 클라이언트가 서버에게 연결 요청을 보내기 위해 SYN 플래그 비트를 1로 설정한 세그먼트를 전송합니다. 또한 sequence number를 랜덤 숫자로 지정하여 함께 보냅니다. 이때 서버는 Listen 상태로 포트 서비스가 가능한 상태여야 합니다.
접속 요청을 받은 서버는 요청을 수락하고 클라이언트 포트도 열어달라는 의미로 SYN와 ACK 플래그 비트를 1로 설정한 세그먼트를 전송합니다. 이때 acknowledgement number를 클라이언트에게 받은 sequence number+ 1로 지정하고 위와 동일하게 sequence number를 지정해서 보냅니다.

마지막으로 클라이언트가 수락 확인을 보내 연결을 맺기위해 ACK 플래그 비트를 1로 설정한 세그먼트를 전송합니다. acknoweldgement number를 서버에게 받은 sequence number + 1로 지정하여 함께 보냅니다. 
포트의 상태를 둘다 established 상태가 되고 연결이 이루어지고 데이터가 오갈 수 있습니다.

</div>
</details>


<details> 
<summary>4 way handshake란?</summary>
<div markdown="2">
연결 성립 후 모든 통신이 끝났다면 4 way handshake를 통해 해제합니다.

먼저 클라이언트가 연결을 종료하겠단 의미로 FIN 플래그 비트를 1로 설정한 세그먼트를 전송합니다
서버가 FIN 플래그로 응답하기 전까지 연결을 유지하며 FIN_WAIT 상태가 됩니다

서버는 FIN 플래그를 받고 ACK플래그로 확인 메세지를 보냅니다. ( acknoweldgement number를 sequence number +1로 자정해서 함께 보냅니다) 
전송할 데이터가 남아있다면 이어서 전송하며 자신의 통신이 끝날 때까지 기다립니다. 서버는 CLOSED_WAIT 상태가 됩니다. 

서버가 통신이 끝났다면 FIN 플래그 비트를 1로 설정한 세그먼트를 보냅니다. 

마지막으로 클라이언트가 해지 준비가 되었다는 메세지를 확인했다는 의미로 ACK 플래그를 보냅니다. 
클라이언트의 ACK 메세지를 받은 서버는 소켓 연결을 close 합니다. 클라이언트는 아직 서버로부터 받지 못한 데이터가 있을 것을 대비해 일정 시간동안 세션을 남겨놓고 잉여 패킷을 기다립니다. 이때 포트 상태는 클라이언트는TIME_WAIT으로 변경됩니다. 
일정시간이 지난후 클라이언트의 소켓 연결도 close하고 둘다 CLOSED 상태가 됩니다.

</div>
</details>

## SSL, 공개키, 대칭키

<details> 
<summary>SSL과 TLS에 대해서 설명하시오.</summary>
<div markdown="2">

SSL과 TLS는 같은 것이라고 할 수 있습니다. SSL은 TCP/IP 암호화 통신에 사용되는 규약으로 SSL 이후 버전을 TLS로 규정하고 있습니다.

HTTP통신에 암호화를 더해 HTTPS 통신을 할 때 SSL은 HTTP를 대신해서 TCP와 통신합니다.
</div>
</details>

<details> 
<summary>공개키 암호화 알고리즘에 대해서 설명하시오.</summary>
<div markdown="2">

공개키 암호화 알고리즘은 비대칭키 암호화 알고리즘으로 쉽게 말해서 암호화와 복호화를 할 때의 키가 대칭적이지 않은 암호화 방식입니다. 암호화를 할 때 공개키를 사용한다면 복호화 시에는 비밀키를 사용합니다. 공개키 암호화 알고리즘은 SSL handshake 과정 중에 키 교환 시에 사용됩니다.

</div>
</details>

<details> 
<summary>SSL 동작 방식은 어떻게 되나요?</summary>
<div markdown="2">

1.웹 서버는 CA를 통해서 인증서를 만든다.

1-1. 웹 서버를 운영하는 쪽에서 HTTPS 적용을 위해 공개키와 개인키를 만든다.

1-2. 신뢰할 수 있는 CA에 인증서 생성을 요청.

1-3. CA는 웹 서버의 공개키, 암호화 방법, 서버의 정보를 담은 인증서를 만들고 CA의 개인키로 암호화하여 서버에게 인증서를 반환

- 클라이언트가 SSL로 암호화된 웹 사이트를 요청 시 서버는 인증서를 웹 브라우저(클라이언트)에게 전송

2.클라이언트 서버 통신 흐름 과정

2-1. 클라이언트가 SSL로 암호화된 페이지 요청

2-2. 서버는 클라이언트에게 인증서 전송

2-3. 클라이언트는 인증서가 신뢰가는 CA로부터 서명된 것인지 판단. 브라우저는 자신의 컴퓨터내에 CA 리스트와 해당 CA를 공개키를 가지고 있기 때문에 공개킬르 활용하여 인증서가 복호화하여 서버의 공개키를 획득한다.

2-4. 클라이언트는 서버(웹사이트)의 공개키를 이용하여 랜덤 대칭 암호화키, 데이터 등을 암호화하여 서버에게 전송하고

2-5. 웹 브라우저(클라이언트)의 대칭키를 얻은 서버는 클라이언트와 대칭키를 이용하여 통신한다.

</div>
</details>

## CORS
<details>
<summary>CORS에 대해서 설명하시오.</summary>
브라우저에서는 보안적인 이유로 cross-origin HTTP 요청들을 제한합니다. 그래서 cross-origin 요청을 하려면 서버의 동의가 필요합니다. 만약 서버가 동의한다면 브라우저에서는 요청을 허락하고, 동의하지 않는다면 브라우저에서 거절합니다.

이러한 허락을 구하고 거절하는 메커니즘을 HTTP-header를 이용해서 가능한데, 이를 CORS(Cross-Origin Resource Sharing)라고 부릅니다.

그래서 브라우저에서 cross-origin 요청을 안전하게 할 수 있도록 하는 메커니즘입니다.
</details>

<details>
<summary>CORS 에러 해결 방법은 뭔가요?
</summary>
1. Access-Control-Allow-Origin 설정
"*"를 설정하게 된다면 모든 외부 출처에서 접속을 할 수 있게 됩니다. 이 방법은 당장은 편리하여 좋아보이지만 보안적인 이슈에 직면할 수 있기 때문에 이러한 방법은 지양해야합니다.
그렇기에 외부에서 사용한다는 요청에 대한 심사를 거친 후 Access-Control-Allow-Origin에 외부 출처를 일일이 적용하는것이 좋습니다.

2. 프록시 서버 설정
외부 도메인서버에 접근하고자할 때 바로 외부 도메인 서버를 통하는 것이 아닌 자신의 서버를 매개로 하여 외부서버에 요청하는 방식입니다.
서버에서 요청을 하게될 때에는 브라우저의 규약인 CORS 정책에 영향을 받지 않습니다.  그러한 이점을 이용하여 Proxy Server라는 출처를 통하기 때문에 CORS 정책을 위반하지 않게되어 우회 할 수 있게됩니다.
</details>

<details>
<summary>CORS preflight는 무엇인가요?
</summary>
실제 요청을 보내는 것이 안전한지 확인하기 위해 먼저 OPTIONS 메서드를 사용하여 cross-origin HTTP 요청을 보냅니다. 이렇게 하는 이유는 사용자 데이터에 영향을 미칠 수 있는 요청이므로 사전에 확인 후 본 요청을 보냅니다.

Origin헤더에 현재 요청하는 origin과, Access-Control-Request-Method헤더에 요청하는 HTTP method와 Access-Control-Request-Headers 요청 시 사용할 헤더를 OPTIONS 메서드로 서버로 요청합니다. 이때 내용물은 없이 헤더만 전송합니다.
브라우저가 서버에서 응답한 헤더를 보고 유효한 요청인지 확인합니다. 만약 유효하지 않은 요청이라면 요청은 중단되고 에러가 발생합니다. 만약 유효한 요청이라면 원래 요청으로 보내려던 요청을 다시 요청하여 리소스를 응답받습니다.
</details>

## 소켓 통신
<details> 
<summary>http통신과 socket 통신의 차이점을 설명하시오.</summary>
<div markdown="2">

http 통신은 단방향 통신으로 클라이언트는 서버에 요청, 서버는 클라이언트에 응답하는 방식으로 동작한다. json, xml, image, html 등등 파일을 전송한다. 응답을 받으면 connection이 끊어지지만 keep alive 옵션으로 일정 시간동안 연결을 유지할 수 있다.

반면에 socket 통신은 두 프로그램이 서로 데이터를 주고 받을 수 있도록 양쪽에 생성되는 통신 단자입니다. 소켓 통신은 서버와 클라이언트 양방향 연결이 이루어진다.

</div>
</details>


<details> 
<summary>RestAPI와 Websocket을 사용하는 상황을 예시를 통해서 설명하시오.</summary>
<div markdown="2">

채팅을 한다고 가정했을때 우리는 RestAPI로 통신을 한다면 송신자측은 post method로 데이터를 보내주게되고 수신자 측은 지속적으로 get method를 보내서 자신에게 새로운 메시지가 오는지 확인하는 방법 밖에 없었다. 이렇게 지속적으로 request를 보내면 서버에 부하가 걸리고 동시간대에 요청을 하는 사용자가 많을 때 특히 문제가 발생한다. 그것을 해결하기 위해서 short polling과 long polling 방식이 있다. short polling은 타이머를 두고 일정 시간마다 server로 요청하는 방식이다. long polling은 매 x초마다 검사를 하는 것이 아니고 delay를 두어 새로 수신된 메시지가 생길 때마다 request를 하는 방식이다. 결론적으로 http로 통신을 하게되면 수신측에서 request를 보내서 확인을 해야한다.

반면에 socket 통신은 양쪽이 연결이 되어있기 때문에 메시지가 수신되면 server가 바로 클라이언트로 응답을 할수 있고, end to end로 연결된 통신을 하게되어 서버의 부하를 줄일 수 있다.

</div>
</details>

## 프록시
<details>
<summary>proxy server란 무엇이고 왜 사용하나요?</summary>
프록시 서버란 클라이언트와 서버 간의 중계 서버로 통신을 대리 수행해주는 서버입니다. 
프록시 서버는 주로 보안 목적이나 캐싱을 위해 사용합니다.
</details>

<details>
<summary>proxy server의 종류에는 무엇이있나요?</summary>
포워드 프록시와 리버스 프록시가 있습니다. 

일반적으로 프록시라고 하면 포워드 프록시를 의미합니다. 포워드 프록시는 클라이언트에서 서버로 리소스를 요청할 때 직접 요청하지않고 포워드 프록시를 거쳐서 요청하게 됩니다. 

리버스 프록시는 애플이케이션 서버 앞에 위치하여 클라이언트가 서버를 요청할때 리버스 프록시를 호출하고  리버스 프록시가 서버로부터 응답을 받아 다시 클라이언트에게 전송하는 역할을 합니다.
</details>

<details>
<summary>포워드 프록시/리버스 프록시의 특징에 대해 설명해주세요.</summary>
포워드 프록시
<br>
1. 캐싱 
<br>
동일한 요청의 경우 프록시 서버에 캐싱된 내용을 전달해주므로써 성능을 향상시킵니다. 전송 시간을 절약할 수 있고 불필요한 서버와의 연결을 하지않아도되어 네트워크 방목 현상을 방지할 수 있습니다.
<br>
2. 익명성
<br>
포워드 프록시는 클라이언트에서 프록시 서버를 거쳐 웹 서비스를 이용할 경우 서버 측에서는 요청을 받을 때 클라이언트 IP가 아닌 프록시 서버의 IP를 전달받게 됩니다. 즉 서버 측에 클라이언트 정보를 숨길 수 있게 됩니다.
<br>
3. 보안(제한)
<br>
보안이 중요한 사내 망에서 정해진 사이트에만 연결할 수 있도록 설정할 수 있습니다.
<br><br>
리버스 프록시<br>
1. 캐싱 <br>
동일한 요청의 경우 프록시 서버에 캐싱된 내용을 전달해주므로써 성능을 향상시킵니다. 전송 시간을 절약할 수 있고 불필요한 서버와의 연결을 하지않아도되어 네트워크 방목 현상을 방지할 수 있습니다.<br>
2. 보안<br>
서버 정보를 클라이언트로 부터 숨길 수 있습니다. 
보안 상의 이유로 서버에 직접 접근하는 것을 막기 위해 DMZ같은 네트워크에 리버스 프록시를 구성하여 접근하도록 할 수 있습니다.
<br>
3. 로드밸런싱<br>
리버스 프록시 뒤에 여러 대의 애플이케이션 서버를 둠으로써 사용자 요청을 분산할 수 있습니다. end-point마다 호출 서버를 설정할 수 있어 서버의 트래픽을 분산할 수도 있습니다.
</details>

## OAuth
<details> 
<summary>OAuth란 무엇인가요?</summary>
<div markdown="2">

OAuth란 인터넷 사용자들이 비밀번호를 제공하지 않고 다른 웹사이트 상의 자신들의 정보에 대해 웹사이트나 애플리케이션의 접근 권한을 부여할 수 있는 공통적인 수단으로서 사용되는 접근 위임을 위한 개방형 표준이다.

쉽게 말해서 다른 서비스의 회원 정보를 안전하게 사용하기 위한 방법이다. OAuth는 Authentication, Authorization(인증, 인가) 둘 다 포함하고 있다.

</div>
</details>

<details> 
<summary>OAuth 1.0과 OAuth 2.0의 차이점을 설명하시오.</summary>
<div markdown="2">

우선 각각의 역할과 토큰, openAPI, 유효 기간이 다릅니다. 1.0 버전의 경우 User, consumer, service provider가 존재하고 토큰으로는 request, access token을 가지고 있습니다. OpenAPI 호출 및 보안으로 전자서명을 사용하고 Access token의 유효기간은 없습니다. 하지만 OAuth2.0의 경우 Resource owner, client, Resource server, Authorization server가 있고, Access token과 refresh token을 가집니다. 보안의 경우 https를 기본으로 사용하고 access token의 유효기간은 refresh token이 존재하기 때문에 짧습니다.

</div>
</details>


<details> 
<summary>OAuth 1.0의 진행 절차를 설명하시오.</summary>
<div markdown="2">

첫 번째로 consumer가 service provider에게 request token을 request 합니다. 그 다음 service provider는 consumer에게 request Token을 수여하면 consumer는 user를 redirect시켜 service provider에서 본인 인증을 하게됩니다. 이후 user가 인증을 하면 consumer는 service provider에게 access token을 요청합니다. service provider는 access token을 consumer에게 수여하게되고 consumer는 이제 access token으로 user의 제3자 앱의 권한을 가지고 user를 인증하게 됩니다.

</div>
</details>


<details> 
<summary>OAuth 2.0의 종류에 대해서 설명하시오.</summary>
<div markdown="2">

OAuth 2.0의 종류로 *Authorization code grant, Implicit grant, Resource Ownser password credentials grant, client credentials grant* 방식이 있습니다.

Authorization code grant 방식은 client가 사용자 대신 특정 리소스에 접근을 요청할 때 사용하고 resource 접근을 위해 authorization server에서 받은 권한 코드로 리소스에 대한 액세스 토큰을 받는 방식입니다. 다른 절차에 비해 보안성이 높고 자주 사용됩니다.

Implicit grant는 autorization code grant와 다르게 권한 코드 교환 단계가 있고, access token을 즉시 반환 받아 이를 인증에 이용하는 방식입니다.

Resource owner password credentials는 client가 암호를 사용하여 액세스 토큰에 대한 사용자의 자격 증명을 교환하는 방식이고 resource owner에서 id, pwd를 전달받아 resource server에 인증하는 방식으로 신뢰할 수 있는 client에서만 가능합니다.

마지막으로 client credentials grant는 client가 컨텍스트 외부에서 access token을 얻어 특정 리소스에 접근을 요청할 때 사용하는 방식입니다.

컨텍스트 외부 ⇒ ./src/user/ , ./img/

</div>
</details>


<details> 
<summary>OAuth 2.0 인증 방식 중 1 가지의 절차를 설명하시오.</summary>
<div markdown="2">

Authorization Code grant 방식은 client에서 authorization server로 권한 부여 요청을 보내고 로그인 팝업창이 전달되면 user는 로그인을 합니다. 로그인 정보가 맞다면 권한 부여 승인 코드를 client에게 전달하고 client는 authorization code를 통해 access token 발급을 요청합니다. authorization server는 자기가 가지고 있는 client id, client secret, authorization code를 전달 받은 정보와 비교하여 동일할 때 access token을 전달합니다. client는 resource server에게 인증을 위한 access token을 전달하면서 필요한 자원을 요청하고 resource server는 access token이 유효하면 해당 자원을 client에게 제공합니다.
<br>
Implicit code grant는 Client가 인증서버에게 사용자 로그인 및 권한 동의 웹페이지를 요청합니다. 로그인 팝업창이 전달되면 사용자는 로그인을하고 로그인 정보가 맞다면 redirect url로 authorization code가 아니라 access token을 전달합니다. 획득한 access token으로 resource server에 api 요청을 보냅니다.
<br>
Resource owner password credentials grant는 resource owner의 인증정보를 client에게 직접 전달합니다. client는 앞서 받은 인증 정보를 authorization server로 전송하여 access token을 발급받습니다. 이후 획득한 access token으로 resource server에 api 요청을 보냅니다.

</div>
</details>
