# REST
<br>

## REST(Representational State Transfer)

REST(Representational State Transfer)는 효율적, 안정적이며 확장가능한 분산시스템을 가져올 수 있는 소프트웨어 아키텍처 디자인 제약의 모음을 나타냅니다. 그리고 그 제약들을 준수했을 때 그 시스템은 RESTful하다고 일컬어집니다.

* RESTful

    * RESTful은 일반적으로 REST라는 아키텍처를 구현하는 웹 서비스를 나타내기 위해 사용되는 용어이다.
        * 즉, REST 원리를 따르는 시스템은 RESTful이란 용어로 지칭된다.
    * RESTful은 REST를 REST답게 쓰기 위한 방법으로, 누군가가 공식적으로 발표한 것이 아니다.

<br>

- HTTP 프로토콜을 그대로 활용하기 때문에 웹의 장점을 활용할 수 있는 아키텍쳐 스타일이다.

- 자원을 이름으로 구분하여 해당 자원의 상태를 주고, 받는 모든 것

<br>

### __구성요소__ 

__자원, 조작, 표현__

자원(Resource)

- 서버에 있는 것. DB 안에 들어가 있는 데이터 또는 이미지 하나하나를 의미한다. ex) 유저, 주문, 이미지주소 등

- URI를 통해 자원을 명시하고, 구분할 수 있다.

조작(Verb; 행위)

- 클라이언트는 HTTP Method를 이용하여 지정한 자원에 대한 조작을 요청한다.

표현(Representation of Resource)

- 어떤 리소스의 특정 시점의 상태를 반영하고 있는 정보

- 클라이언트가 서버에게 자원에 대한 조작을 요청하면 서버는 이에 대한 적절한 응답을 보낸다.

- REST에서 하나의 자원은 JSON, XML 등 여러 형태의 Representation으로 나타내어 질 수 있다.

<br>

### REST 조건
1. Server-Client(서버-클라이언트 구조)
    - 클라이언트-서버 구조는 사용자 인터페이스에 대한 관심을 데이터 저장에 대한 관심으로부터 분리함으로써 클라이언트의 이식성과 서버의 규모확장성을 개선한다.
    - REST Server: API를 제공하고 비즈니스 로직 처리 및 저장을 책임진다.
    - Client: 사용자 인증이나 context(세션, 로그인 정보) 등을 직접 관리하고 책임진다.
    - 서로 간 의존성이 줄어든다.

2. Stateless(무상태)
    - 클라이언트와 서버의 통신에는 상태가 없어야한다. 
    - 모든 요청은 필요한 모든 정보를 담고 있어야한다.
    - Client의 context를 Server에 저장하지 않는다. -> 세션과 쿠키와 같은 context 정보를 신경쓰지 않아도 되므로 구현이 단순해진다.
    - Server는 각각의 요청을 완전히 별개의 것으로 인식하고 처리한다. -> 각 API 서버는 Client의 요청만을 단순 처리한다. 이전 요청이 다음 요청의 처리에 연관되어서는 안된다.

3. Cacheable(캐시 처리 가능)
    - 캐시가 가능해야한다. 즉 모든 서버 응답은 캐시 가능한지 그렇지 아닌지 알 수 있어야한다.
    - 캐시 사용을 통해 응답시간이 빨라지고 REST Server 트랜잭션이 발생하지 않기 때문에 전체 응답시간, 성능, 서버의 자원 이용률을 향상시킬 수 있다.
    - HTTP 프로토콜 표준에서 사용하는 LAST-Modified 태그나 E-Tag를 이용해 캐싱 구현이 가능하다.

4. Layered System(계층화)
    - 계층(hierarchical layers)으로 구성이 가능해야하며, 각 레이어에 속한 구성요소는 인접하지 않은 레이어의 구성요소를 볼 수 없어야한다.
    - Client는 REST API Server만 호출한다.
    - REST Server는 다중 계층으로 구성될 수 있으며, 보안로드 밸런싱, 암호화 계층을 추가해 구조상의 유연성을 둘 수 있고 PROXY, 게이트웨이 같은 네트워크 기반의 중간매체를 사용할 수 있게 한다.

5. Uniform Interface(인터페이스 일관성)
    - 구성요소(클라이언트, 서버 등) 사이의 인터페이스는 균일(uniform)해야한다.
    - 요청이 어디에서 오는지와 무관하게, 동일한 리소스에 대한 모든 API 요청은 동일하게 보여야 한다.
    - 이것은 요청을 하는 Client가 플랫폼(Android, Ios, Jsp 등) 에 무관하며, 특정 언어나 기술에 종속받지 않는 특징을 의미한다.
    - REST API는 사용자의 이름이나 이메일 주소 등의 동일한 데이터 조각이 오직 하나의 URI(Uniform Resource Identifier)에 속함을 보장해야 한다. 리소스가 너무 클 필요는 없지만, 이는 클라이언트가 필요로 하는 모든 정보를 포함해야 합니다.

    <br>

    - Identification of resources
        - 리소스가 URI로 식별되면 된다.
    - Manipulation of resources through representation
        - 리소스를 요청할 때 서버는 리소스를 표현하여 응답한다.
        - 이 표현은 클라이언트가 이해하고 조작할 수 있는 형식으로 리소스의 현재 상태를 캡처한다.
        -  서버가 자원의 표현을 보내기 때문에 클라이언트가 클라이언트의 요구에 맞는 특정 표현을 요청할 수 있다. (메타데이터 등을 활용) -> 콘텐츠 협상
        - API에서 콘텐츠 협상을 사용하여 여러 클라이언트가 동일한 URL에서 다른 리소스 표현에 액세스할 수 있도록 할 수 있다.
    - Self-Descriptiveness(자체표현 구조)
        - REST API 메시지만 보고도 이를 쉽게 이해할 수 있는 자체 표현 구조로 되어있다.
    - HATEOS(Hypermedia As The Engine Of Application State)
        - RESTful API를 사용하는 클라이언트가 전적으로 서버에 의해 동적으로 상호작용을 할 수 있다. 쉽게 말하면 클라이언트가 서버에 요청시 서버는 요청에 의존되는 URI를 Response에 포함시켜 반환한다. 
        -  동적인 API를 제공할 수 있게됩니다.(모든 관련된 동작을 URI를 통해 알려줍니다.) 즉, 클라이언트가 API의 변화에 일일이 대응하지 않아도 된다는 장점을 가져옵니다.
        - Hypermedia (링크)를 통해서 애플리케이션의 상태 전이가 가능해야 한다. (Hypermedia (링크)에 자기 자신에 대해한 정보가 담겨야 한다.)

        ```
        {
            "timeline_url": "https://github.com/timeline",
            "user_url": "https://github.com/{user}",
            "current_user_public_url": "https://github.com/octocat",
            "current_user_url": "https://github.com/octocat.private?token=abc123",
            "current_user_actor_url": "https://github.com/octocat.private.actor?token=abc123",
            "current_user_organization_url": "",
            "current_user_organization_urls": [
                "https://github.com/organizations/github/octocat.private.atom?token=abc123"
            ],
            "security_advisories_url": "https://github.com/security-advisories",
            "_links": {
                "timeline": {
                "href": "https://github.com/timeline",
                "type": "application/atom+xml"
                },
                "user": {
                "href": "https://github.com/{user}",
                "type": "application/atom+xml"
                },
                "current_user_public": {
                "href": "https://github.com/octocat",
                "type": "application/atom+xml"
                },
                "current_user": {
                "href": "https://github.com/octocat.private?token=abc123",
                "type": "application/atom+xml"
                },
                "current_user_actor": {
                "href": "https://github.com/octocat.private.actor?token=abc123",
                "type": "application/atom+xml"
                },
                "current_user_organization": {
                "href": "",
                "type": ""
                },
                "current_user_organizations": [
                {
                    "href": "https://github.com/organizations/github/octocat.private.atom?token=abc123",
                    "type": "application/atom+xml"
                }
                ],
                "security_advisories": {
                "href": "https://github.com/security-advisories",
                "type": "application/atom+xml"
                }
            }
        }

        ```

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

- point-to-point 통신 모델을 가정하므로 둘 이상으로 상호작용하는 분산환경에는 유용하지 않다.



<br>

## REST API
  * REST API의 정의
    * REST 기반으로 서비스 API를 구현한 것

    <br>

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
  
  <br>













