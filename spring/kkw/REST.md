### REST

- Representational State Transfer
- 자원의 이름으로 구분하여 해당 자원의 상태를 주고 받는 모든 것을 의미
- 자원의 표현에 의한 상태 전달
- REST는 기본적으로 웹의 기술과 HTTP 프토로콜을 그대로 활용하기 때문에 웹의 장점을 최대한 활용할 수 있는 아키텍쳐 스타일

### REST의 개념

- HTTP URI를 통해 자원을 명시하고 HTTP METHOD를 통해 해당 자원에 대한 CRUD Operation을 적용하는 것을 의미
- REST는 자원 기반의 구조 설계의 중심에 Resource가 있고 HTTP Method를 통해 Resource를 처리하도록 설계된 아키텍쳐를 의미
- 웹 사이트의 이미지, 텍스트, DB 내용 등의 모든 자원에 고유한 ID인 HTTP URI를 부여한다
- CRUD Operation

  - Create : 생성(POST)
  - Read : 조회(GET)
  - Update : 수정(PUT)
  - Delete : 삭제(Delete)
  - HEAD : header 정보 조회(HEAD)

### 장단점

- 장점
  - HTTP 프로토콜의 인프라를 사용하기 때문에 REST API 사용을 위해 별도의 인프라 구축 필요 없음
  - HTTP 표준 프로토콜에 따르는 모든 기능을 사용이 가능
  - REST API 메세지가 의도하는 바를 명확하게 나타내므로 의도하는 바를 쉽게 파악 가능
  - 서버와 클라이언트의 역할을 명확하게 분리
- 단점
  - 표준이 존재하지 않음

### 구성

- 자원(Resource) : URI
  - 모든 자원에 고유한 ID가 존재하고, 이 자원은 Server에 존재
  - Client는 URI를 이용해서 자원을 지정하고 해당 자원의 상태에 대한 조작을 Server에 요청한다
- 행위(Verb) : HTTP Method
  - HTTP 프로토콜의 Method를 사용한다
  - GET, POST, PUT, DELETE
- 표현(Representation of Resource)
  - Client가 자원의 상태에 대한 조작을 요청하면 Server는 이에 적절한 응답을 보낸다
  - REST에서 하나의 자원은 JSON, TEXT, XML 등 여러 형태로 나타내질 수 있다
  - 일반적으로 JSON, XML으로 주고 받음

### REST 아키텍쳐에 적용되는 6가지 특징

- 인터페이스의 일관성
  - URI로 지정한 자원에 대한 조작을 통일되고 한정적인 인터페이스로 수행
  - HTTP 표준 프로토콜에 따르는 모든 플랫폼에서 사용이 가능하다
- Stateless
  - 각 요청 간 클라이언트의 콘텍스트가 서버에 저장되어서는 안 된다
  - Server는 각각의 요청을 완전히 별개의 것으로 인식하고 처리한다
- 캐시 처리 가능
  - 웹 표준 HTTP 프로토콜을 사용하므로 웹에서 사용하는 기존의 인프라를 그대로 활용이 가능
  - 캐시 사용을 통해 응답 시간이 빨라지고 REST Server에 트랜잭션이 발생하지 않기에 응답시간, 성능, 서버의 자원 이용률이 모두 상승한다
- 계층화
  - API서버는 순수 비지니스 로직만 수행, 클라이언트는 대상 서버에 직접 연결되었는지 중간 서버를 통해 연결되었는지 알 수 없다.
  - Proxy, 게이트웨이 같은 네트워크 기반의 중간 매체를 사용 가능
- Code on demand(Optional)
  - 서버가 클라이언트에게 코드를 응답해주면, 클라이언트는 응답 코드를 실행 할 수 있다
- 클라이언트/서버 구조 : 클라이언트와 서버가 독립적으로 분리되어 있다

### REST API

- REST 기반으로 서비스 API를 구현한 것
- 특징
  - REST 기반으로 시스템을 분산해 확장성과 재사용성을 높여 유지보수 및 운용을 편리하게 할 수 있다
  - HTTP를 지원하는 프로그램 언어로 클라이언트, 서버를 구현할 수 있다
- 설계 기본 규칙
  - URI는 정보의 자원을 표현해야 한다
  - 자원에 대한 행위는 HTTP Method로 표현한다
    1. URI에 HTTP METHOD가 들어가면 안 된다
       ```
         GET /user/delete/1 -> DELETE /user/1
       ```
    2. URI에 행위에 대한 동사 표현이 들어가지 않는다
       ```
         GET /user/show/1 -> GET /user/1
       ```
    3. 경로 부분 중 변하는 부분은 유일한 값으로 대체한다

### RESTful 하다?

- RESTful은 일반적으로 REST 라는 아키텍쳐로 구현한 웹 서비스를 나타내는 표현
- REST API를 제공하는 웹 서비스를 RESTful 하다고 할 수 있다
