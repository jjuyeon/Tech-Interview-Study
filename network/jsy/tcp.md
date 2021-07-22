# :question: TCP 연결지향형 프로토콜

#### reference
https://nirsa.tistory.com/29<br>
https://drehzr.tistory.com/725<br>
https://mindnet.tistory.com/entry/%EB%84%A4%ED%8A%B8%EC%9B%8C%ED%81%AC-%EC%89%BD%EA%B2%8C-%EC%9D%B4%ED%95%B4%ED%95%98%EA%B8%B0-22%ED%8E%B8-TCP-3-WayHandshake-4-WayHandshake<br>
https://www.crocus.co.kr/1362
<hr>

## Question
1. [TCP 프로토콜의 특징을 UDP 프로토콜과 비교하여 설명해주세요.](#4-tcp-vs-udp)
- UDP는 비신뢰성, 비연결을 지향하는 프로토콜인 반면, TCP는 데이터 전송 순서가 바뀌지 않도록 보장하고 데이터가 잘 전송되었는지 수신 여부를 확인하는 특징을 가지고 있습니다.
- 즉, TCP의 가장 큰 특징은 신뢰성과 연결지향적 프르토콜이라는 것입니다.
<br><br>

2. [3-way handshake에 대해 설명해주세요.](#2-3-way-handshake--연결-성립)
- TCP 통신을 시작할 때, 연결을 설정(요청/수락)하기 위해 사용합니다.
- 1. 클라이언트가 서버에게 SYN 패킷을 보내 연결을 요청합니다. 
- 2. 서버는 클라이언트에게 확인 및 연결 요청의 ACK+SYN 패킷을 전달합니다. 
- 3. 클라이언트는 확인 ACK 패킷을 전달하고, 통신을 시작합니다.
<br><br>

3. [3-way handshake에서 클라이언트가 서버가 보낸 ACK+SYC을 받지 못하면 어떻게 되나요?](#2-3-way-handshake--연결-성립)
- 클라이언트는 서버에게 SYN 패킷을 보내고 SYN_SENT상태로 일정시간동안 기다립니다. 
- Timeout이 되기 전까지 서버로부터 SYN/ACK 패킷이 받지 못하면, 클라이언트는 ***다시 SYN 패킷을 보내고 수신을 대기합니다.***  (무한로딩)
<br><br>

4. [4-way handshake에서 서버가 마지막에 FIN을 보내는 이유는 무엇인가요?](#3-4-way-handshake--연결-해제)
- 클라이언트가 데이터 전송을 마쳤다고 하더라도 서버는 아직 보낼 데이터가 남아 있을 경우를 고려하였기 때문입니다.
- 이러한 경우를 위해, 일단 클라이언트에게서 받은 FIN 패킷에 대한 ACK 패킷을 보냅니다.
- 이후, 서버에서 모든 데이터를 다 전송한 후에, 자신도 FIN 패킷을 보냅니다.
<br><br>

5. [4-way handshake에서 클라이언트가 마지막에 ACK를 보내는 이유는 무엇인가요?](#3-4-way-handshake--연결-해제)
- 서버가 FIN을 보내고 클라이언트에서 ACK(확인응답)을 받기 전까지, 서버는 계속 CLOSE_WAIT 상태로 기다리고 있습니다.
- 어플리케이션이 close()를 적절하게 처리하지 못해 CLOSE_WAIT 상태가 statement에 많아지게 된다면, 불필요한 자원 소모 뿐만 아니라, Socket Hang Up 에러가 발생할 수 있습니다. 
  - (Socket Hang Up: 너무 많은 Socket이 연결되어 있을 경우, 더 이상 할당이 불가능하여 연결을 하지 못한다.)
- cf) 즉, ***ACK(확인응답)를 받기 전까지는 항상 대기 상태라는 것을 잊지 않으면 된다.***
<hr>

## :nerd_face:	What I study
### 1. TCP (Transmission Control Protocol)
- 4계층에 위치하는, 신뢰성 있는 데이터 전송을 지원하는 프로토콜
- 신뢰성 있는 전송을 보장함으로써, 어플리케이션 구현이 훨씬 쉬워지게 되었다.
- 통신을 시작할 때, 3-way handshake를 통해 연결을 설정한다.
- 통신을 끝낼 때, 4-way handshake를 통해 연결을 해제한다.
- 장점
  - 오류제어, 흐름제어, 혼잡제어를 통해 신뢰성을 보장한다.
  - 패킷 도착 순서가 바뀌거나, 중복되거나, 누락되는 등의 문제가 없다.
  - IP 계층의 신뢰성 없는 서비스에 대해 다방면으로 신뢰성을 제공한다.
- 단점
  - UDP보다 전송 속도가 느리다.
- ex.<br>FTP 서버 (TCP로 파일을 공유하는 서버)<br>웹 HTTP 통신<br>이메일
#### 1) TCP header
![tcp]( https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbUV2Yw%2FbtqA0NrfJxc%2FOjE8FsNhslaapLzw5nwkt1%2Fimg.png)
- Source Port: 시작 포트번호
- Destination Port: 목적지 포트번호
- Sequence Number: byte 단위로 순서화되는 번호, 송신한 바이트 수 <br> -> 이를 통해, 신뢰성 및 흐름제어 기능을 제공하게 된다.
- Acknowledgment Number: 수신하기를 기대하는 다음 byte 번호, 수신한 바이트 수
- Offset: TCP header의 길이
- Reserved: 예약된 필드, 현재 사용되지 않음
- Flag: 현재의 통신 상태를 표현하는 플래그, 이를 통해 TCP 통신을 제어한다.<br> -> 각각의 플래그는 1비트를 차지한다. (ON/OFF)
- Window: 한 번에 수신할 수 있는 데이터의 크기, 흐름제어를 수행하게 되는 필드
- Checksum: 오류검출
- Urgent Pointer: 어디서부터 긴급한 값인지 알려주는 플래그
#### 2) TCP Flag
- URG(Urgent): 긴급 비트
- ACK(Acknowledgement): 승인 비트, 물어본 것에 대한 응답을 해줄 때 사용
- PSH(Push): 밀어넣기 비트, 버퍼와 상관없이 강제로 데이터를 밀어넣는다.
- RST(Reset): 초기화 비트, 연결 상태에서 문제가 발생하면 연결 상태를 리셋한다.
- SYN(Synchronization): 동기화 비트, 상대방과 연결을 시작할 때 무조건 사용되는 플래그
- FIN(Finish): 종료 비트, 세션 연결을 종료시킬 때 사용(더 이상 전송할 데이터가 없음)
#### 3) 오류제어
- 오류 검출, 재전송
- ARQ 기법을 통해, 프레임이 손상 / 손실된 경우, 재전송을 통해 오류를 복구한다.
- ex) <br> Stop and Wait ARQ, <br> Go-Back-n ARQ(슬라이딩 윈도우)
- ARQ 기법에 대한 구체적인 설명은 아래 링크를 참고하기
  - https://github.com/WooVictory/Ready-For-Tech-Interview/blob/master/Network/TCP.md
  - https://github.com/gyoogle/tech-interview-for-developer/blob/master/Computer%20Science/Network/TCP%20(%ED%9D%90%EB%A6%84%EC%A0%9C%EC%96%B4%ED%98%BC%EC%9E%A1%EC%A0%9C%EC%96%B4).md
#### 4) 흐름제어
- 송신측(호스트)과 수신측(호스트)의 데이터 처리 속도 차이를 해결하기 위한 기법
- 수신측에서 응답을 보낼 때, **윈도우 사이즈를 설정하여 현재 어느 정도까지 수신할 수 있는지** 수시로 알려준다.
- 이러한 기법이 나온 이유?
  - 수신측은 도착한 패킷을 버퍼에 쌓아두고, 동시에 버퍼에 쌓인 데이터를 순차적으로 꺼내서 처리한다.
  - 여기서 데이터를 처리하는 속도가 데이터가 들어오는 속도보다 느리면 문제가 발생할 수 있다.
  - 이를 해결하기 위해 윈도우 사이즈를 계속 응답으로 알려준다.
#### 5) 혼잡제어
- 송신측(호스트)의 데이터 전달과 네트워크(라우터)의 데이터 처리 속도를 해결하기 위한 기법
- 네트워크의 혼잡을 피하기 위해, **송신측에서 보내는 데이터의 전송 속도를 강제로 줄인다.**
- 이러한 기법이 나온 이유?
  - 하나의 라우터에게 데이터가 몰린 경우, 라우터가 모든 데이터를 다 처리할 수 없다.
  - 이러면 호스트들은 데이터를 재전송하게 되고, 결국 데이터손실이 발생할 수 있다.
  - 이러한 네트워크의 혼잡을 피하기 위해, 송신측에서 보내는 데이터의 전송 속도를 강제로 줄이게 되었다.
<br><br>

### 2. ***"3-way handshake"*** : 연결 성립
- TCP는 통신을 시작할 때, 3-way handshake를 통해 연결을 설정한다.
- 즉, 연결을 본격적으로 맺기 전에, 서로 TCP 연결을 요청/수락하는 준비 과정이다.

![3way](https://t1.daumcdn.net/cfile/tistory/225A964D52F1BB6917)
- 아래와 같은 3번의 통신이 완료되면 연결이 성립된다!<br><br>
1. 클라이언트가 서버에게 접속을 요청하는(포트를 열어달라는) SYN 패킷을 보낸다.
    - SYN을 보낸 후, 클라이언트는 SYN_SENT 상태가 된다. (SYN/ACK 응답을 기다림)
2. 서버는 클라이언트의 요청인 SYN 패킷에 대한 요청 수락 응답으로 ACK 패킷을 보낸다. <br> 또한, 클라이언트에게도 접속을 요청하는 SYN 패킷을 보낸다.
    - SYN을 보낸 후, 서버는 SYN_RECEIVED 상태가 된다. (ACK 응답을 기다림)
3. 클라이언트는 ACK, SYN 패킷을 받고 이에 대한 응답으로 ACK 패킷을 보낸다.
<br><br>

### 3. ***"4-way handshake"*** : 연결 해제
- TCP는 통신을 끝낼 때, 4-way handshake를 통해 연결을 해제한다.

![4way](https://t1.daumcdn.net/cfile/tistory/2152353F52F1C02835)
- 아래와 같은 4번의 통신이 완료되면 연결이 해제된다!<br><br>
1. 클라이언트는 서버에게 연결을 종료한다는 FIN 패킷을 보낸다.
2. 서버는 클라이언트의 요청인 FIN 패킷에 대한 요청 수락 응답으로 ACK 패킷을 보낸다.
    - ACK를 보낸 후, 서버는 CLOSE_WAIT 상태이다. (FIN 응답을 기다림)
3. 서버는 모든 통신을 다 처리했다면, 연결을 종료하겠다는 FIN 패킷을 클라이언트에게 보낸다.
4. 클라이언트는 FIN 패킷을 받고, 이에 대한 확인 응답으로 ACK 패킷을 보낸다.
    - ACK를 보낸 후, 클라이언트는 TIME_WAIT 상태이다.
    - TIME_WAIT: 서버로부터 FIN을 수신했지만, 아직 서버로부터 받지 못한 데이터가 있을 것을 대비하여 일정시간동안 기다리는 상태
    - TIME_WAIT가 대비하는 상황? 클라이언트에서 세션을 종료시킨 후, 뒤늦게 도착하는 패킷이 있다면 이 패킷은 버려지고 데이터는 손실된다. 이러한 상황을 방지하고자 만들어진 과정이 **TIME_WAIT** 이다.
5. 서버는 ACK를 받은 이후 소켓을 닫는다.<br> 클라이언트도 TIME_WAIT 시간이 끝나면 소켓을 닫는다.
<br><br>

### 4. TCP vs UDP
|TCP|UDP|
|:---:|:---:|
|***연결지향***|***비연결지향***|
|전송 순서 보장|순서 보장 X|
|데이터 수신 여부 확인|수신 여부 확인 X|
|***신뢰성***|***비신뢰성***|
|속도가 느리다|속도가 빠르다|
|파일전송, 이메일, 웹 HTTP 통신|DNS, RTP, Multicast|