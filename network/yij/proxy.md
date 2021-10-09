# Proxy

## (Network)Proxy란?

서버와 클라이언트 사이에 중계기로써 클라이언트 대신 서버와 대리로 통신을 수행하는 것(프록시 서버라고 함)

- 클라이언트 대신 서버에게 요청을 받아 응답
- 캐시를 통해 자원들을 저장
- 프록시 서버를 거치는 요청 및 응답을 확인할 수 있음
- 프록시 서버로 넘어온 데이터를 조작할 수 있음

<br>

## Proxy Server의 역할

### Web Cache Proxy

클라이언트가 서버로 가기 전 웹 캐싱 프록시 서버를 통해 캐싱을 해주는 역할

- 네트워크 비용 감소
- 응답 속도 향상

### Filter

프록시 서버를 거치는 요청 및 응답을 확인할 수 있음
클라이언트의 요청을 가로채 접근 권한에 대해 제어할 수 있음

- 보안성 향상
- 프록시를 통한 모든 요청/응답 로깅 가능

### TransCoder

프록시 서버로 넘어온 데이터를 조작할 수 있음
(데이터 압축, 언어 반환)

ex)

- 스페인어를 사용하는 사용자 -> 응답 결과를 언어 변환해서 전달
- 웹브라우저가 탑재된 모바일 -> 응답 결과를 압축해서 전달

<br>

- 네트워크 비용 감소
- 웹 서버의 역할 감소

### Anonymizer

프록시 서버로 넘어온 데이터를 조작할 수 있음

ex)
익명화된 메시지는 식별화 정보를 포함하지 않는다

- 보안성 향상

<br>

## Forward Proxy vs Reverse Proxy

- Forward Proxy는 클라이언트 대신 요청을 보낸다
- Reverse Proxy는 서버의 응답을 대신 클라이언트에게 전달한다

<br>

### Forward Proxy

클라이언트와 인터넷 사이에 위치함

#### 캐싱

클라이언트가 요청한 내용 캐싱.

1. 전송 시간 절약
2. 불필요한 외부 전송 X
3. 외부 요청 감소로 인한 네트워크 병목 현상 방지

#### 익명성

클라이언트가 보낸 요청을 감춰서 Server가 응답 받은 요청을 누가 보냈는지 알지 못하게 함 (Server가 받은 요청 IP = Proxy IP)

<br>

### Reverse Proxy

#### 캐싱

Forward Proxy와 동일한 역할 수행

#### 보안

서버 정보를 클라이언트로부터 숨김. 클라이언트는 Reverse Proxy를 실제 서버라고 생각하여 요청하므로 실제 서버의 IP가 노출되지 않음

#### Load Balancing

여러 대의 서버에 요청을 나누어 진행할 수 있도록 결정해주는 역할(네트워크의 고가용성)

- 소규모 단위에서 배포중이던 서비스의 사용자가 증가했을 때 부하가 높아짐
- Scale Up(Server의 하드웨어 성능을 높이는 것)을 통해 부하를 줄이고자 함
- 서비스 규모가 커질 수록 Scale Up으로는 성능 향상의 한계가 있음
- Scale Out(여러 대의 Server가 나누어 일을 하는 것)으로 규모가 커질 때 마다 서버를 추가하는 방식으로 서비스 규모를 유동적으로 확대할 수 있음
- 해당 과정에서 여러 대의 서버가 분산 처리할 수 있도록 요청을 나누어주는 서비스를 Load Balancing이라고 함

##### Load Balancer 종류

- L4: Transport Layer(IP & Port) Level에서 Load Balancing(TCP/UDP)
  - ex) https://2jinishappy.tistory.com/ 으로 접근 시 서버 A, 서버 B로 로드밸런싱
- L7: Application Layer(User Request) Level에서 Load Balancing(HTTPS/HTTP/FTP)
  - ex) https://2jinishappy.tistory.com/ 으로 접근 시 /category와 /search를 담당 서버들로 로드밸런싱

<br>

### Proxy Server를 통한 무중단 배포 예시

1. SpringBoot 서버를 8081 Port로 구동
2. NginX Proxy 서버는 80 Port로 구동
3. EC2 Server에 접근 가능한 Port는 80밖에 없음
4. 80요청이 들어오면 Proxy Path를 통해 서버로 중개 가능
5. 새로운 변경 사항이 있어서 서버를 재시작 해야 할 경우 새로운 8082 Port로 서버를 구동시킴
6. NginX의 Proxy Path를 8082로 변경(리로드 시간은 체감할 수 없을 만큼 짧음)