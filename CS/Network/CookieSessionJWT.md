# 쿠키, 세션, 토큰

> 인증의 필요성  
인증 방식  
서버 기반 인증 (쿠키 & 세션)  
토큰 기반 인증 (JWT 토큰)  
> 

# *인증의 필요성*

> HTTP 프로토콜은 `비연결성`, `무상태성` 프로토콜이다.
> 
1. `HTTP 프로토콜은 비연결성(Connectionless) 프로토콜이다.`
    
    클라이언트가 request를 보내고, 서버는 이에 대한 response를 보내면 연결이 끊어진다.
    
2. `HTTP 프로토콜은 무상태성(Stateless) 프로토콜이다`
    
    위처럼 response, request를 주고 받은 뒤, 상태 정보를 유지하지 않은 채 통신이 끝난다.
    

  

<br/>    

> 인증의 필요성 → 쿠키, 세션, 토큰의 필요성
> 

HTTP는 서버가 다수의 클라이언트와 연결을 계속 유지하지 않기 때문에 불필요한 자원 낭비를 줄일수 있지만 동시에 서버가 클라이언트를 식별할 수 없다. 

만약 클라이언트가 로그인을 하더라도 다음 요청시 해당 클라이언트의 인증 정보를 기억하지 못한다면 다시 로그인해야하는 문제가 발생한다.

- 클라이언트가 매번 요청할 때마다 서버는 새로운 사용자로 인식하기 때문에 인증 절차가 중복되는 문제가 있다.

**HTTP의 무상태성으로 인해 인증절차가 중복되는 문제가 발생한다. 이 문제는 `쿠키, 세션, 토큰` 으로 사용자 인증 정보를 유지하여 해결할 수 있다.**

<br/>

# *인증 방식*

1. `HTTP 기본 인증`
    
    가장 잘 알려진 HTTP 인증 규약으로, 사용자의 이름과 비밀번호를 Base64 인코딩하여 Header에 담아 전송하는 인증 방식이다.
    이 방식은 request을 보낼 때마다 사용자의 계정 정보를 Header에 담아 전송하므로 보안에 취약하다  
      
    <details>
    <summary>Base64 인코딩이란</summary>
    Base64란 Binary Data를 Text로 바꾸는 Encoding(binary-to-text encoding schemes)의 하나로써 Binary Data를 Character set에 영향을 받지 않는 공통 ASCII 영역의 문자로만 이루어진 문자열로 바꾸는 Encoding이다.
    </details>
2. `서버 기반 인증`
    
    서버의 세션과 클라이언트의 쿠키를 기반으로 하는 인증 방식이다.
    
3. `토큰 기반 인증`
    
    클라이언트의 세션 상태를 저장하지 않고, 필요한 정보가 담겨있는 토큰을 활용하는 인증 방식이다.
    
    - 토큰은 클라이언트가 가지고 있다가 서버로 request를 보낼 때, 인증 헤더에 담아 보낸다. 서버는 토큰을 검증하고 요청을 처리한다. 이와 같은 방식으로 인증 처리가 이뤄진다.

<br/>

# *서버 기반 인증(쿠키 & 세션)*

## ## *쿠키란*

쿠키란 클라이언트가 어떠한 웹사이트를 방문할 경우, 그 사이트가 사용하고 있는 서버를 통해 클라이언트의 브라우저에 설치되는 `key-value` 형태의 문자열이다.

- HTTP 규격상 상태 정보를 유지할 수 없는 상황에서 등장한 기술이 바로 `쿠키`이다
- 쿠키는 HTTP1.1 규격에 포함되지 않는다. 즉, HTTP 기술이 아닌데도 불구하고 HTTP의 단점을 극복하기 위해 사용한다.

![99F0CA485B96889D17.png](img/CookieSessionJWT/99F0CA485B96889D17.png)

<br/>    

> 쿠키의 속성과 종류

<details>
<summary>쿠키의 속성</summary>

![Untitled](img/CookieSessionJWT/Untitled.png)

| Name | 쿠키의 이름 |
| --- | --- |
| Value | 쿠키 값  |
| Domain | 쿠키에 접근 가능한 도메인을 설정하는 속성이다.<br/> - `domain=google.com` 으로 도메인을 설정하면 *.google,com 과 같은 서브 도메인에서 해당 쿠키를 사용할 수 있다.<br/> - 도메인을 설정하지 않는다면 서브도메인에서 해당 쿠키에 접근할 수 없다 |
| Path | 쿠키에 접근할 수 있는 경로를 설정하는 속성이다.<br/> - `path=/cat` 으로 path를 설정하면 `/cat` , `/cat/1` , `/cat/1/age` 와 같은 경로에서 해당 쿠키에 접근할 수 있다 |
| Expires/Max-Age | Expires/Max-Age는 쿠키를 얼마나 유지할 것인지 설정하는 속성이다.<br/> - Expires/Max-Age가 설정되지 않은 쿠키는 브라우저가 닫힐때 삭제된다. (이와 같은 쿠키를 세션 쿠키 라 한다) |
| HttpOnly | 클라이언트에서 Javascript의 document,cookie 를 활용하여 쿠키를 조작할 수 없도록 선언하는 설정이다.<br/> - XSS (Cross-site Scripting) 공격을 막기 위해 사용한다. (XXS : 웹 사이트 관리자가 아닌 다른 이가 웹 페이지에 악성 스크립트를 삽입할 수 있는 취약점이다)<br/> - XSS는 서버를 이용하여 클라이언트에게 공격을 가하는 방법이다. |
| Secure | Secure 속성을 설정하면 해당 쿠키는 HTTPS 통신에서만 접근할 수 있다 |
| SameSite | 사이트 외부에서의 해당 쿠키에 접근하지 못하도록 선언하는 설정한다.<br/> - SameSite를 선언하면 쿠키가 Cross-Site 요청시 전송되지 않는다. <br/> - CSRF (크로스 사이트 요청 위조) 공격을 막기 위해 사용한다. (CSRF : 사용자가 자신의 의지와 무관하게 공격자의 의도대로 웹 사이트에 요청을 하게하는 공격이다)<br/> - CSRF는 클라이언트를 이용하여 서버에 공격을 가하는 방식이다. |
  
</details>

<details>
<summary>쿠키의 종류</summary>

| 종류 | 설명 |
| --- | --- |
| Session Cookie | 메모리에만 저장되는 쿠키이다. <br/> - 브라우저 종료시 삭제된다.<br/> - 쿠키의 Expires 속성이 없으면 Session Cookie로 간주한다. |
| Persistent Cookie | 장기간 유지되는 쿠키이다. <br/> - 파일로 저장되기 때문에 브라우저 종료시에도 유지된다. (ex) Max-Age : 1년) <br/> - 쿠키의 Expires 속성이 있으면 Persistent Cookie로 간주한다. |
| Secure Cookie | HTTPS에서만 사용되는 쿠키이다. <br/> - 쿠키 정보가 암호화되어 전송된다. |
| Third-Party Cookie | 방문한 도메인과 다른 도메인의 쿠키이다. <br/> - 주로 광고 배너를 관리할 때 유입 경로 추적시 사용된다. |

</details>


<br/>    

> 동작 방식
> 

![Untitled](img/CookieSessionJWT/Untitled%201.png)

1. 웹 브라우저가 서버에게 request 보낸다.
2. 서버는 상태 유지를 하려는 값을 쿠키로 생성하고, 이를 response의 HTTP 헤더(`Set-Cookie`)에 포함하여 전송한다.
    
    ```java
    TTP/1.0 200 OK
    Content-type: text/html
    Set-Cookie: yummy_cookie=choco
    Set-Cookie: tasty_cookie=strawberry
    ```
    
3. 웹 브라우저는 전달받은 쿠키를 받아 저장하고, 다음 request를 전송할 때 HTTP 헤더(`Cookie`)에 담아서 전송한다.
    
    ```java
    GET /sample_page.html HTTP/1.1
    Host: www.example.org
    Cookie: yummy_cookie=choco; tasty_cookie=strawberry
    ```


<br/>    

> 쿠키의 문제점
> 
1. `쿠키에 대한 정보를 매 헤더(Http Header)에 추가하여 보내기 때문에 상당한 트랙픽을 발생시킨다`
2. `보안에 취약하다`
    
    클라이언트의 request 헤더에 쿠키 값을 그대로 담아 전송하기 때문에 데이터가 노출될 수 있고 브라우저 개발자 도구에서 위변조가 가능하다.
    
3. `쿠키의 개수, 크키가 한정되어있다`
    
    쿠키 표준안인 RFC 2109에 따르면 쿠키는 300개까지 만들 수 있으며, 최대 크기는 4,096바이트(`0.004096 MB`)이고, 하나의 호스트나 도메인에서 최대 20개까지 만들 수 있다 (대부분의 브라우저는 표준안보다 더 적은 개수의 쿠키만을 지원한다)
    
4. `웹브라우저를 변경할 경우 다른 웹브라우저에서 저장한 쿠기값을 사용할 수 없다`
    
    웹 브라우저마다 쿠키에 대한 지원 형태가 다르기 때문에 브라우저간 공유가 불가능하다
    

<br/>    

> 쿠키의 대안

<details>
<summary>쿠키의 대안 : Web Storage API</summary>
    
`Web Storage API`는 기존 쿠키의 문제점을 극복하기 위해 **웹 브라우저가 직접 데이터를 저장할 수 있도록 해준다**. (기존에는 서버가 만든 쿠키를 브라우저가 저장하는 방식이였다)

- Web Storage는 최소 5MB 이상의 공간을 가지고 있고, 이 정보는 서버로 전송되지 않는다.
- Origin 마다 Web Storage가 하나씩만 존재한다. 그리고 하나의 Origin에 속하는 모든 웹 페이지는 같은 데이터를 저장하고, 같은 데이터에 접근할 수 있다.
    - Origin : 도메인과 프로토콜의 한쌍으로 이루어진 식별자이다.
        - 도메인(domain): naver.com
        - 오리진(origin): https://www.naver.com:port

Web Storage API는 사용자가 데이터를 저장할 수 있도록 두 가지 객체를 제공한다.

- sessionStorage 객체 : 하나의 세션(session)만을 위한 데이터를 저장하는 객체
- localStorage 객체 : 보관 기한이 없는 데이터를 저장할 수 있는 객체
    
</details>
    
<br/>

## ## *세션이란*

세션은 쿠키의 문제점으로 인한 보안 문제를 해결하기 위해 등장한 기술로, 일정 시간동안 같은 웹 브라우저로 부터 들어오는 일련의 요구를 `하나의 상태`로 보고 그 상태를 일정하게 유지시킨다.

- `하나의 상태 = 사용자에 대한 고유한 식별값 = SessionId`
    - 서버는 SessionId를 메모리 또는 Redis와 같은 인메모리 데이터베이스에 저장한다.
- 시간이 지남에 따라 하드웨어의 비용이 감소하여 중요한 데이터는 서버에 저장하는 식으로 발전하였다.

![99FEB34F5B96889D34.png](img/CookieSessionJWT/99FEB34F5B96889D34.png)

<br/>

## ## 쿠키와 세션

> 활용 방식
> 
1. 서버에 저장된 세션을 구분하기 위한 SessionId값을 저장하는 용도로 쿠키를 사용된다.
    
    ![99376D4C5B96889E11.png](img/CookieSessionJWT/99376D4C5B96889E11.png)
    
2. 유투브, 구글에서 로그인하지 않아도 사용자가 접근한 동영상이나 검색 기록을 쿠키에 저장해두고, 해당 쿠키를 바탕으로 검색 최적화에 사용하기도 한다.
3. 쇼핑몰 사이트에서는 사용자의 쇼핑 동향을 쿠키에 저장하여, 이를 기반으로 제품을 추천한다.

> *쿠키 vs 세션 비교*
> 

|  | Cookie | Session |
| --- | --- | --- |
| 저장위치 | 클라이언트의 웹 브라우저가 지정하는 메모리 또는 하드디스크에 저장 | 서버의 메모리에 저장 |
| 저장형식 | Text | Object |
| 만료시점 | 쿠키 저장시 expires 속성으로 설정(설정 없으면 브라우저 종료 시) | 클라이언트가 로그아웃하거나, 설정 시간동안 반응이 없으면 무효화 되기 때문에 정확한 시점 알 수 없음 |
| 리소스 | 클라이언트에 저장되고 클라이언트의 메모리를 사용하기 때문에 서버 자원 사용하지 않음 | 세션은 서버에 저장되고, 서버 메모리로 로딩 되기 때문에 세션이 생길 때마다 리소스를 차지함 |
| 용량제한 | 클라이언트도 모르게 접속되는 사이트에 의하여 설정될 수 있기 때문에 쿠키로 인해 문제가 발생하는 걸 막고자 한 도메인당 20개, 하나의 쿠키 당 4KB로 제한해 둠 | 클라이언트가 접속하면 서버에 의해 생성되므로 개수나 용량 제한 없음 |

<br/>

## ## 서버 기반 인증

> 서버 기반 인증 과정
> 

![image.png](img/CookieSessionJWT/image.png)

1. 클라이언트는 로그인 정보를 서버에게 전달한다
2. 서버는 `DB`를 통해 로그인 정보를 검증하고, 정확하다면 사용자를 식별할 고유 SessionID를 생성한다. 이후 HTTP response 헤더의 `Set-Cookie`필드에 SessionID를 담아 클라이언트에게 전달한다
3. 클라이언트는 전달받은 SessionID를 쿠키에 저장하고, 요청시 HTTP request 헤더의 `Cookie` 필드에 쿠키를 담아 서버에 전달한다.
4. 서버는 쿠키를 받아 `세션 저장소`를 통해 SessionID의 유효성을 확인한 이후, 요청을 처리한다. (세션저장소는 WAS의 세션, RDB, In-memory DB가 될 수 있다)

<br/>    

> 서버 기반 인증 시스템의 문제점
> 
1. `서버 부하 문제`
    
    만약 사용자 수가 많아지면 메모리 또는 데이터베이스에 저장할 세션 정보가 많아진디. 이는 메모리나 디스크 부하(disk I/O)를 일으킨다.
    
2. `확장성 문제`
    
    더 많은 트래픽을 처리하기 위해 웹 서버를 증설할 경우, 새로운 웹 서버에 기존의 세션 정보를 옮겨야 하기 때문에 Scaling Out이 어려워진다.
    
3. `CORS(Cross-origin resource sharing) 방식`
    
    웹 브라우저에서 세션 관리 용도로 사용되는 쿠키는 단일 도메인/서브 도메인에서만 사용 가능하다. 이러한 이유로 CORS 방식(여러 도메인에 request를 보내는 브라우저)을 사용한다면 쿠키, 세션 관리가 어렵다.
    
    <details>
    <summary>cross-origin이란</summary>
    <div markdown="2">   

    cross-origin이란 다음 중 한 가지라도 다른 경우를 말한다.
    1. 프로토콜 - http와 https는 프로토콜이 다르다.
    2. 도메인 - domain.com과 other-domain.com은 다르다.
    3. 포트 번호 - 8080포트와 3000포트는 다르다.
    </div>
    </details>

<br/>

# *토큰 기반 인증 (JWT 토큰)*

토큰 기반 인증 시스템은 인증 받은 사용자들에게 토큰을 발급하고, 서버에 요청할 때마다 헤더에 토큰을 담아 보내도록하여 유효성 검사를 한다.

- 사용자의 인증 정보를 서버에 저장하지 않고, 클라이언트 측에서 들어오는 요청만으로 인증처리한다.

**서버 기반 인증 시스템과 달리 상태 정보를 유지하지 않으므로 `stateless`한 구조를 가진다.**


<br/>    

> 토큰 기반 인증 과정
> 

![image.png](img/CookieSessionJWT/image%201.png)

1. 클라이언트가 로그인 정보를 서버에게 전달한다.
2. 서버는 로그인 정보를 검증하고, 정확하다면 사용자 식별 정보를 기반으로 Token을 발급한다. 그리고 이를 클라이언트에게 전달한다.
3. 클라이언트는 전달받은 Token을 쿠키나 localStorage, sessionStorage 등에 저장하고, 이후 요청시에 Token을 담아 전달한다.
4. 서버는 클라이언트로부터 받은 Token을 검증하고 요청을 처리한다.

<br/>    

> 토큰 기반 인증 시스템의 이점
> 
1. `Stateless(무상태성) & 확장성`
    
    토큰은 서버가 아닌 클라이언트 측에서 저장되기 때문에 서버는 Stateless하며, 서버 확장시 세션 정보로 인한 Scaling Out에 제약이 없다.
    
2. `여러 플랫폼 및 도메인에서 사용가능` → CORS 문제 해결
    
    디바이스의 종류 및 도메인과 상관없이 토큰 기반의 인증을 할 수 있다.
    
    - 주로 JSON 포맷의 JWT 토큰을 사용한다.

<br/>    

> JWT(JSON Web Token)
> 

JSON 포맷을 이용하여 사용자에 대한 정보를 저장하는 Claim 기반의 Web Token으로 모바일이나 웹의 사용자 인증을 위해 사용하는 암호화 토큰이다.

- JWT는 URL의 파라미터로 사용될 수 있도록 Base64url 인코딩을 사용하며 `헤더` + `내용`+ `서명`으로 구성되어있다.
- `key=value` 형식으로 이루어진 한 쌍의 정보를 Claim이라고 칭한다.

<details>
<summary>JWT 이전의 토큰 기반 인증 방식</summary>

| 인증타입 | 설명 |
| --- | --- |
| Basic | 사용자 아이디와 암호를 Base64로 인코딩한 값을 토근으로 사용한다.(RFC 7617) |
| Bearer | JWT 혹은 OAuth에 대한 토큰을 사용한다. (RFC 6750) |
| Digest | 서버에서 난수 데이터 문자열을 클라이언트에 보낸다. 클라이언트는 사용자 정보와 nonce를 포함하는 해시값을 사용하여 응답한다 (RFC 7616) |
| HOBA | 전자 서명 기반 인증 (RFC 7486) |
| Mutual | 암호를 이용한 클라이언트-서버 상호 인증 (draft-ietf-httpauth-mutual) |
| AWS4-HMAC-SHA256 | AWS 전자 서명 기반 인증 |

</details>

<br/>    

> JWT 구성
> 

![image.png](img/CookieSessionJWT/image%202.png)

```java
curl http://127.0.0.1:8000/toktokhan/ -H 'Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiIxMjM0NTY3ODkwIiwibmFtZSI6IkpvaG4gRG9lIiwiaWF0IjoxNTE2MjM5MDIyfQ.SflKxwRJSMeKKF2QT4fwpMeJf36POk6yJV_adQssw5c

header = eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9
payload = eyJzdWIiOiIxMjM0NTY3ODkwIiwibmFtZSI6IkpvaG4gRG9lIiwiaWF0IjoxNTE2MjM5MDIyfQ
signature = SflKxwRJSMeKKF2QT4fwpMeJf36POk6yJV_adQssw5c
```

1. `헤더` header
    
    JWT 토큰의 타입과 사용되는 해시 알고리즘(HMAC SHA256 또는 RSA) 정보가 담겨있다.
    
    `eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9` 를 디코딩하면
    
    ```java
    {
      "typ": "JWT",
      "alg": "HS256"
    }
    ```
    
2. `내용` payload
    
    클라이언트에 대한 정보가 들어있다.
    
    - 약속된 Claim으로는 iss(발급자), sub(토큰명), aud(대상자), exp(만료일자), nbf(활성일자), iat(발급일자), jti(고유식별자) 등이 선택적으로 포함 될 수 있다. (모든 일자 정보는 NumericDate 형태이다)
    - JWT를 디코딩할 수 있기 때문에 보안상 민감한 정보를 payload에 담아서는 안된다
    
    `eyJzdWIiOiIxMjM0NTY3ODkwIiwibmFtZSI6IkpvaG4gRG9lIiwiaWF0IjoxNTE2MjM5MDIyfQ` 를 디코딩하면
    
    ```java
    {
      "sub": "1234567890", // 등록된 플레임
      "name": "John Doe", // 비공개 플레임
      "iat": 1516239022  // 등록된 플레임
    }
    ```
    
3. `서명` signature
    
    서명은 header와 payload를 더한 뒤 secret key로 해싱하여 생성한다.
    
    - 서버는 클라이언트가 전달한 토큰의 서명으로 토큰 위변조를 확인한다. 만약 헤더 또는 페이로드 정보가 클라이언트에 의해 변경된 경우 서명이 무효화된다.
    - 서명은 서버측에서 관리하는 secret key로만 복호화할 수 있다.
    
    `SflKxwRJSMeKKF2QT4fwpMeJf36POk6yJV_adQssw5c` 를 디코딩하면
    
    ```java
    HMACSHA256(
      base64UrlEncode(header) + "." + base64UrlEncode(payload),
      your-256-bit-secret
    )
    ```


<br/>    

> JWT의 장단점
> 

| 장점 | 단점 |
| --- | --- |
| 1. Header와 Payload를 가지고 Signature를 생성하므로 데이터 위변조를 막을 수 있다. | 1. 쿠키/세션과 다르게 JWT는 토큰의 길이가 길어, 인증 요청이 많아질수록 네트워크 부하가 심해진다. |
| 2. 인증 정보에 대한 별도의 저장소가 필요없다. | 2. Payload 자체는 암호화 되지 않기 때문에 유저의 중요한 정보는 담을 수 없다. |
| 3. JWT는 토큰에 대한 기본정보와 전달할 정보 및 토큰이 검증됬음을 증명하는 서명 등 필요한 모든 정보를 자체적으로 지니고 있다. | 3. 토큰은 한번 발급 되면 유효기간이 만료될 때 계속 사용되어 탈취 당하게 되면 대처하기 힘들다. |


<br/>    

> JWT의 보안 전략
> 
1. `짧은 만료기한 설정`
    
    토큰의 만료시간을 짧게 설정하여 탈취되더라도 피해를 최소화할 수 있다.
    
    하지만 이 방법은 사용자가 로그인을 자주 해야한다는 불편함이 존재한다.
    
2. `Refresh Token`
    
    ![Untitled](img/CookieSessionJWT/Untitled%202.png)
    
    클라이언트가 로그인 요청을 보내면, 서버는 Access Token과 함께 긴 만료 기간을 가진 Refresh Token을 발급하는 전략을 취한다.
    
    - 이후 Access Token이 만료되었을 때, Refresh Token을 사용하여 Access Token의 재발급을 요청한다.
    - 서버는 DB에 저장된 Refresh Token과 클라이언트가 보낸 Refresh Token을 비교하여 유효한 경우 새로운 Access Token을 발급해준다.
    
    Refresh Token을 활용하면 Access Token의 만료 기한을 짧게 설정하면서, 사용자가 자주 로그인 할 필요가 없다. 또한 서버가 강제로 Refresh Token을 만료시킬 수 있다.
    
    그러나 서버의 Stateless함을 갖게 한다는 점에서 이점이 있는 JWT의 장점을 완벽하게 누릴 수 없다는 단점이 있다.


<br/>    
   

# 참고

[서버 인증. 쿠키(Cookie), 세션(Session), 토큰(JWT)](https://velog.io/@dogy/%EC%84%9C%EB%B2%84-%EC%9D%B8%EC%A6%9D.-%EC%BF%A0%ED%82%A4Cookie-%EC%84%B8%EC%85%98Session-%ED%86%A0%ED%81%B0JWT)

[[HTTP] HTTP 쿠키란(COOKIE)? 쿠키 등장 배경 그리고 쿠키와 세션의 차이점.](https://dololak.tistory.com/535)

[쿠키,세션,JWT](https://github.com/NKLCWDT/cs/blob/main/Network/Cookie%2C%20Session%2C%20JWT.md), [mozilla](https://developer.mozilla.org/ko/docs/Web/HTTP/Cookies), [쿠키(Cookie) 그리고 세션(Session)](https://nesoy.github.io/articles/2017-03/Session-Cookie)

[Web Storage](http://tcpschool.com/html/html5_api_webStorage)

[쿠키 속성](https://velog.io/@y1andyu/%EC%BF%A0%ED%82%A4-%EC%86%8D%EC%84%B1%EB%93%A4), [쿠키의 단점](https://d2.naver.com/helloworld/233847)

[인증 방식 : Cookie & Session vs JWT](https://tecoble.techcourse.co.kr/post/2021-05-22-cookie-session-jwt/)

[세션 저장소](https://parkadd.tistory.com/16)

[JWT](https://tech.toktokhan.dev/2021/04/30/JWT/)

# 면접 예상 질문

> 사용자 인증 방식의 종류를 말해보세요
> 

> 서버 기반 인증에 대해 설명해보세요
> 

> 토큰 기반 인증에 대해 설명해보세요
> 

> 서버 기반 인증의 단점과 JWT를 이용한 토큰 기반 인증의 장단점을 설명해보세요
> 

> JWT를 이용한 토큰 기반 인증 방식에서 Refresh Token의 용도가 무엇인가요?
>
