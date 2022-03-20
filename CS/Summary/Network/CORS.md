# CORS

CORS는 '교차 출처 리소스 공유'라는 의미로 HTTP 헤더를 사용하여 한 출처에서 실행중인 웹 어플리케이션이 다른 출처의 선택한 자원에 접근할 수 있도록 권한을 부여하여 브라우저에 알려주는 정책이다.


## Origin(출처)

출처란 URL 구조에서 살펴본 Protocol, Host, Port를 합친 것을 말한다. 브라우저 개발자 도구의 콘솔 창에 location.origin을 실행하면 출처를 확인할 수 있다.

<br>

### 같은 출처 Vs. 다른 출처
현재 웹페이지의 주소가 https://csstudy.github.io/tech/ 일 때 같은 출처인지 다른 출처인지 확인하기

https://csstudy.github.io/about?q=work : 같은 출처

http://csstudy.github.io : 다른 출처

https://csstudy.github.io:81/about/ : 다른 출처

<br>

## 동일 출처 정책(SOP)
SOP(Same Origin Policy)는 동일 출처 정책이라는 의미로 같은 출처에 대한 HTTP 요청만을 허락한다. 웹 어플리케이션에서 중요한 보안 모델로 사용된다.

postman으로 api를 테스트하거나 다른 서버에서 API를 호출할 때는 잘 동작하다가 브라우저에서 API를 호출할때 CORS 정책 위반으로 오류가 발생하는 상황이 있다. 그 이유는 브라우저가 동일 출처 정책을 지켜서 다른 출처의 리소스 접근을 금지하기 때문이다.

동일 출처 정책을 지키면 외부 리소스를 가져오지 못해 불편하지만, 동일 출처 정책은 XSS나 XSRF 등의 보안 취약점을 노린 공격을 방어할 수 있다. 하지만 현실적으로는 외부 리소스를 참고하는 것은 필요하기 때문에 외부 리소스를 가져올 수 있는 방법이 존재해야 한다. 외부 리소스를 사용하기 위한 SOP의 예외 조항이 CORS다.

<br><br>

## CORS의 동작 방식
웹 서버에게 보안 cross domain 데이터 전송을 활성화하는 cross domain 접근 제어권을 부여한다.


### Simple request

Simple Request는 서버에게 바로 요청을 보내는 방법이다. 클라이언트가 서버에게 바로 요청을 하면 서버는 Access-Control-Allow-Origin 헤더를 포함한 응답을 브라우저에 보낸다. 브라우저는 Access-Control-Allow-Origin 헤더를 확인해서 CORS 동작을 수행할지 판단한다.

#### SImple Request 를 보내기위한 조건 (모두 만족하여야한다.)

1. 요청 메서드가 GET, HEAD, POST 중 하나여야한다.
2. Accept, Accept-Language, Content-Language, Content-Type, DPR, Downlink, Save-Data, Viewport-Width, Width를 제외한 헤더를 사용하면 안된다.
3. Content-Type 헤더는 application/x-www-form-urlencoded, multipart/form-data, text/plain 중 하나를 사용해야 합니다. 우리가 많이 사용하고있는 application/json은 포함되지 않는다.

### Preflight request

서버에 예비 요청을 보내서 안전한지 판단한 후 본 요청을 보내는 방법이다. 본 요청에 앞서, OPTIONS 메서드로 요청이 보내지고, 해당 메서드를 통해 실제 요청을 전송할지 판단한다. 서버는 OPTIONS의 요청에 대한 응답으로 Access-Control-Allow-Origin 헤더를 포함한 응답을 브라우저에 보내고, 브라우저는 위 Simple Request와 동일하게 Access-Control-Allow-Origin 헤더를 확인해서 CORS 동작을 수행할지 판단한다.

- POST요청이지만 Content-Type이 application/x-www-form-urlencoded, multipart/form-data, text/plain이 아닌 경우도 여기에 해당한다.

### Credentialed request
인증된 요청을 사용하는 방법이다. 이 시나리오는 CORS의 기본적인 방식이라기 보다는 다른 출처 간 통신에서 좀 더 보안을 강화하고 싶을 때 사용하는 방법이다.

기본적으로 브라우저가 제공하는 비동기 리소스 요청 API인 XMLHttpRequest 객체나 fetch API는 별도의 옵션 없이 브라우저의 쿠키 정보나 인증과 관련된 헤더를 함부로 요청에 담지 않는다. 이때 요청에 인증과 관련된 정보를 담을 수 있게 해주는 옵션이 바로 credentials 옵션이다.

이 옵션에는 총 3가지의 값을 사용할 수 있으며, 각 값들이 가지는 의미는 다음과 같다.

same-origin(기본값): 같은 출처 간 요청에만 인증 정보를 담을 수 있다.
include: 모든 요청에 인증 정보를 담을 수 있다.
omit: 모든 요청에 인증 정보를 담지 않는다.
만약 same-origin이나 include와 같은 옵션을 사용하여 리소스 요청에 인증 정보가 포함된다면, 이제 브라우저는 다른 출처의 리소스를 요청할 때 단순히 Access-Control-Allow-Origin만 확인하는 것이 아니라 좀 더 빡빡한 검사 조건을 추가하게 된다.

## CORS 에러 해결 방안
1. Access-Control-Allow-Origin 세팅하기
CORS 정책 위반으로 인한 문제를 해결하는 가장 대표적인 방법은, 그냥 정석대로 서버에서 Access-Control-Allow-Origin 헤더에 알맞은 값을 세팅해주는 것이다.

이 헤더는 Nginx나 Apache와 같은 서버 엔진의 설정에서 추가할 수도 있지만, 아무래도 복잡한 세팅을 하기는 불편하기 때문에 소스 코드 내에서 응답 미들웨어 등을 사용하여 세팅하는 것을 추천한다. Spring, Express, Django와 같이 이름있는 백엔드 프레임워크의 경우에는 모두 CORS 관련 설정을 위한 세팅이나 미들웨어 라이브러리를 제공하고 있으니 세팅 자체가 어렵지는 않을 것이다.

2. Webpack Dev server로 리버스 프록싱하기
사실 CORS를 가장 많이 마주치는 환경은 바로 로컬에서 프론트엔드 어플리케이션을 개발하는 경우라고 해도 과언이 아니다. 백엔드에는 이미 Access-Control-Allow-Origin 헤더가 세팅되어있겠지만, 이 중요한 헤더에다가 http://localhost:3000 같은 범용적인 출처를 넣어주는 경우는 드물기 때문이다.

프론트엔드 개발자는 대부분 웹팩과 webpack-dev-server를 사용하여 자신의 머신에 개발 환경을 구축하게 되는데, 이 라이브러리가 제공하는 프록시 기능을 사용하면 아주 편하게 CORS 정책을 우회할 수 있다.

