# HTTP 프로토콜
## HTTP 프로토콜

### HTTP 특징
- HyperText Transfer Protocol (하이퍼 텍스트 전송 프로토콜)
- www 에서 쓰이는 핵심 프로토콜
- 문서의 전송을 위해 쓰임
- 오늘날은 거의 모든 웹 어플리케이션에서 사용 중 -> 음성, 화상 등 여러 종류의 데이터를 MIME로 정의하여 전송 가능
- **Request / Response 동작에 기반하여 서비스 제공**

### HTTP 1.0
- 연결 수립, 동작, 연결 해제의 단순함이 특징 -> 하나의 URL은 하나의 TCP를 연결
- HTML 문서를 전송 받은 뒤 연결을 끊고 다시 연결하여 데이터를 전송함
- 단순 동작이 반복되어 통신 부하 문제 발생

### HTTP 1.1
- HTTP 1.0과 호환 가능
- Multiple Request 처리가 가능하여 Client의 Request가 많을 경우 연속정인 응답 제공 -> Pipeline 방식의 Request / Response 진행
- HTTP 1.0과 달리 Server가 갖는 하나의 IP Address와 다수의 Web Site 연결 가능
- 빠른 속도와 Internet Protocol 설계에 최적화될 수 있도록 Cache를 사용
- Data를 압축해서 전달이 가능하도록 하여 전달하는 Data 양이 감소

### HTTP 1.0과 1.1의 차이
- 커넥션 유지
  - 파이프라이닝
- 호스트 헤더
- 강력한 인증 절차

### HTTP 1.1의 단점
- HTTP HOLB (Head Of Line Blocking) 
  - 먼저 받은 요청은 뒤에 있는 요청이 끝날 때까지 대기해야하는 문제가 발생
  - 현대의 브라우저들은 파이프라이닝을 못하도록 막고 있음
- RTT (Round Trip Time)
  - TCP 상에서 동작하는 HTTP 특성상 Handshake가 반복적으로 일어나 불필요한 RTT가 증가함 -> 이는 네트워크 지연을 초래
- 무거운 Header 구조
  - 다수의 HTTP 요청에는 매 요청 시 마다 중복된 헤더값을 전송하게 됨
  - domain에 설정된 쿠키 정보도 매 요청 시 마다 헤더에 포함되어 전송되나보니 전송 값보다 헤더가 큰 경우가 발생


### HTTP 2.0
- SPDY
![SPDY](https://user-images.githubusercontent.com/43779730/127163069-2110f3cc-6a3a-4e9f-8d2a-0f2789ef7ad2.png)
  - 구글이 더 빠른 속도를 위해 지연 시간 관점에서 만든 프로토콜로 HTTP를 통한 전송을 재정의하는 형태로 구현하였다. 
  - HTTP 2.0은 SPDY를 기반으로 설계되었다.

- SPDY의 개선사항을 적용해 수정된 성능 향상에 초점을 맞춘 프로토콜

- **Multiplexed Streams**
  - 한 커넥션으로 동시에 여러 개의 메세지를 주고 받으며, 응답은 순서에 상관없이 Stream으로 받는다.
- **Stream Prioritization**
  - stream에 우선 순위를 결정하여 우선 순위가 높은 stream이 좀 더 높은 가중치를 가지게 된다.
  - 그렇다고 순서를 보장해주지는 않는다 (우선 순위 높은 것이 거절 당했다고 우선 순위 낮은 것도 포기하는 것을 원치 않기 때문)
- **Server Push**
  - 서버가 클라이언트의 요청이 없어도 리소스를 보내줄 수 있다.
  - 클라이언트가 요청할 것들을 미리 보내줄 수 있어 지연 시간을 줄일 수 있다.
- **Header Compression**
  - 기존 plain text 였던 header를 Huffman Coding을 사용하는 HPACK이라는 Header 압축방식을 이용하여 데이터 전송 효율을 높였다.
  - 중복된 header에 대해서는 index 값만 전송하고, 중복되지 않은 header 값만 인코딩하여 전송한다.
  - Huffman Coding 방식: 데이터 문자의 빈도에 따라서 다른 길이의 부호를 사용하는 알고리즘

  - HTTP 2.0의 단점
    - HTTP 2.0도 TCP를 사용하기 때문에 handshake의 RTT로 인한 지연시간, TCP의 HOLB 문제는 해결 할 수 없다.
  
참고 사이트 : https://developers.google.com/web/fundamentals/performance/http2?hl=ko

![Comparison-of-HTTP-versions](https://user-images.githubusercontent.com/43779730/127325328-9ffe45cd-888b-4bc7-ba63-f9228f74c395.png)



### HTTP 3.0

- QUIC
  - 구글에서 개발한 Quick UDP Internet Connections 의 약자
  - UDP를 기반으로 TCP + TLS + HTTP의 기능을 모두 구현하는 프로토콜
  - HTTP 3.0의 기반 기술
  - 기존 TCP를 수정하기가 어려워 UDP 기반에서 확장하는 것이 편하기 때문에 UDP 기반을 선택 (UDP는 커스터마이징이 용이하다)
  

    ![http3-1-1](https://user-images.githubusercontent.com/43779730/127327654-60efedb4-0d5c-4057-b414-741854efbb19.png)

- UDP 기반의 QUIC을 사용하여 통신하는 프로토콜
- UDP를 사용했다고 **신뢰성을 포기한 것은 아님**
- **RTT 감소로 인한 지연시간 단축**
  - TCP를 사용하지 않기 때문에 3 Way HandShake 과정을 거치지 않음
  - 클라이언트로 응답해주는 사이클을 RTT(Round Trip Time)이라고 하는데, TCP는 연결을 생성하기 위해 기본적으로 1RTT 가 필요하고, TLS를 암호화까지 한다면 3RTT가 필요함
  - 반면 QUIC은 연결 설정에 필요한 정보와 함께 데이터도 보내기 때문에 첫 연결 설정에 1RTT만 소요된다.
  - 한 번 연결에 성공하면 그 서버는 설정을 캐싱해놓고 있기 때문에 다음 연결 때는 0 RTT만에 바로 통신이 가능하다.

    - ![gcp-cloud-cdn-performance](https://user-images.githubusercontent.com/43779730/127328045-d0ed0619-8b8d-4fa7-aa66-091d1bc8a183.gif)


- **패킷 손실 감지에 걸리는 시간 단축**
  - QUIC은 헤더에 별도의 패킷 번호 공간을 부여하여 전송 순서 자체를 나타낸다.
  - 재전송시 동일한 번호가 전송되는 시퀸스 번호와 다르게 매 전송마다 패킷 번호가 증가하기 때문에 패킷의 전송 순서를 명확하게 파악할 수 있다.
  - TCP는 시퀸스 번호에 기반하여 암묵적으로 전송 순서를 파악하지만, QUIC은 고유한 패킷 번호를 통해서 바로 알아내기 때문에 패킷 손실 감지에 걸리는 시간이 단축됨
  - 관련 링크 : https://datatracker.ietf.org/doc/rfc9002/

- **멀티플렉싱을 지원**
- **클라이언트의 IP가 바뀌어도 연결이 유지됨**
  - QUIC은 Connection ID를 사용하여 서버와 연결을 생성한다. 
  - 이 Connection ID는 서버에서 생성한 랜덤한 값일 뿐, 클라이언트의 IP와는 무관하기 때문에 클라이언트의 IP가 변경되어도 기존의 연결을 계속 유지할 수 있음
  - 이는 모바일 기기의 경우 Wi-Fi
  
- 참고 사이트 : https://www.saturnsoft.net/network/2019/03/21/quic-http3-1/



### HTTP의 Status Code
- 100번대 : 정보를 확인할 때 사용
- 200번대 : 통신이 성공했을 때 응답을 받는 코드
- 300번대 : 요청을 마치기 위해 추가 동작을 취해야함
- 400번대 : 클라이언트에 오류가 있음을 나타냄
  - 400 : 요청이 잘못되었음
  - 401 : 권한이 없음
  - 404: 페이지가 없음
  - 405 : 요청에 지정된 방법을 사용할 수 없음 (ex . POST 방식에서 GET으로 요청함)
- 500번대 : 서버에 오류가 있음을 나타냄
  - 500 : 서버 내부에 오류가 있음
  - 502 : 게이트웨이 오류
  - 503 : 서비스가 이용이 불가능한 상태임


### 주소 창에 URL을 입력하면 일어나는 일
1) 웹 브라우저에 주소를 입력하게 되면, 그 주소에 해당하는 DNS 서버를 조회하여 IP 정보를 얻어온다
2) 생략된 포트 번호를 파악하기 위해 https의 포트인 443을 찾아내 http 요청 메세지를 생성한다.
3) 요청을 보내기 전 IP와 포트 정보를 사용해 3-Way-Handshake를 통해 연결을 확립한다
4) 요청 메세지가 전송 계층에 전달되어 TCP 헤더가 붙게 되고 -> 세그먼트
5) 네트워크 계층에 전달되어 IP 헤더가 붙게 된다 -> IP 패킷
6) 브라우저가 만들어진 패킷을 서버에 전달
7) 서버는 받은 패킷의 헤더를 제거하고 해석 후 요청을 처리
8) 처리된 결과를 통해 HTTP 응답 메세지를 생성하고 헤더를 붙여 패킷을 만든다
9) 만들어진 응답 메세지를 다시 브라우저에 전송함
10) 브라우저는 해당 패킷을 해석하여 HTML을 화면에 렌더링하게 된다.