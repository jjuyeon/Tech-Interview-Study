# 목차
- [CPU Scheduling이 필요한 이유](#cpu-scheduling이-필요한-이유)
- [CPU Scheduling이 필요한 프로세스의 상태 변화](#cpu-scheduling이-필요한-프로세스의-상태-변화)
- [CPU Scheduling 성능 척도](#cpu-scheduling-성능-척도)
- [Scheduling Algorithms](#scheduling-algorithms)
  - [nonpremmptive](#nonpremmptive-scheduling비선점형)  
  - [preemptive](#preemptive-scheduling선점형)


# CPU Scheduling이 필요한 이유
프로그램은 CPU와 I/O를 번갈아가면서 수행하는데, 이때 자원을 효율적으로 사용하기 위해서 CPU Scheduling이 필요하다.


# CPU Scheduling이 필요한 프로세스의 상태 변화
1. Running -> Blocked (예: I/O 요청) => 자진반납(nonpreemptive)
2. Running -> Ready (예: 할당시간 만료) => 강제로 빼앗음(preemptive)
3. Blocked -> Ready (예: I/O 완료 후 인터럽트) => 강제로 빼앗음(preemtive) 
4. Terminate => 자진반납(nonpreemtive)
  
  
# CPU Scheduling 성능 척도
- `CPU utilization` : CPU 이용률
- `Throughput` : CPU 처리량
- `Turnaround time` : CPU를 사용하러 왔다가 다 쓰고 나가는 총 소요시간
- `Waiting time` : ready queue에서 기다린 시간
- `Response time` : ready queue에 들어와서 처음으로 CPU를 얻기까지 기다린 시간         

CPU 이용률, 처리량이 높을수록 // 소요시간과 기다린시간이 짧을수록 좋은 것이다.
  
  
# Scheduling Algorithms 
## nonpremmptive scheduling(비선점형) 
- [FCFS(First Come First Service)](#fcfsfirst-come-first-service)
- [SJF(Shortest Job First)](#sjfshortest-job-first)
## preemptive scheduling(선점형)
- [SRTF(Shortest Remaing Time First)](#srtfshortest-remaing-time-first)
- [RR(Round Robin)](#rrround-robin)


# FCFS(First Come First Service)
먼저 들어온 프로세스부터 처리한다. CPU 수행시간이 긴 프로세스가 먼저 들어오면 기다리는 시간이 길어지고, CPU 수행시간이 짧은 프로세스가 먼저 들어오면 기다리는 시간이 길어진다. 
즉, *먼저 들어오는 프로세스가 무엇인가에 따라 대기 시간이 결정된다.*          

특징
- `Convoy effect` : 먼저 들어온 CPU 수행시간이 긴 프로세스로 인해서 나중에 들어온 짧은 프로세스가 기다리는 현상


# SJF(Shortest Job First)
프로세스를 시작하는 시점에서, CPU 수행시간이 짧은 프로세스 먼저 처리한다. 프로세스처리도중 더 짧은 프로세스가 들어와도 현재 처리중인 프로세스를 끝까지 처리한다.         

특징
- `Starvation` : CPU 수행시간이 긴 프로세스가 무한히 기다리는 현상
- `Aging` : `Starvation`의 해결책으로, 기다릴수록 우선순위를 높여주어 처리되도록 한다.   


# SRTF(Shortest Remaing Time First)
현재 수행중인 프로세스보다 CPU 수행시간이 더 짧은 프로세스가 들어오면, 그 프로세스를 먼저 처리한다.              

특징
- `minimum average waiting time`을 보장한다.  

# RR(Round Robin)
각 프로세스는 동일한 크기의 할당 시간을 가지고 할당 시간이 끝나면 ready queue 뒤로 다시 줄을 선다. n개의 프로세스가 ready queue에 있고 할당 시간이 q일때, 
각 프로세스는 최대 q 시간 단위로 CPU를 1/n씩 얻는다. 즉, *어떤 프로세스도 (n-1)q 시간이상 기다리지 않는다.* 
이때, q가 크면 FCFS와 같은 효과이고 q가 작으면 context switch 오버헤드가 커진다.

특징
- `Response time`이 짧다.
- `Turnaround time`은 길다.
- `Waiting time`은 CPU 수행시간에 비례한다.
- 만약, CPU 수행시간이 동일한 프로세스 여러개가 들어오면 앞에서부터 처리해서 빨리빨리 집으로 보내면 되는데 굳이 쪼개서 다같이 집에 돌아갈 수 있다.
