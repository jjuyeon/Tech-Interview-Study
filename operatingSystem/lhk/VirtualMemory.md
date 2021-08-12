# 목차
- [가상메모리](#가상메모리)
- [Demand Paging(요구 페이징)](#demand-paging요구-페이징)
- [Demand Paging의 성능](#demand-paging의-성능)
- 출처: KOCW 운영체제, 이화여자대학교 반효경 교수님

# 가상메모리
0번지부터 시작하는, 자신만의 메모리 주소 공간    
논리적 메모리와 물리적 메모리를 완전히 분리하여 실제 물리적 메모리의 크기보다 더 큰 프로그램을 실행할 수 있다.    

<img src="https://user-images.githubusercontent.com/46019755/128875891-32d45099-3ddb-42e1-90e2-1c4008fc276a.png" width="60%" height="80%">      


# Demand Paging(요구 페이징)
모든 페이지를 메모리에 한꺼번에 올리는 것이 아니라, 요청된 페이지만 메모리에 올리는 것    
- 메모리 사용량 감소
- 입출력 오버헤드 감소
- 더 많은 프로그램 동시 실행 가능

## 유효-무효 비트
- 하드웨어 지원 필요
- 현재 페이지가 메모리에 있는지(유효) 메모리에 없는지(무효) 확인하는 비트    
- 이때, 무효비트는 현재 사용되지 않는 주소 영역임을 나타낼 수도 있다.     
- 메모리에 없다는 것은, swap 영역에 내려가있다는 것으로 `page falut(페이지 부재)`가 발생했음을 의미        

<img src="https://user-images.githubusercontent.com/46019755/129144706-8faf5f9d-ed6c-4f96-b808-32d79bc7222d.png" width="60%" height="80%">      


## page fault 처리루틴
<img src="https://user-images.githubusercontent.com/46019755/129145288-6a44cab0-04d5-4a22-9350-7e7785a0f7e2.png" width="60%" height="80%">   

1. 페이지 요청         
2. invalid면 MMU가 trap을 걸어서 os에 요청              
    - 이때, 사용하지 않는 주소 영역이거나 읽기 전용 페이지에 쓰기를 하려고 한다면, 프로세스 종료             
3. backing store(hdd)에서 해당 페이지를 가져옴   
4. physical memory의 빈 프레임에 해당 페이지를 넣음                  
    - 빈 프레임이 없으면, 메모리에 있는 페이지 중 하나를 디스크로 쫗아냄(`swap out`)    
5. valid로 세팅   
6. restart instruction   


## Demand Paging의 성능
page fault rate를 p라고 했을 때, 
- 0이면, page fault가 전혀 나지 않는 다는 것. 즉, 메모리에서 모든 페이지를 얻을 수 있는 경우
- 1이면, page 요청할 때마다 page fault가 계속 난다는 것.        
### Effective Access Time(유효 접근 시간)
(1-p) x memory access + p (page fault 오버헤드 + 메모리에 빈 프레임이 없으면 swap out 오버헤드 + 요청 페이지 swap in 오버헤드 + 재시작 오버헤드)      
### Page Replacement
- os가 하는 업무
- 어떤 프레임을 내쫓아야하는지 결정해야 함
- 쫓아내고 바로 사용하지 않을 것 같은 프레임을 쫓아내는 것이 가장 효과적   
### Replace Algorithm
- page fault rate 최소화가 목표
- page fault를 얼마나 내느냐가 알고리즘의 성능 척도
- Optimal Algorithm, FIFO Algorithm, LRU Algorithm, LFU Algorithm       

