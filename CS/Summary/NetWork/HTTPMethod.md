# HTTP Method

## HTTP 프로토콜

 HTTP란 텍스트 기반의 통신 규약으로 웹 브라우저와 웹 서버 사이의 통신을 위해 고안되었다

- HTTP는 TCP/IP 기반의 통신 프로토콜이다
- 동작방식
    1. TCP Connection 수립
    2. 클라이언트가 서버로 HTTP Request 전송
    3. 서버는 HTTP Request에 대한 HTTP Response 전송

### 특징

- `비연결성(Connectionless)`
    - 클라이언트와 서버가 connection을 맺은 후 request, response를 주고 받으면 connection을 끊는다
    - connection 유지에 필요한 리소스 소모를 줄인다. → 더 많은 connection을 맺을 수 있음
    - Keep-Alive 헤더로 connection을 유지할 수 있음 → 리소스 소모에 유의
- `무상태성(Stateless)`
    - Connectionless로 인해 서버는 클라이언트의 상태정보(state)를 기억하지 못한다
    - 단점 : 매번 새로운 인증이 필요함 → 쿠키 & 세션(서버 기반 인증), JWT(토큰 기반 인증) 으로 해결
    - 장점 : 서버의 Scaling Out이 원활함(세션 정보를 저장하지 않기 때문)
    

## HTTP Request Message

```java
GET https://www.test.com HTTP/1.1 // 시작 줄
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) ...// 헤더
Upgrade-Insecure-Requests: 1
// 본문 - 현재는 없음.
```

- 클라이언트가 서버로 보내는 메시지
- HTTP Request Message는 3부분으로 구성됨
    1. 시작줄 : `HTTP Method 종류` & `URI` & `HTTP 버전`
    2. 헤더 : Request에 대한 정보를 나타냄
    3. 본문 : RequestBody (Request와 함께 전송할 데이터)

### HTTP Method 종류

- `GET` (Read)
    - 자원 조회 시 사용
    - 데이터를 쿼리 파라미터, 쿼리 스트링을 통해 전달 (데이터를 RequestBody로 전달할 수 있으나 지원하지 않는 곳이 많음)
    - GET Request에 대한 Response : 캐시 가능
- `HEAD`
    - GET 메소드와 비슷하게 동작
    - 응답코드 & 헤더만 존재(RequestBody가 없음)
- `POST` (Create)
    - 자원 생성 시 사용
    - 생성할 데이터를 RequestBody에 담아 전달함 (주로 JSON 형식)
    - POST Request에 대한 Response : 캐시 가능
- `PUT` (Create, Update)
    - 자원 대체 시 사용 (URI에 자원 식별자 표시)
    - 대체할 데이터를 RequestBody에 담아 전달함 (RequestBody에 담지 않은 내용은 해당 자원에서 삭제됨) → 자원을 수정할 때에는 PATCH를 사용할 것!
    - PUT Request에 대한 Response : 캐시 불가능
- `PATCH` (Update)
    - 자원 수정 시 사용
    - 수정할 데이터만 RequestBody에 담아 전달함 (RequestBody에 담지 않은 내용은 삭제 안됨)
    - 서버에서 PATCH 메소드를 지원하지 않는 경우 POST를 사용하면 됨
- `DELETE` (Delete)
    - 자원 삭제 시 사용 (URI에 자원 식별자 표시, 표시 안하면 모두 삭제됨)
    

### HTTP Method 속성

- `안전성`
    - 해당 HTTP 메소드를 호출해도 자원의 내용이 변경되지 않는 특성
    - 안전한 메소드는 서버의 상태를 아예 변경시키지 않음
    - GET, HEAD, OPTIONS, TRACE
- `멱등성`
    - 해당 HTTP 메소드를 여러번 호출해도 요청의 효과가 동등하다는 특성
    - 멱등한 메소드는 서버의 상태를 변경시킬 수도, 시키지 않을 수도 있음. 한 요청에 대한 서버의 상태는 항상 동일함
    - 멱등성은 서버가 정상적으로 Response를 못 보낼 경우, 클라이언트가 동일한 Request를 보내도 되는지 판단하기 위해 필요한 개념이다. → **자동 복구 매커니즘에 필요한 개념**
    - GET, PUT, DELETE
        - `GET /data` : 여러번 호출해도 자원 정보만 조회될 뿐 서버의 상태가 변하지 않음
        - `PUT /data/3` : 여러번 호출해도 해당 자원의 내용이 요청한 값으로 대체되었다는 결과가 동일함
        - `DELETE /data/3` : 여러번 호출해도 해당 자원이 삭제되었다는 결과가 동일함
        - `**POST /data` : 여러번 호출되면, 동일한 내용의 자원이 중복되어 생성되기 때문에 응답결과가 매번 달라져 멱등성을 만족하지 않음!**
- `캐시 가능`
    - 해당 HTTP 메소드의 응답 결과를 캐싱할 수 있다는 특성
    - GET, HEAD, POST(POST의 경우 응답 헤더에 Cache-Control 또는 Expires 필드가 포함되어있다면 캐싱이 가능함, [참고](https://stackoverflow.com/questions/626057/is-it-possible-to-cache-post-methods-in-http))

### HTTP Method 비교

- POST vs PUT

| POST | PUT |
| --- | --- |
| 리소스의 위치를 지정하지 않은 채, 리소스를 생성하는데 사용되는 메소드 | 리소스의 위치를 지정하여, 리소스의 내용을 대체하는데 사용되는 메소드 |
| POST 연산이 여러번 수행되면 중복된 리소스가 생성된다. | PUT 연산이 여러번 수행되어도, 지정된 리소스가 동일한 내용으로 대체된다. |
| 멱등성을 만족하지 않는다 → 리소스의 위치를 지정하지 않는 HTTP 요청 메소드이므로 | 멱등성을 만족한다 → 리소스의 위치를 지정하는 HTTP 요청 메소드이므로 |
- GET vs POST

|  | GET | POST |
| --- | --- | --- |
| 용도 | 서버의 자원을 조회할 때 사용한다. READ | 서버의 자원을 생성하거나 수정할 때 사용한다. CREATE, UPDATE |
| URL에 데이터 노출 여부 | URL에 쿼리스트링으로 데이터를 포함하여 요청하기 때문에 URL에 데이터가 노출된다. | URL에 데이터가 노출되지 않는다 |
| URL 예시 | `http://localhost:8080/boardList?name=제목&content=내용` | `http://localhost:8080/addBoard` |
| 데이터 위치 | HTTP Request Message의 header 부분에 URL이 담겨 전송된다. (URL의 쿼리스트링에 데이터가 포함됨) | HTTP Request Message의 body 부분에 데이터가 담겨 전송된다. |
| response 캐싱 | GET response 정보를 캐싱할 수 있다. | 기본적으로 POST response의 캐싱이 불가능하다.(응답 헤더에 Cache-Control 또는 Expires 필드를 포함하면 가능함) |
| 데이터 크기 제한 | 데이터를 URL이라는 공간에 담아 전달하기 때문에 전송가능한 데이터 크기가 제한적이다. | 바이너리 데이터와 같은 크기가 큰 데이터를 전송하기 적합하다. |
| 보안 | URL에 데이터가 노출되어 보안에 취약하다. | URL에 데이터가 노출되지 않아서 GET방식보다 상대적으로 보안적이지만, 암호화 하지 않는 이상 고만고만하다. |
| 멱등성 | 멱등성을 만족한다 (여러번 조회해도, 같은 결과를 얻는다) | 멱등성을 만족하지 않는다 (여러번 POST 요청을 날리면, 중복된 자원이 생성된다) |

- POST에 맞는 요청을 GET으로 수행할 경우 캐싱 문제 (전제조건 : POST 요청 헤더에 Cache-Control 또는 Expires 필드를 포함하지 않아 응답결과를 캐싱할 수 없는 경우)
    
    - GET 방식은 멱등성을 가지고 있다. 따라서 GET 요청을 여러번 수행하여도 같은 response를 얻을 수 있다. 그렇기 때문에 브라우저에서 GET의 response를 캐싱하여, 동일한 URL로 GET 요청을 보내면 캐싱된 데이터가 응답한다.
    
    - POST 방식은 멱등성을 만족하지 않는다. 즉, POST 요청을 여러번 수행하면 자원이 중복 생성되어 같은 결과를 얻지 못한다. 매 요청마다 결과가 다르다는 특성 때문에 POST의 response를 캐싱할 수 없다. 
    
    - 그럼에도 불구하고 POST 방식에 맞는 요청을 GET 방식으로 수행한다면, response가 캐싱되어 실제 자원의 내용과 다른 결과(not fresh response)를 얻는 문제가 있다.
    

## HTTP Response Message

```java
HTTP/1.1 200 OK	// 시작 줄
Connection: keep-alive	// 헤더
Content-Encoding: gzip
Content-Length: 35653
Content-Type: text/html;

<!DOCTYPE html><html lang="ko" data-reactroot=""><head><title... // 본문
```

- 서버가 클라이언트로 보내는 메시지
- HTTP Response Message는 3부분으로 구성됨
    1. 시작줄 : `HTTP 버전` & `상태 코드(Status Code)` & `상태 메시지(Status Message)`
    2. 헤더 : Request에 대한 정보를 나타냄
    3. 본문 : RequestBody (Request와 함께 전송할 데이터)

### 상태 코드(Status Code)

- 클라이언트가 보낸 요청의 처리 상태에 대한 코드
- 숫자 3자리로 이루어져있고, 크게 5개의 분류로 나눌 수 있다.

| 1XX | 정보전송 임시응답 | 요청을 받았으며 작업을 계속한다 |
| --- | --- | --- |
| 2XX | 성공 | 클라이언트의 요청을 수신하여 이해했고, 성공적으로 처리했다 |
| 3XX | 리다이렉션 완료 | 클라이언트는 요청을 마치기 위해 추가 동작을 취해야한다 |
| 4XX | 클라이언트 요청 오류 | 클라이언트에 오류가 있음을 나타낸다 |
| 5XX | 서버 오류 | 서버가 유효한 요청을 명백하게 수행하지 못했음을 나타낸다 |