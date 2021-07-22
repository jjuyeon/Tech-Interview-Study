# :question: UDP 비연결지향형 프로토콜

#### reference
https://rednooby.tistory.com/17<br>
https://github.com/WooVictory/Ready-For-Tech-Interview/blob/master/Network/UDP.md
<hr>

## Question
1. [UDP 서버의 특징에 대해서 설명하세요.](#1-udp-user-datagram-protocol)
- UDP는 단순히 데이터를 전송하는 역할만 하는 프로토콜입니다.
- UDP는 패킷의 전송 여부와 오류 여부를 파악할 수 없기 때문에 비신뢰성과 비연결성이라는 특징을 가지고 있습니다.
- 또한, 패킷이 잘 전송되었는지 파악하는 과정이 없기 때문에 상대적으로 전송 속도가 빠르다는 특징도 있습니다.
<br><br>

2. [UDP는 어느 상황에서 사용하는지 설명하세요.](#2-dns에서-udp를-사옹하는-이유)
- 상대적으로 데이터 전송의 신뢰성보다 속도가 중요한 **실시간 스트리밍, DNS 서버, RTP 프로토콜** 등에 사용됩니다.
<hr>

## :nerd_face:	What I study
### 1. UDP (User Datagram Protocol)
- 4계층에 위치하는, 단순히 데이터를 전송하는 역할만 하는 프로토콜
- 간단하기 때문에 신뢰할 수 없는 비연결성 프로토콜<br><br>
- 단점
  - 데이터그램의 도착 순서가 바뀌거나, 중복되거나, 누락되기도 한다. (비연결성, 비신뢰성)
  - 흐름제어와 에러제어가 없기 때문에, 손상된 세그먼트에 대한 재전송을 하지 않는다.
- 장점
  - **전송 속도가 빠르다** -> 실시간 스트리밍, 온라인 게임에 사용된다.
- 그래서 UDP는 일반적으로 오류의 검사와 수정이 필요없는 프로그램에서 수행할 것으로 가정한다.
- ex. <br>1) DNS 서버(도메인을 물으면 IP를 알려주는 서버), <br>2) TFTP 서버(UDP로 파일을 공유하는 서버), <br>3) RIP 프로토콜(라우팅 정보를 공유하는 프로토콜)<br>4) RTP(Real Time Protocol)<br>5) Multicast / Broadcast
#### 1) UDP Header
- 8바이트의 고정된 크기를 가진다.
![udp](https://t1.daumcdn.net/cfile/tistory/272A5A385759267B36)
- Source Port: 시작 포트번호
- Destination Port: 도착지 포트번호
- Length: 헤더와 데이터의 길이
- Checksum: 오류검출(Optional, 데이터의 훼손유무를 확인)
#### 2) DNS에서 UDP를 사옹하는 이유
- DNS는 7계층(어플리케이션 계층) 프로토콜
- DNS 요청이 많은데 TCP 처럼 세션을 맺고 통신한다면, <br>1) 속도도 느려지고, <br>2) 서버 리소스(overhead)도 엄청나게 소모될 것이다.
- 비신뢰성이라는 UDP의 단점을 보완하기 위해, 어플리케이션 계층에서 신뢰성을 추가할 수 있다. (timeout, resend)
#### 3) RTP에서 UDP를 사용하는 이유
- ex. 전화할 때, 손상된 데이터가 있어도 다른 조치를 하지 않고 수신자가 듣지 못한체로 넘어간다.
- ex. 온라인 게임 중에, 네트워크 상태가 좋지 않으면 잠시 화면이 끊겼다가(로딩) 다시 되돌아온다.
#### 4) Multicast / Broadcast에서 UDP를 사용하는 이유
- 1:N(all) 통신 방식에서 하나의 장치가 데이터를 받지 못하여 재전송을 요청할 수 있는 상황이라면, (TCP)<br>제대로 받은 다른 모든 장치에게도 데이터가 재전송된다는 문제점이 발생할 수 있다.
- 이를 방지하기 위해, UDP를 사용한다.