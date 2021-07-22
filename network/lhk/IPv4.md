# IP 주소 
- [IPv4](#ipv4)
- [IPv4 조각화](#ipv4-조각화)
- [ICMP](#icmp)
- [라우팅 테이블](#라우팅-테이블)  
- 참고 자료                  
        
        https://www.youtube.com/watch?v=s5kIGnaNFvM&list=PL0d8NnikouEWcF1jJueLdjRIC4HsUlULi&index=6&t=3s
        https://www.youtube.com/watch?v=_i8O_o2ozlE&t=141s
        https://www.youtube.com/watch?v=JaBCIUsFE74&list=PL0d8NnikouEWcF1jJueLdjRIC4HsUlULi&index=10&t=41s
        https://www.youtube.com/watch?v=CjnKNIyREHA&list=PL0d8NnikouEWcF1jJueLdjRIC4HsUlULi&index=12
        https://www.youtube.com/watch?v=_AONcID7Sc8&list=PL0d8NnikouEWcF1jJueLdjRIC4HsUlULi&index=14
        
        수제비 2020 정보처리기사 필기
        

## IPv4
- 네트워크 상에서 데이터를 교환하기 위한 프로토콜   
- 정확한 전달을 보장하지 않음     
- 중복된 패킷 전달, 패킷 순서 잘못 전달 가능성    
- 4계층 TCP가 데이터를 정확하고 순차적으로 전달    
- 20 Byte     
![Screenshot_1](https://user-images.githubusercontent.com/46019755/126610020-14f0c4d7-301a-494e-adc4-05414d38b9e6.png)
        
        Version - 4
        IHL(Header Length) - 최대 1111(15) 표현. 헤더 길이는 20 ~ 60이므로 4로 나눔. 보통 20이라 5
        Total Length - 페이로드까지 합쳐진 길이
        Identification - 쪼개진 데이터를 합치기 위해 id 부여
        IP Flags의 M - 여러번 쪼개면 1로 설정
        Fragment Offset - 받을 때 조립할 순서
        Time To Live(TTL) - 네트워크 장비 하나씩 지날때마다 1 감소. 상대방 os 종류를 알 수 있음. 윈도우(128), 리눅스(64)
        Protocol - 상위 프로토콜 종류
        Header Checksum - 오류 있는지 확인


## IPv4 조각화
- 보내려는 데이터가 MTU보다 클 때 사용
- 큰 IP 패킷들이 적은 MTU(Maximum Transmission Unit)를 갖는 링크를 통하여 전송되려면 여러 개의 작은 패킷으로 조각화 되어 전송
- 목적지까지 패킷을 전달하는 과정에서 통과하는 라우터마다 전송에 적합한 프레인으로 변환
- 한 번 조각화되면, 최종 목적지에 도달할 때까지 재조립되지 않음 => 재조립은 항상 최종 수신지에서만
- IPv4에서는 발신지, 중간 라우터에서 조각화 가능
- IPv6에서는 발신지에서만 가능    
- MTU에서 IPv4의 헤더길이 20을 뺀 만큼씩 조각화
- 뒤에 조각이 더 있으면 MF를 1로 세팅. Offset은 시작점에서 떨어진 만큼/8
- MTU를 지나서 Ethernet이 붙음    


## ICMP
- Internet Control Message Protocol (인터넷 제어 메시지 프로토콜)            
- 상대방과 통신이 되는지 확인 용도         
- 오류 메시지 전송 받는 데 사용           
- 프로토콜 구조의 Type과 Code를 통해 오류 베시지 전송 받음         
![Screenshot_2](https://user-images.githubusercontent.com/46019755/126607301-f854e0e2-4318-4f21-80dc-ffc521339b6b.png)       
![Screenshot_3](https://user-images.githubusercontent.com/46019755/126607489-53913c94-7eb7-4179-a54e-30e301fa54a9.png)        
        
        Type  
        0 - 응답
        8 - 요청
        3 - 목적지까지 가지 못한 경우 (경로 문제)
        11 - 목적지까지 갔는데, 응답이 없는 경우 (상대방 문제, 주로 방화벽)
        5 - 원격지의 상대방 라우팅 테이블 수정할 때 사용


## 라우팅 테이블  
`$netstat -r`    
서로 다른 네트워크 대역에 있는 A와 B의 통신   
1. 자신의 라우팅 테이블 확인  
2. B의 네트워크 대역이 라우팅 테이블에 있어야 출발 가능   
3. ICMP 프로토콜 작성(요청이므로 08)
4. IPv4 프로토콜 작성
5. Ethernet 프로토콜 작성(가까운 목적지 주소로)
6. 공유기를 지나다니며 목적지가 자신의 ip인지 확인 후, 아니면 Ethernet을 가까운 목적지 주소로 작성           
7. 6번 과정을 반복하며 B에 도착하면 ICMP에 응답(00) 작성 후 다시 A로       
        
        Ethernet은 네트워크 대역이 바뀔 떄마다 다시 작성    
        만약 목적지 주소의 MAC주소를 모르면 ARP 프로토콜로 알아와야 함
        
        
