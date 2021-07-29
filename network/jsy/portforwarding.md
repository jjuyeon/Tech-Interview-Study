# :question: NAT와 포트포워딩

#### reference
https://en.wikipedia.org/wiki/Network_address_translation<br>
https://ooeunz.tistory.com/104<br>
CM :heart_eyes:
<hr>

## Question
1. [NAT이란 무엇인지 설명해주세요.](#1-nat-network-address-traslation)
- 내부 IP와 외부 IP가 서로 통신하기 위해서는, IP 주소를 변환해주어야 합니다.
- 이때, NAT를 사용하여 이러한 주소 변환 규칙을 설정하고, 내부 IP와 외부 IP를 서로 변환하며 통신합니다.
<br><br>

2. [포트포워딩이란 무엇인지 설명해주세요.](#2-portforwarding-portmapping)
- 포트를 통해, 외부에서 내부망으로 접근이 가능하도록 하는 기술입니다.
- 즉, 설정해둔 포트로 외부에서 통신을 요청하는 경우, 매핑된 내부 IP로 포워딩해주는 기술입니다.
<hr>

## :nerd_face:	What I study
### 1. NAT (Network Address Traslation)
![nat](https://upload.wikimedia.org/wikipedia/commons/6/63/Network_Address_Translation_%28file2%29.jpg)
- 네트워크 주소 변환 규칙
- 외부망과 내부망을 나눠주는 기능을 한다.
- 내부 IP -> 외부 IP로 가기 위해서는, 내부 IP, 포트번호를 외부 IP, 포트번호로 변환해줘야 한다.<br>외부 IP -> 내부 IP로 가기 위해서는, 외부 IP, 포트번호를 내부 IP, 포트번호로 변환해줘야 한다. <br>그래서 ***NAT를 통해 이러한 주소 변환 규칙을 설정하고, 내부 IP 주소와 외부 IP 주소를 서로 변환하며 통신한다.***
- 맨 처음부터 외부 IP가 내부 IP로 접근할 수 없다. (내부 -> 외부는 통신이 허용된다.)
- 대부분 사설 네트워크에 속한 여러 개의 호스트가 하나의 공인 IP 주소를 사용하여 인터넷에 접속하기 위해 사용한다.
- 부족한 공인 IP 주소를 효율적으로 사용하기 위해, 공인 IP 주소와 사설 IP 주소를 변환해준다.

#### 1) 장점
- 여러 사설 네트워크를 사용함으로써 인터넷 공인 IP 주소를 절약할 수 있다.
- 내부 IP 주소를 외부로 알리지 않으면서, 외부의 침입/공격을 막을 수 있다.

#### 2) 단점
- 네트워크의 복잡성이 증가한다.
- 네트워크 성능의 지연에 영향을 끼친다.

#### 3) 왜 NAT를 쓰면서 통신해야할까? (내부 IP에서 외부 IP로 다이렉트로 통신은 왜 안될까?)
- 다른 네트워크에 있는 장치와 통신하기 위해서는, 각 네트워크의 공인 IP 주소를 통해서만 가능하다.
- 공인 IP 주소는 외부망 주소니까 당연히 내부망의 주소를 외부망의 주소로 변경해야하는 작업이 필요하다.
- 그래서 NAT를 사용해야만 한다.

#### 4) 왜 통신 처음에는 외부 IP가 내부 IP로 접속할 수 없을까? (포트포워딩이 필요한 이유)
- NAT는 NAT table을 통해 주소를 변환해준다.
- NAT table은 내부망이 외부망으로 외출할 때 업데이트된다. (테이블의 용도는 되돌아왔을 때, 내부망의 정확한 위치로 돌아갈 수 있도록 기록해두는 것)
- 그래서, 내부에서 외부로 먼저 통신 요청을 하지 않으면 외부에서는 내부 IP를 알 수 있는 방법이 없다. (왜? table에 업데이트가 안되니까)
- 이러한 문제를 해결하기 위해, 나타난 방법이 **포트포워딩**이다!
<br><br>

### 2. Portforwarding (PortMapping)
| | |
|---|---|
|![portforwarding](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FkB4pj%2FbtqDjHQjRGR%2F5qbbDDAcLKeI6PM1xxXWK0%2Fimg.png)|![portforwarding2](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fcgpupe%2FbtqDhvwvKrd%2FYPOxXh8Oj0jCzbbGafdLx1%2Fimg.png)| 
##### 1) 공유기를 설치하게 되면 공유기와 연결된 장치는 192.168~로 시작하는 IP를 공유기로부터 부여받는다.
##### 2) 공유기에게 21번 포트로 요청이 오면 192.168.0.20 PC로 연결하라는 이정표를 달아주는 것이 포트포워딩이다.
<br>

- 패킷이 라우터나 방화벽과 같은 네트워크 장비를 지나는 동안, **특정 IP 주소와 포트 번호의 통신 요청**을 **특정 다른 IP 주소와 포트 번호로 넘겨주는** NAT의 기능 중 하나이다.
- ***통신할 때 맨 처음부터 외부 IP가 내부 IP로 접근할 수 없었는데, 포트포워딩을 통해서 외부에서 접속이 가능하도록 할 수 있다.***
- 외부망(게이트웨이)의 반대쪽에 위치한 사설네트워크에 상주하는 호스트에 대한 서비스를 생성하기 위해 주로 사용된다.