# 네트워크 모델
- [TCP/IP 모델](#TCP/IP-모델)
- [OSI 7계층 모델](#OSI-7계층-모델)
- [두 모델 비교](#두-모델-비교)
- [패킷](#패킷)
- 참고 자료                  
        
        https://www.youtube.com/watch?v=y9nlT52SAcg&list=PL0d8NnikouEWcF1jJueLdjRIC4HsUlULi&index=4
        수제비 2020 정보처리기사 필기
        후니의 쉽게 쓴 네트워킹

## TCP/IP 모델
<img src="https://user-images.githubusercontent.com/46019755/125283501-70408780-e353-11eb-962f-b77308c4ae7d.png" width="50%" height="40%">

## OSI 7계층 모델
ISO에서 표준으로 지정한 모델   
**면접에서 가장 많이 물어보는 모델**             
<img src="https://user-images.githubusercontent.com/46019755/125271233-79c2f300-e345-11eb-9f7b-bfa90ae23da2.png" width="50%" height="60%">  
1계층(물리. Physical Layer) - 전기적, 기능적인 특성을 이용해서 통신 케이블로 데이터 전송
|계층|개념|특징|프로토콜|전송단위|장치|
|-|-|-|-|-|-|
|7계층: 응용<br>(Application Layer)|일반적인 응용 서비스 수행|응용 서비스 연결<br>데이터 생성|HTTP, FTP|데이터(Data)|L7 스위치|
|6계층: 표현<br>(Presentation Layer)|하위 계층에서 온 데이터를 사용자가 이해할 수 있는 형태로 만드는 역할|암/복호화<br>데이터 압축|JPEG, MPEG|데이터(Data)|-|
|5계층: 세션<br>(Session Layer)|논리적인 연결 담당|통신하는 사용자 동기화|SSH, TLS|데이터(Data)|L5 스위치|
|4계층: 전송 <br>(Transport Layer)|신뢰성 있는 데이터 전달|데이터 분할<br>흐름 제어<br>오류 제어<br>혼잡 제어|TCP(연결), UDP(비연결)|세그먼트(Segment)|L4 스위치|
|3계층: 네트워크<br>(Network Layer)|다양한 길이의 패킷 전달|라우팅<br>패킷 포워딩<br>인터네트워킹|ARP, IP, ICMP|패킷(Packet)|라우터, L3 스위치|
|2계층: 데이터링크<br>(Data Link Layer)|직접 연결된 노드 간 데이터 물리적인 전송|오류 제어<br>흐름 제어|Ethernet|프레임(Frame)|스위치, 브리지|
|1계층: 물리<br>(Physical Layer)|실제 장치들을 연결하기 위해 필요한 물리적 세부 사항 정의|0과 1의 비트 정보를 전기적 신호로 변환|RS-232C|비트(Bit)|허브|  

## 두 모델 비교
### 공통점
    - 계층적 네트워크 모델
    - 계층간 역할 정의
### 차이점
    - TCP/IP는 프로토콜 기반. 데이터 전송기술 (실무적)
    - OSI는 역할 기반. 통신 전반에 대한 표준 (논리적)
    
## 패킷
네트워크에서 전달하는 데이터의 형식화된 **블록**            
여러 프로토콜들로 캡슐화             
제어 정보와 사용자 데이터(페이로드)로 이루어짐 - 누가 누구에게, 어떻게 어떤 데이터를, 뭘 요청하는지, 뭘 보내주는지 등등             
헤더 + 페이로드(실질 데이터) + 풋터(잘 사용하지 않음)                

### 캡슐화
페이로드에 프로토콜을 헤더로 붙이는 과정                  
패킷을 **보낼 때** 사용     
상위 계층에서 하위 계층으로 내려가면서 프로토콜을 붙임
하위 계층에 상위 계층을 붙일 수 없음 
<img src="https://user-images.githubusercontent.com/46019755/125285479-a67f0680-e355-11eb-804d-49ddf99ff1f4.png" width="50%" height="60%">                  

### 디캡슐화
패킷을 **받을 때** 사용             
하위 계층부터 까보면서 상위 계층 확인          
<img src="https://user-images.githubusercontent.com/46019755/125286041-4a68b200-e356-11eb-8cf7-3cd8616ad80e.png" width="50%" height="60%">              

## 계층별 패킷 이름
### 세그먼트           
4계층까지 캡슐화된 상태           
**TCP(4계층) + 데이터**

### 패킷 => 네트워크상에서 전달되는 데이터(패킷)과 다른 의미
3계층까지 캡슐화된 상태                
**IPv4(3계층) + TCP(4계층) + 데이터**

### 프레임
2계층까지 캡슐화된 상태                
**Ethernet(2계층) + IPv4(3계층) + TCP(4계층) + 데이터**



