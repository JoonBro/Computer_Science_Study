# Http

## Http란?

> `HTTP(Hyper Text Transfer Protocol)` 란 HTML(웹문서를 만들기 위한 언어) 문서를 주고 받는데 쓰이는 통신 프로토콜(통신규약)이며, TCP 와 UDP를 사용하여 통신하며, 80번 포트를 사용합니다.

- 웹에서 이루어지는 모든 데이터 교환의 기초이며, `클라이언트-서버 포로토콜`이기도 합니다.
- `클라이언트-서버 프로토콜`이란 수신자 측(웹 브라우저)에 의해 요청이 초기화되는 프로토콜
- TCP/IP를 이용하는 응용프로토콜(application protocol)입니다.
- - 응용 계층 (DNS, FTP, HTTP)
- 전송 계층 (TCP, UDP, SCTP)
- 네트워크 계층 (IP, ARP, RARP)
- 링크 계층 (이더넷, WIFI, 토큰링)
- HTTP는 연결상태를 유지하지 않는 `비연결성 프로토콜`입니다. 클라이언트가 이전에 요청한 내용을 기억하지 않습니다. 즉 요청, 응답 방식으로 동작
- 클라이언트(web browser) → 서버 : `요청(request)`
- 서버 → 클라이언트 : `응답(response)`
- 도메인 + 자원위치(URL), 도메인 + 자원의 식별자(URI)를 통해 요청을 하고, 서버가 요청에 따른 HTML 문서응답을 해줍니다.
- HTML외에도 JSON 데이터, XML과 같은 형태의 정보도 주고 받을 수 있습니다.

## HTTP Request(요청) 메세지

- 요청은 웹 브라우저의 URL을 통해 어느 웹사이트(도메인)의 어느 경로에 있는 페이지를 요청할지 나타내는 행위입니다.

![Untitled](https://user-images.githubusercontent.com/44499629/119152706-846eb380-ba8b-11eb-9313-63f7ba41f53e.png)

요청은 위 요소들로 구성!

- `**HTTP Method**` : 주어진 리소스에 수행하길 원하는 행동을 나타낸다. 클라이언트가 수행하고자 하는 동작을 의미합니다.

  ```markdown
  - GET : 요청받은 URI를 통해 특정 리소스를 검색하기 위해 요청하는 방식입니다. URL 뒤에 ? 를 사용
          하여 parameter를 전송하기 때문에 body영역을 사용하지 않습니다. 따라서 데이터 전송양에
          제한이 있습니다.
  - HEAD : GET 메서드와 동일한 응답을 요구하지만, 응답 본문(BODY)을 포함하지 않습니다.
           HEAD와 응답코드로만 응답, 보통 웹 서버의 다운 여부 점검(Health Check)이나 웹 서버
           정보(버전 등) 을 얻기 위해 사용될 수 있습니다.
  - POST : 요청된 자원을 생성(CREATE)합니다. 새로 작성된 리소스인 경우 HTTP 헤더 항목 LOCATION
  	       에 URI 주소를 포함하여 응답합니다. 그리고 데이터가 body영역을 통해 전송됩니다. 따라서 
           대용량 데이터를 보내기에 적합합니다.
  - PUT : 요청된 자원을 수정(UPDATE)합니다. 내용 갱신을 위주로 LOCATION(URI)를 보내지 않아도
          됩니다. 
  - DELETE : 요청된 자원을 삭제할 것을 요청합니다. (안정성 문제로 대부분의 서버에서 비활성)
  - CONNECT : 동적으로 터널모드를 교환, 프락시 기능을 요청시 사용됩니다.
  - OPTIONS : 웹서버에서 지원되는 메소드의 종류를 확인할 경우 사용합니다.
  - TRACE : 원격지 서버에 루프백 메세지를 호출하기 위해 테스트용으로 사용합니다.
  - PATCH : PUT과 유사하게 요청된 자원을 수정(UPDATE)할 때 사용합니다. PUT의 경우 자원 전체를
            갱신하는 의미지만, PATCH는 해당 자원의 일부를 교체하는 의미로 사용합니다.
  ```

  ### **HTTP POST과 PUT의 차이**

  ---

  **POST는 보통 INSERT의 개념**으로 사용되고, **PUT은 UPDATE개념**으로 생각하면 이해하기 쉽다. 또한 POST는 멱등하지 않고 PUT은 멱등하다. 즉 동일한 자원을 여러번 POST 하면 서버자원에는 변화가 생기지만, 여러번 PUT하는 경우는 변화가 생기지 않는다.

  예를들어 POST의 경우 클라이언트가 리소스의 위치를 지정하지 않는 경우 사용된다. (/dogs)

  따라서, 아래와 같은 요청이 여러번 수행되는 경우 매번 새로운 dog가 생성되어 dogs/3, dogs/4 등 매번 새로운 자원이 생성된다. 멱등하지 않다는 말이다.

  POST **/dogs** HTTP/1.1

  { "name": "blue", "age": 5 }

  HTTP/1.1 201 Created

  반면 PUT의 경우는 클라이언트가 명확하게 리소스의 위치를 지정한다. (/dogs/3)

  따라서, 아무리 많이 수행되더라도 리소스의 위치가 지정되어 새로운 자원이 생성되지 않으며 동일한 리소스(/dogs/3)를 수정하기 때문에 여러번 요청하더라도 멱등하다.

  PUT **/dogs/3** HTTP/1.1

  { "name": "blue", "age": 5 }

  ### **HTTP PUT과 PATCH의 차이**

  ---

  **PUT이 해당 자원의 전체를 교체**하는 의미를 지니는 대신, **PATCH는 일부를 변경한다는 의미**를 지니기 때문에 최근 update 이벤트에서 PUT보다 더 의미적으로 적합하다고 평가받고 있다. 또한 PUT의 경우는 멱등하지만, PATCH의 경우는 멱등하지 않다. PUT은 전체 자원을 업데이트 하기 때문에 동일 자원에 대해서 동일하게 PUT을 처리하는 경우 멱등하게 처리된다. 반면 PATCH로 처리되는 경우 자원의 일부가 변경되기 때문에 멱등성을 보장할 수 없다.

  ### **HTTP 멱등성**

  ---
  ![R1280x0](https://user-images.githubusercontent.com/44499629/119153198-fd6e0b00-ba8b-11eb-97b6-8e99de61187b.jpeg)


  멱등(idempotent)의 의미는 같은 작업을 계속 반복해도 같은 결과가 나오는 경우를 의미한다. 동일한 자원에 대한 GET요청이라면 클라이언트에 반환되는 모든 응답은 동일해야 한다. 특정 자원에 대한 DELETE의 경우도       자원은 더이상 이용하 수 없어야 하며, DELETE요청을 다시 호출한 경우도 자원은 여전히 사용할 수 없는 상태야여 한다.

  출처: [https://javaplant.tistory.com/18](https://javaplant.tistory.com/18)

## HTTP 응답(Response) 메세지

![Untitled 1](https://user-images.githubusercontent.com/44499629/119152788-96505680-ba8b-11eb-94bd-7684babd9efb.png)


## HTTP Status Code(상태코드)

`HTTP Status Code(상태코드)` 란 서버가 클라이언트에게 응답의 상태를 알리는 수단이며, 5가지 클래스로 분류됩니다.

- **1xx : Informational**
  - 서버가  요청을 클라이언트에서 성공적으로 수신했으며 서버 끝에서 처리중이라는 정보를 나타냅니다. 서버의 임시 응답이며 일반적으로 상태 줄과 선택적 헤더만 포함하며 빈 줄로 끝납니다. 현재는 거의 사용하지 않습니다.
- **2xx : Success**
  - 서버가 요청을 받고 성공적으로 처리되었음을 나타냅니다.
- **3xx : Redirection**
  - 브라우저는 자동으로 다른 URL로 리다이렉션되므로 브라우저 창에는 이 코드가 표시되지 않지만, 이미지 파일처럼 캐싱된 파일을 새로고침 후 확인하면 3xx를 확인할 수 있습니다.
- **4xx : Client Error**
  - 서버가 해결할 수 없는 클라이언트 측 에러 코드입니다. 주로 클라이언트(사용자)가 서버에 잘못된 요청을 했을 경우 발생합니다.
- **5xx : Server Error**
  - 서버가 클라이언트 요청을 처리하지 못할 경우 발생합니다. 통신하지 않는 것이 보안상 안전하여 대부분의 에러코드를 500 error로 처리합니다.

| 응답코드 |                             설명                             |
| :------: | :----------------------------------------------------------: |
|   100    | Continue(클라이언트로부터 일부 요청을 받았으며 나머지 정보를 계속 요청함) |
|   101    |                     Swtiching protocols                      |
|   200    |                              OK                              |
|   201    |    Created(PUT 메소드에 의해 원격지 서버에 파일이 생성됨)    |
|   202    |              Accepted(웹 서버가 명령을 수신함)               |
|   203    | Non-authoritative information(서버가 클라이언트 요청 중 일부만 전송) |
|   204    | No content(사용자 요구를 처리하였으나 전송할 데이터가 없음)  |
|   301    |  Moved Permanently(요구한 데이터를 변경된 타 URL에 요청함)   |
|   302    |                       Not temporarily                        |
|   304    | Not modified(컴퓨터 로컬의 캐시정보를 이용함, 대체로 gif 등은 웹서버에 요청하지 않음) |
|   400    |      Bad Request(사용자의 잘못된 요청을 처리할 수 없음)      |
|   401    |       Unauthorized(인증이 필요한 페이지를 요청한 경우)       |
|   402    |                   Payment required(에약됨)                   |
|   403    | Forbidden(접근 금지, 디렉터리 리스팅 요청 및 관리자 페이지 접근 등을 차단) |
|   404    |                 NotFound(요청한 페이지 없음)                 |
|   405    |     Method not allowed(허용되지 않는 http method 사용함)     |
|   407    |      Proxy authentication required(프락시 인증 요구됨)       |
|   408    |               Request timeout(요청 시간 초과)                |
|   410    |                  Gone(영구적으로 사용 금지)                  |
|   412    |             Precondition failed(전체 조건 실패)              |
|   414    |        Request-URI too long(요청 URL 길이가 긴 경우)         |
|   500    |            Internal Server error(내부 서버 오류)             |
|   501    |          Not Implemented(웹 서버가 처리할 수 없음)           |
|   503    |  Service unavailable(서버가 요청을 처리할 준비가 되지 않음)  |
|   504    |            Gateway Timeout(게이트웨이 시간 초과)             |
|   505    |   HTTP version not supported(해당 http 버전 지원되지 않음)   |

## REST API

**"REST 아키텍처의 제약 조건을 준수하는 애플리케이션 프로그래밍 인터페이스"**

REST는 Representational State Transfer의 줄임말이다. API(Application Programming Interface)는 애플리케이션 소프트웨어를 구축하고 통합하는 정의 및 프로토콜 세트이다.

---
Reference

https://velog.io/@doomchit_3/Internet-HTTP-개념차렷-IMBETPY

https://developer.mozilla.org/ko/docs/Web/HTTP/Methods

https://gyrfalcon.tistory.com/entry/HTTP-응답-코드-종류-HTTP-메소드-종류

---
