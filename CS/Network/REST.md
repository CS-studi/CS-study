# REST

> REST

> REST API

<br>

## REST(Representational State Transfer)

자원을 이름(자원의 표현)으로 구분하여 해당 자원의 상태(정보)를 주고 받는 모든 것을 의미한다. -> 자원(resource)의 표현(representation) 에 의한 상태 전달

REST(Representational State Transfer)는 월드 와이드 웹(www)과 같은 분산 하이퍼미디어 시스템을 위한 소프트웨어 개발 아키텍처의 한 형식.

- HTTP 프로토콜을 그대로 활용하기 때문에 웹의 장점을 활용할 수 있는 아키텍쳐 스타일이다.

- REST는 네트워크 상에서 Client와 Server 사이의 통신 방식 중 하나이다.

<br>

### __구성요소__ 

__자원, 조작, 표현__

자원(Resource)

- 서버에 있는 것. DB 안에 들어가 있는 데이터 또는 이미지 하나하나를 의미한다. ex) 유저, 주문, 이미지주소 등

- URI를 통해 자원을 명시하고, 구분할 수 있다.

조작(Verb; 행위)

- 클라이언트는 HTTP Method를 이용하여 지정한 자원에 대한 조작을 요청한다.

표현(Representation of Resource)

- 클라이언트가 서버에게 자원에 대한 조작을 요청하면 서버는 이에 대한 적절한 응답을 보낸다.

- REST에서 하나의 자원은 JSON, XML 등 여러 형태의 Representation으로 나타내어 질 수 있다.

<br>

### REST 특징
1. Server-Client(서버-클라이언트 구조)
    - REST Server: API를 제공하고 비즈니스 로직 처리 및 저장을 책임진다.
    - Client: 사용자 인증이나 context(세션, 로그인 정보) 등을 직접 관리하고 책임진다.
    - 서로 간 의존성이 줄어든다.

2. Stateless(무상태)
    - Client의 context를 Server에 저장하지 않는다. -> 세션과 쿠키와 같은 context 정보를 신경쓰지 않아도 되므로 구현이 단순해진다.
    - Server는 각각의 요청을 완전히 별개의 것으로 인식하고 처리한다. -> 각 API 서버는 Client의 요청만을 단순 처리한다. 이전 요청이 다음 요청의 처리에 연관되어서는 안된다.

3. Cacheable(캐시 처리 가능)
    - 캐시 사용을 통해 응답시간이 빨라지고 REST Server 트랜잭션이 발생하지 않기 때문에 전체 응답시간, 성능, 서버의 자원 이용률을 향상시킬 수 있다.
    - HTTP 프로토콜 표준에서 사용하는 LAST-Modified 태그나 E-Tag를 이용해 캐싱 구현이 가능하다.

4. Layered System(계층화)
    - Client는 REST API Server만 호출한다.
    - REST Server는 다중 계층으로 구성될 수 있으며, 보안로드 밸런싱, 암호화 계층을 추가해 구조상의 유연성을 둘 수 있고 PROXY, 게이트웨이 같은 네트워크 기반의 중간매체를 사용할 수 있게 한다.
    - 클라이언트 입장에서는 바로 끝단의 서버와 연결되어 있는지, 중간의 프록시나 로드 밸런서와 연결되어 작동하는지 알 수 없습니다. 보안을 위해 여러 겹의 서버나 통신이 있을 수 있고, 서버도 응답을 주기 위해 다른 서버와 통신할 수도 있지만 클라이언트 입장에선 철저하게 분리되어 있기 때문에 직접적으로 바로 연결된 쪽과의 연결만 신경 써주면 됩니다.

5. Uniform Interface(인터페이스 일관성)
    - HTTP 표준에만 따른다면, 어떤 플랫폼이든 특정 언어나 기술에 종속되지 않고 모든 플랫폼에 사용할 수 있으며, URI로 지정한 리소스에 대한 조작이 가능하다.
    - URI로 지정한 리소스를 Http Method를 통해서 표현하고 구분한다.
    - Self-Descriptiveness(자체표현 구조)
        - REST API 메시지만 보고도 이를 쉽게 이해할 수 있는 자체 표현 구조로 되어있다.
    - HATEOS(Hypermedia As The Engine Of Application State)
        - RESTful API를 사용하는 클라이언트가 전적으로 서버에 의해 동적으로 상호작용을 할 수 있다. 쉽게 말하면 클라이언트가 서버에 요청시 서버는 요청에 의존되는 URI를 Response에 포함시켜 반환한다. 
        -  동적인 API를 제공할 수 있게됩니다.(모든 관련된 동작을 URI를 통해 알려줍니다.) 즉, 클라이언트가 API의 변화에 일일이 대응하지 않아도 된다는 장점을 가져옵니다.
        - Hypermedia (링크)를 통해서 애플리케이션의 상태 전이가 가능해야 한다. (Hypermedia (링크)에 자기 자신에 대해한 정보가 담겨야 한다.)

6. Code-On-Demand
    - 클라이언트의 요청에 따라 서버에서 클라이언트로 실행 가능한 소프트웨어를 전달합니다.
    - Client에 보내는 데이터를 바로 실행 가능한 코드를 보내서 이것을 Client에서 실행하는 것을 말한다.
    - optional

<br>

__장점__

- HTTP 프로토콜의 인프라를 그대로 사용하므로 REST API 사용을 위한 별도의 인프라를 구출할 필요가 없다.

- HTTP 프로토콜의 표준을 최대한 활용하여 여러 추가적인 장점을 함께 가져갈 수 있게 해준다.

- HTTP 표준 프로토콜에 따르는 모든 플랫폼에서 사용이 가능하다.

- REST API 메시지가 의도하는 바를 명확하게 나타내므로 의도하는 바를 쉽게 파악할 수 있다.

- 서버와 클라이언트의 역할을 명확하게 분리한다.


__단점__

- 표준이 존재하지 않는다. -> 관리가 쉽지 않다. 

- HTTP Method 형태가 제한적이다. -> 단순히 보내는 기능 외에도 다양한 세부 기능들을 구현하는데 제약이 발생할 수 있다.

- 브라우저를 통해 테스트할 일이 많은 서비스라면 쉽게 고칠 수 있는 URL보다 Header 값을 처리해야하므로 전문성이 요구된다.


<br>


__REST가 필요한 이유__

- 애플리케이션 분리 및 통합
    - 거대한 애플리케이션을 모듈, 기능별로 분리하기 쉬워졌다. RESTful API를 서비스하기만 하면 어떤 다른 모듈 또는 애플리케이션들이라도 RESTful API를 통해 상호간에 통신을 할 수 있기 때문이다.
- 다양한 클라이언트의 등장
    - 웹 페이지를 위한 HTML 및 이미지등을 보내던 것과 달리 이제는 데이터만 보내면 여러 클라이언트에서 해당 데이터를 적절히 보여주기만 하면 된다. 예를 들어 모바일 애플리케이션으로 html같은 파일을 보내는 것은 무겁고 브라우저가 모든 앱에 있는 것은 아니기 때문에 알맞지 않았는데 RESTful API를 사용하면서 데이터만 주고 받기 때문에 여러 클라이언트가 자유롭고 부담없이 데이터를 이용할 수 있다. 서버도 요청한 데이터만 깔끔하게 보내주면되기 때문에 가벼워지고 유지보수성도 좋아졌다.

<br>

## REST API
  * REST API의 정의
    * REST 기반으로 서비스 API를 구현한 것
    
  * REST API의 특징
    * 사내 시스템들도 REST 기반으로 시스템을 분산해 확장성과 재사용성을 높여 유지보수 및 운용을 편리하게 할 수 있다.
    * REST는 HTTP 표준을 기반으로 구현하므로, HTTP를 지원하는 프로그램 언어로 클라이언트, 서버를 구현할 수 있다.
    * 즉, REST API를 제작하면 델파이 클라이언트 뿐 아니라, 자바, C#, 웹 등을 이용해 클라이언트를 제작할 수 있다.
  * REST API 설계 **기본 규칙**
    1. URI는 정보의 자원을 표현해야 한다.
        * resource는 동사보다는 명사를 사용한다.
        * resource는 영어 소문자 복수형을 사용하여 표현한다.
        * Ex) `GET /Member/1` -> `GET /members/1`
    2. 자원에 대한 행위는 HTTP Method(GET, PUT, POST, DELETE 등)로 표현한다.
        * URI에 HTTP Method가 들어가면 안된다.
        * Ex) `GET /members/delete/1` -> `DELETE /members/1`
        * URI에 행위에 대한 동사 표현이 들어가면 안된다.
        * Ex) `GET /members/show/1` -> `GET /members/1`
        * Ex) `GET /members/insert/2` -> `POST /members/2`
  * REST API 설계 규칙
    1. 슬래시 구분자(/ )는 계층 관계를 나타내는데 사용한다.
        * Ex) `http://restapi.example.com/houses/apartments`
    2. URI 마지막 문자로 슬래시(/ )를 포함하지 않는다.
        * URI에 포함되는 모든 글자는 리소스의 유일한 식별자로 사용되어야 하며 URI가 다르다는 것은 리소스가 다르다는 것이고, 역으로 리소스가 다르면 URI도 달라져야 한다.
        * REST API는 분명한 URI를 만들어 통신을 해야 하기 때문에 혼동을 주지 않도록 URI 경로의 마지막에는 슬래시(/)를 사용하지 않는다.
        * Ex) `http://restapi.example.com/houses/apartments/ (X)`
    3. 하이픈(- )은 URI 가독성을 높이는데 사용
        * 불가피하게 긴 URI경로를 사용하게 된다면 하이픈을 사용해 가독성을 높인다.
    4. 밑줄(_ )은 URI에 사용하지 않는다.
        * 밑줄은 보기 어렵거나 밑줄 때문에 문자가 가려지기도 하므로 가독성을 위해 밑줄은 사용하지 않는다.
    5. URI 경로에는 소문자가 적합하다.
        * URI 경로에 대문자 사용은 피하도록 한다.
        * RFC 3986(URI 문법 형식)은 URI 스키마와 호스트를 제외하고는 대소문자를 구별하도록 규정하기 때문
    6. 파일확장자는 URI에 포함하지 않는다.
        * REST API에서는 메시지 바디 내용의 포맷을 나타내기 위한 파일 확장자를 URI 안에 포함시키지 않는다.
        * Accept header를 사용한다.
        * Ex) `http://restapi.example.com/members/soccer/345/photo.jpg (X)`
        * Ex) `GET / members/soccer/345/photo HTTP/1.1 Host: restapi.example.com Accept: image/jpg (O)`
    7. 리소스 간에는 연관 관계가 있는 경우
        * /리소스명/리소스 ID/관계가 있는 다른 리소스명
        * Ex) `GET : /users/{userid}/devices (일반적으로 소유 ‘has’의 관계를 표현할 때)`
    8. :id는 하나의 특정 resource를 나타내는 고유값
        * Ex) student를 생성하는 route: POST /students
        * Ex) id=12인 student를 삭제하는 route: DELETE /students/12
* RESTful
  * RESTful의 개념
    * RESTful은 일반적으로 REST라는 아키텍처를 구현하는 웹 서비스를 나타내기 위해 사용되는 용어이다.
      * 즉, REST 원리를 따르는 시스템은 RESTful이란 용어로 지칭된다.
    * RESTful은 REST를 REST답게 쓰기 위한 방법으로, 누군가가 공식적으로 발표한 것이 아니다.
  * RESTful의 목적
    * 이해하기 쉽고 사용하기 쉬운 REST API를 만드는 것
    * RESTful API를 구현하는 근본적인 목적이 퍼포먼스 향상에 있는게 아니라, 일관적인 컨벤션을 통한 API의 이해도 및 호환성을 높이는게 주 동기이니, 퍼포먼스가 중요한 상황에서는 굳이 RESTful API를 구현하실 필요는 없습니다.
  * RESTful 하지 못한 경우
    * Ex1) CRUD 기능을 모두 POST로만 처리하는 API
    * Ex2) route에 resource, id 외의 정보가 들어가는 경우(/students/updateName)


<br>





### 📚 참고
[REST 1](https://gmlwjd9405.github.io/2018/09/21/rest-and-restful.html)

[REST 2](https://shyvana.tistory.com/7)

[REST 3](https://wonit.tistory.com/454)

[REST API](https://github.com/WeareSoft/tech-interview/blob/master/contents/network.md) 

<br>


***

## Summary

***

<br>

# ⁉️ 면접 예상 질문

> 1. REST의 특징에 대해서 설명하시오.

> 2. REST를 사용하는 이유에 대해서 설명하시오.

> 3. RESTful하게 API를 디자인한다는 것은 무엇인지 설명하시오.











