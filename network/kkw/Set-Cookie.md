## Set-Cookie

### Cookie

- HTTP 쿠키는 서버가 브라우저에게 전송하는 데이터
- 클라이언트가 이를 저장하고 다시 서버에게 전송을 함으로써 서버는 Statless 해짐
- 주로 세 가지 목적을 위해 사용
  - 세션 관리 : 서버에 저장해야할 로그인, 장바구니, 게임 스코어 등의 정보 관리
  - 개인화 : 사용자 선호, 테마 등의 세팅
  - 트래킹 : 사용자 행동을 기록하고 분석하는 용도
- 과거에는 클라이언트 측의 정보를 저장할 때 주로 사용하였지만 지금은 권장하지 않음(현대는 modern storage APIs를 권장)

### Stateless

- 모든 HTTP request는 완전히 독룁 되어 있다는 의미
- 전후 요청에 영향을 받지 않는 상태
- 서버는 매 Request를 처리할 때마다 독립적으로 처리하므로 HTTP 프로토콜은 많은 정보량을 가지게 됨
- 이를 위해 쿠키를 사용함

### Set-Cookie 란?

- HTTP 요청을 수신할 때, 서버는 Response와 함께 Set-Cookie 헤더를 전송 가능
- 쿠키는 보통 브라우저에 의해서 저장되며, 그 후 쿠키는 같은 서버에 의해 만들어진 Request들의 Cookie HTTP header에 포함되어 전송됨

```
Set-Cookie: <cookie-name>=<cookie-value>
```

- 서버는 헤더에 클라이언트에게 다음과 같이 쿠키를 저장하라고 전달함

```
HTTP/1.0 200 OK
Content-type: text/html
Set-Cookie: yummy_cookie=choco
Set-Cookie: tasty_cookie=strawberry

```

- 그 후에는 서버로 보내는 모든 요청에 쿠키가 담기게 됨

```
GET /sample_page.html HTTP/1.1
Host: www.example.org
Cookie: yummy_cookie=choco; tasty_cookie=strawberry
```

### 쿠키의 종류

- Session Cookie
  - 클라이언트가 셧다운 되면 바로 사라지는 쿠키
  - Expires 혹은 Max-Age를 지정하지 않은 경우
- Permanent Cookie

  - 사용자가 브라우저를 열고 닫을 때마다 이러한 쿠키 설정이 보존 되도록 되있는 쿠키

- HttpOnly

  - XSS 공격 방지를 위해 Javascript의 Document.cookie API로 접근이 불가능

- Secure

  - HTTPS 프로토콜 상에서 암호된 요청일 경우에만 전송되는 쿠키
  - 안전하지 않은 사이트(http:)는 쿠키에 secure 설정을 지시할 수 없음
  - 그렇다고 이 방법이 실질적인 보안을 제공하진 않으니 민감한 정보를 담지 말 것

- path
- 쿠키가 특정한 디렉토리에서만 활성화 되도록 하기 위해서 Path를 설정할 수 있다.

- Samesite
- 서드 파티 쿠키들을 안전하게 사용하기 위해서 사용
- 총 3가지의 정책을 가지고 있음

  - None : 크로스 사이트의 요청에도 항상 전송 (즉, 서드 파티 쿠키도 전송됨)
  - Strict : 가장 보수적인 정책, 모든 쿠키는 크로스 사이트 요청에는 전송하지 않음. 퍼스트 파티 쿠키만 전송함
  - Lax : 대체로 서드 파티 쿠키도 전송되지 않지만, 몇가지 예외적인 요청에만 허용
    - Top-level-navigation : 링크 `<a>` 를 클릭하거나, window.location.replace 등으로 자동으로 이뤄지는 이동, `302` 리다이렉트를 이용한 이동
    - `iframe` 이나 `img`를 문서에 삽입함으로서 발생하는 HTTP 요청에는 적용 안 됨, `iframe` 안에서 이동하는 경우 안 됨)
    - `GET`처럼 서버의 상태를 바꿀 수 없는 요청에 한해서 전송됨

- 20년 2월 이후로 크롬은 Samesite 설정이 기본적으로 Lax로 설정됨(기존에는 None)
- Secure를 none으로 설정하려면 해당 쿠키는 반드시 `Secure` 상태여야만 한다.

### XSS (Cross-site Scripting)

- 공격 대상이 Client
- XSS는 사이트변조나 백도어를 통해 클라이언트에 대한 악성공격을 한다.
- 웹 브라우저에서 사용자가 입력 할수 있는 input태그 등에 악의적인 script를 작성하여 해당 contents를 이용하는 다른 이용자의 개인정보 및 쿠키정보 탈취, 악성코드 감염, 웹 페이지 변조등의 공격을 한다.

### CSRF(Cross Site Request Forgery)

- 공격 대상이 Server
- CSRF는 요청을 위조하여 사용자의 권한을 이용해 서버에 대한 악성공격을 한다.

1. 공격대상 사이트는 쿠키로 사용자 인증을 수행함.
2. 피해자는 공격 대상 사이트에 이미 로그인 되어있어서 브라우저에 쿠키가 있는 상태.
3. 공격자는 피해자에게 그럴듯한 사이트 링크를 전송하고 누르게 함. (공격대상 사이트와 다른 도메인)
4. 링크를 누르면 HTML 문서가 열리는데, 이 문서는 공격 대상 사이트에 HTTP 요청을 보냄.
5. 이 요청에는 쿠키가 포함(서드 파티 쿠키)되어 있으므로 공격자가 유도한 동작을 실행할 수 있음.

### 참고 사이트

- https://developer.mozilla.org/en-US/docs/Web/HTTP/Cookies#Secure_and_HttpOnly_cookies
- https://ko.javascript.info/cookie
