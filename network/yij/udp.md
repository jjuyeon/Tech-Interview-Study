## 비연결지향형 UDP 프로토콜

### UDP 프로토콜 - User Datagram Protocol
- 전송 방식이 단순해서 서비스의 신뢰성이 낮다
- 데이터그램 도착 순서가 바뀌거나, 중복되거나, 통보 없이 누락될 수 있다
- 일반적으로 오류의 검사와 수정이 필요 없는 프로그램에서 수행할 것으로 가정한다

<br>

> Source Port
> Destination Port
> Length
> Checksum

으로 이루어져 있다

### UDP 프로토콜을 사용하는 프로그램

1. DNS 서버(도메인을 물으면 IP 주소를 알려주는)
2. tftp 서버(UDP 프로토콜로 파일을 공유)
3. RIP 프로토콜(라우팅 정보를 공유)