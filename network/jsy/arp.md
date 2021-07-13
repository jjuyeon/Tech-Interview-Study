# :question: ARP 프로토콜

#### reference
https://coding-factory.tistory.com/720
<hr>

## Question
1. [ARP 프로토콜 동작 흐름에 대해 설명해주세요.](#1-arp-protocol-address-resolution-protocol)
- ARP 프로토콜은 IP 주소를 통해, 해당 IP 주소에 맞는 물리 주소를 알아내는 프로토콜입니다.
- 동작 흐름을 간단하게 설명드리면 <br>IP 프로토콜이 ARP 프로토콜에게 ARP 요청 메시지를 생성하도록 합니다. <br>이후 요청 메시지를 받은 호스트와 라우터 중, 요청 메시지의 목적지 IP 주소와 일치하는 시스템은 자신의 물리 주소(MAC 주소)를 포함하는 ARP 응답 메시지를 전송합니다.<br> 이러한 흐름을 통해, IP 주소를 통하여 MAC 주소를 얻을 수 있게 됩니다.
<br><br/>

2. [ARP와 RARP를 비교해서 설명해주세요.](#2-rarp-protocol-reverse-address-resolution-protocol)
- ARP 프로토콜과 RARP 프로토콜은 네트워크 계층의 주소 결정 프로토콜이라는 공통점을 가지고 있습니다.
- 차이점은 ARP는 논리 주소를 통해 물리 주소를 알아내는 반면, RARP는 물리 주소를 통해 논리 주소를 알아냅니다.

<hr/>

## :nerd_face:	What I study
### 1. ARP Protocol (Address Resolution Protocol)

#### 1) ARP 프로토콜에 대한 설명
![arp](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FdtXirx%2Fbtq3NGrivTP%2FSiJGrscVJ8Fd5Gl8P8IQgk%2Fimg.png)
- 주소 결정 프로토콜
- IP 주소를 그 IP 주소에 맞는 MAC 주소로 대응시키기 위해 사용되는 프로토콜
- [1] 처음 통신을 시작하면 상대방의 MAC 주소를 알 수 없음. <br>[2] IP 주소를 통해서는, 데이터를 보내고자 하는 PC의 네트워크까지만 접근할 수 있음. <br> --> 이때 ARP 프로토콜을 통해 상대방의 MAC 주소를 알아내기 위해 사용한다.

#### 2) 필요성
- IP 주소만으로 통신 불가능
  - IP 주소는 가변적
  - 그러므로, IP 주소만으로 통신하기엔 불가능
- MAC 주소만으로 통신 불가능
  - MAC 주소만으로 통신하기 위해서는, 전 세계의 모든 PC의 MAC 주소를 등록해야 함.
  - 저장 공간이 버티질 못함.

#### 3) 동작 흐름
1. 송신자는 목적지 IP 주소는 알고 있으나, MAC 주소는 모른다.
2. 송신자는 목적지 IP 주소를 지정하여 패킷을 송신한다.
3. MAC 주소를 알아내기 위해, IP 프로토콜이 ARP 프로토콜에게 ARP Request 메시지를 생성하도록 요청한다.
   - [참고] ARP Request 메시지는 브로드캐스트 방식으로 전송한다.
   - (전체에게 메시지를 보낼 수 있도록 브로드캐스트를 사용한다.)
4. 메시지는 데이터링크 계층으로 전달되고, 이더넷 프레임으로 Encapsulation 된다. 
   - [참고] "송신자 MAC 주소-> 발신지 주소, 수신자 MAC 주소-> 브로드캐스트 주소"로 지정한다.
5. 모든 호스트와 라우터는 이 프레임을 수신하고, 자신의 ARP 프로토콜에게 전달한다.
6. 목적지 IP 주소가 일치하는 시스템은 **자신의 MAC 주소를 포함하고 있는 ARP Reply 메시지를 보낸다.** 
   - [참고] ARP Reply 메시지는 유니캐스트 방식으로 전송한다.
   - (송신자가 Request 메시지에 자신의 물리주소를 포함했기 때문에 송신자에게만 보낼 수 있으므로, 유니캐스트를 사용한다.)
7. 최초 송신 측은 지정한 IP 주소에 대응하는 MAC 주소를 획득한다. (목적 달성!!)
<br><br/>

### 2. RARP Protocol (Reverse Address Resolution Protocol)

#### 1) RARP 프로토콜에 대한 설명
- 역순 주소 결정 프로토콜
- ARP 프로토콜과 반대 관계
- MAC 주소를 그 MAC 주소에 맞는 IP 주소로 대응시키기 위해 사용되는 프로토콜
- [참고]
  - RARP Request 메시지는 브로드캐스트 방식으로 전송한다.
  - RARP Reply 메시지는 유니캐스트 방식으로 전송한다.

#### 2) ARP vs RARP
- 모두 네트워크 계층 프로토콜

|ARP|RARP|
|:---:|:---:|
|주소 결정 프로토콜|ARP의 반대|
|IP -> MAC|MAC -> IP|