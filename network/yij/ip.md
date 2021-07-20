## IP 프로토콜 - 네트워크 상에서 데이터를 교환하기 위한 프로토콜

데이터가 정확하게 전달될 것을 보장하지 않는다

중복된 패킷을 전달하거나 패킷의 순서를 잘못 전달할 가능성도 있다(악의적으로 이용하면 Dos 공격이 된다).

데이터의 정확하고 순차적인 전달은 그보다 상위 프로토콜인 TCP에서 보장한다



- version : IPv4 or IPv6인데 현재 IPv4를 사용

- IHL : (헤더의 길이)/4 를 2진수로 표현

- Total Length : PayLoad까지 합친 헤더의 길이

- Identification & IP Flags & Fragment Offset : 
- IP Flags : D - 패킷을 조각화하지 않겠다 / M - 첫 번째 패킷 뒤에 패킷이 더 있다는 것을 알림
- Fragment Offset : 조각화된 패킷의 순서를 알아내기 위함 
- TTL : 패킷이 살아있을 수 있는 시간
- Protocol : 전송계층 프로토콜 종류(상위 프로토콜)
- Header Checksum : 헤더에 오류가 있는지 체크하기 위함



## ICMP 프로토콜 - Internet Control Message Protocol

네트워크 컴퓨터 위에서 돌아가는 운영체제에서 오류 메시지를 전송 받는 데 주로 쓰인다

프로토콜 구조의 Type과 Code를 통해 오류 메시지를 전송 받는다

- 0 - 응답
- 8 - 요청
- 3 - 목적지까지 도달하지 못함
- 11 - 목적지에는 도달했으나 응답받지 못함(ex. 방화벽)
- 5 - 옛날에 쓰던 것으로 