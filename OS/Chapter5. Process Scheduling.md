# :bookmark_tabs: Operating System Concepts 9th edition      
## 5장 Process Scheduling   
__중요하다고 생각되는 목차에는 :star: 표시해놓았습니다.__   
__:star:되어 있는 목차를 클릭하시면 클릭하신 목차의 내용이 있는 페이지로 넘어가며__   
__해당 페이지 내에 있는 중요 개념 옆에 :star: 표시해놓았습니다.__   

__혹시 잘못된 내용이 있거나 보완해야할 점이 있으면 `issue` 해주시거나 알려주시면 감사하겠습니다.:bow:__   

* [5.Process Scheduling](#4process-scheduling)   
  - [5.1 Basic Concepts](#51-basic-concepts)   
    - [5.1.1 CPU-I/O Burst Cycle(CPU 입출력 버스트 사이클)](#511-cpu-io-burst-cyclecpu-입출력-버스트-사이클)   
    - [5.1.2 CPU Scheduler(CPU 스케줄러)](#512-cpu-schedulercpu-스케줄러) :star:   
    - [5.1.3 Preemptive Scheduling(선점 스케줄링)](#513-preemptive-scheduling선점-스케줄링) :star:   
    - [5.1.4 Dispatcher(디스패처)](#514-dispatcher디스패처) :star:   
  - [5.2 Scheduling Criteria](#52-scheduling-criteria) :star:   
  - [5.3 Scheduling Algorithms](#53-scheduling-algorithms)   
    - [5.3.1 First-Come, First-Served Scheduling(선입선처리 스케줄링)](#531-first-come-first-served-scheduling선입선처리-스케줄링) :star:   
    - [5.3.2 Shortest-Job-First Scheduling(최단 작업 우선 스케줄링)](#532-shortest-job-first-scheduling최단-작업-우선-스케줄링) :star:      
    - [5.3.3 Priority Scheduling(우선순위 스케줄링)](#533-priority-scheduling우선순위-스케줄링) :star:       
    - [5.3.4 Round-Robin Scheduling(라운드-로빈 스케줄링)](#534-round-robin-scheduling라운드-로빈-스케줄링) :star:     
    - [5.3.5 Multilevel Queue Scheduling(다단계 큐 스케줄링)](#535-multilevel-queue-scheduling다단계-큐-스케줄링) :star:    
    - [5.3.6 Multilevel Feedback Queue Scheduling(다단계 피드백 큐 스케줄링)](#536-multilevel-feedback-queue-scheduling다단계-피드백-큐-스케줄링) :star:     
  - [5.4 Thread Scheduling](#54-thread-scheduling)   
    - [5.4.1 Contention Scope(경쟁 범위)](#541-contention-scope경쟁-범위)     
    - [5.4.2 Pthread Scheduling](#542-pthread-scheduling)   
  - [5.5 Multiple-Processor Scheduling](#55-multiple-processor-scheduling)     
    - [5.5.1 Approaches to Multiple-Processor Scheduling(멀티프로세서 스케줄링 접근법)](#551-approaches-to-multiple-processor-scheduling멀티프로세서-스케줄링-접근법)     
    - [5.5.2 Processor Affinity(프로세서 친화성)](#552-processor-affinity프로세서-친화성) :star:   
    - [5.5.3 Load Balancing(부하 분산)](#553-load-balancing부하-분산) :star:     
    - [5.5.4 Multicore Processor(멀티코어 프로세서)](#554-multicore-processor멀티코어-프로세서) :star:     
  - [5.6 Real-time CPU Scheduling](#56-real-time-cpu-scheduling)     
    - [5.6.1 Minimizing Latency(지연 최소화)](#561-minimizing-latency지연-최소화) :star:     
    - [5.6.2 Priority-Based Scheduling(우선순위-기반 스케줄링)](#562-priority-based-scheduling우선순위-기반-스케줄링)     
    - [5.6.3 Rate-Monotonic Scheduling(비율-단조 스케줄링)](#563-rate-monotonic-scheduling비율-단조-스케줄링)     
    - [5.6.4 Earliest-Deadline-First Scheduling(최단 마감 우선 스케줄링)](#564-earliest-deadline-first-scheduling최단-마감-우선-스케줄링)     
    - [5.6.5 Proportional Share Scheduling](#565-proportional-share-scheduling)     
    - [5.6.6 POSIX Real-Time Scheduling(POSIX 실시간 스케줄링)](#566-posix-real-time-schedulingposix-실시간-스케줄링)   
  - [5.7 Operation System Examples](#57-operation-system-examples)   
  - [5.8 Algorithm Evaluation](#58-algorithm-evaluation)   
    - [5.8.1 Deterministic Modeling(결정론적 모델링)](#581-deterministic-modeling결정론적-모델링)   
    - [5.8.2 Queueing Models(큐잉 모델)](#582-queueing-models큐잉-모델)   
    - [5.8.3 Simulations(시뮬레이션)](#583-simulations시뮬레이션)   
    - [5.8.4 Implementations(구현)](#584-implementations구현)   

## 5.Process Scheduling   
### 5.1 Basic Concepts   

  - __멀티 프로그래밍은 CPU의 대기 시간을 활용하기 위해 한 순간에 다수의 프로세스들을 메모리에 저장시킨다.__   
    - 하나의 프로세스가 기다려야만 할 때, OS는 해당 프로세스를 CPU에서 제거하고 다른 프로세스에 CPU를 할당한다.   
    
#### 5.1.1 CPU-I/O Burst Cycle(CPU 입출력 버스트 사이클)   

  - __프로세스 실행은 CPU 실행과 입출력 사이클로 구성된다.__   
    - 프로세스들은 두 상태 사이에서 변경된다.   
    - 프로세스 실행은 CPU 버스트로 시작하고 입출력 버스트 - > CPU 버스트 -> 입출력 버스트 순으로 계속된다.   
      - 마지막 CPU 버스트는 실행 종료를 위한 시스템 요청과 함께 끝난다.   
      
      <p align="center"><img src="https://github.com/seongbeenkim/CS-Interview/blob/master/OS/image/5_1_CPU_BurstCycle.jpg" height = 400px title="5_1_CPU_BurstCycle" alt="5_1_CPU_BurstCycle"></img><br><strong>5.1 CPU, 입출력 버스트의 연속적인 변경</strong> </p>   
      
    - 입출력 중심 프로그램은 짧은 CPU 버스트를 많이 가진다.   
    - CPU 중심 프로그램은 다수의 긴 CPU 버스트를 가진다.   
    - 이러한 버스트의 분포는 CPU 스케줄링 알고리즘을 선택하는데 매우 중요하다.   

#### 5.1.2 CPU Scheduler(CPU 스케줄러)   

  - __CPU가 idle 될 때 마다 OS는 ready queue에서 프로세스 하나를 선택해야 한다.__   
    - __프로세스 선택은 단기 스케줄러(CPU 스케줄러)에 의해 수행된다.__   
      - 스케줄러는 메모리 내의 실행될 준비가 되어 있는 프로세스들 중에서 선택하여 CPU에 할당 한다.   
    - ready queue는 항상 FIFO queue일 필요가 없다.   
      - FIFO, Priority, Tree, Unordered linked list로 구현될 수도 있다.   
    - queue에 있는 기록들은 프로세스들의 PCB이다.   
    
#### 5.1.3 Preemptive Scheduling(선점 스케줄링)   

  - __CPU 스케줄링이 발생되야 하는 4가지 상황__ :star:      
    - __1. 프로세스가 실행 상태(Running state)에서 대기 상태(Waiting state)로 전환될 때__   
      - 입출력 요청 후 또는 자식 프로세스가 종료되기를 기다리기 위한 wait() 호출 후    
    - __2. 프로세스가 실행 상태(Running state)에서 준비 상태(Ready state)로 전환될 때__   
      - 인터럽트 발생    
    - __3. 프로세스가 대기 상태(Waiting state)에서 준비 상태(Ready state)로 전환될 때__   
      - 입출력 완료   
    - __4. 프로세스가 종료할 때__   
    
  - __1, 4의 경우에는 일반적으로 스케줄링의 선택권이 없다.__   
    - ready queue에 프로세스가 존재한다면, 새로운 프로세스가 반드시 선택되야 한다.   
    - __1, 4의 스케줄링 방법은 비선점 또는 협조적이라고 한다.__   
      - CPU가 프로세스에 할당되면, 프로세스가 제거되거나 대기 상태로 전환됨으로써 CPU를 해제할 때까지 CPU를 점유한다.   
      
  - __2, 3의 경우 선택권이 있다.__   
    - __2, 3의 스케줄링 방법은 선점이라고 한다.__   
  
  - __선점(Preemptive)__ :star:   
    - 여러 프로세스 사이에 데이터가 공유될 때, 경쟁 상태(race condition)가 필요하다.  
      - 하나의 프로세스가 데이터를 수정 중일 때, 다른 프로세스가 데이터를 읽기 위해 선점하면 이슈가 발생한다.   
    - 시스템 호출 처리 과정 중, 커널은 프로세스를 위한 활동으로 바쁠 수 있다.   
      - 이러한 활동이 커널 데이터(ex) 입출력 큐) 변경과 연관이 있을 수도 있다.   
        - 이러한 변경 중간에 프로세스가 선점되고 커널이 같은 구조를 수정하거나 읽어야할 경우 아주 큰 문제가 발생한다.   
    - UNIX의 대부분 버전을 포함하는 특정 OS는 문맥 교환하기전에 시스템 호출이 완료되기를 기다리거나, 입출력 block이 발생되기를 기다림으로써 이러한 문제를 해결한다.   
    - 이러한 선점 구조는 커널 자료 구조가 일관적이지 않을 동안 커널이 프로세스를 선점하지 않게 하기 때문에, 커널 구조는 간닪하다.   
      - 하지만 이러한 커널 실행 모델은 주어진 시간 내에 작업이 완료되어야 하는 실시간 연산을 지원하는데 있어서는 취약하다.   
      
  - __인터럽트는 언제든 발생할 수 있고, 커널에 의해 항상 무시될 수 없기때문에 인터럽트에 의해 영향을 받는 코드 영역은 동시 사용으로부터 보호되어야 한다.__   
    - OS는 거의 항상 인터럽트를 받아들여야 한다.   
      - 그렇지 않을 경우 입력이 손실되거나 출력이 겹쳐서 쓰여질 수 있다.   
    - 이러한 코드 부분에 여러 프로세스가 병행으로 접근할 수 없도록 하기 위해서, 진입 시에 인터럽트를 disable하고 종료 시 reenable한다.   
    - 인터럽트를 disable하는 코드 영역은 자주 발생해서는 안되며 아주 적은 명령어만 포함해야한다.   
    
#### 5.1.4 Dispatcher(디스패처)   
  
  - __단기 스케줄러에 의해 선택된 프로세스에 CPU의 제어를 넘겨주는 모듈__   
    - 문맥 교환   
    - 사용자 모드로 전환   
    - 프로그램을 재시작하기 위해 사용자 프로그램의 적절한 위치로 이동   
    
  - __모든 프로세스 전환간 발생하기 때문에 가능한 한 아주 빨라야 한다.__   
  
  - __디스패치 지연(Dispatch latency)__   
    - 디스패처가 하나의 프로세스를 정지하고 다른 프로세스를 실행시키는데 걸리는 시간   
    
### 5.2 Scheduling Criteria   

  - __스케줄링 알고리즘 비교를 위한 속성__   
    - __CPU 이용률(CPU utilization)__   
      - __CPU를 최대한 바쁘게 만들어야 하기 때문에 40 ~ 90% 범위 안으로 사용해야한다.__   
      
    - __처리량(Throughput)__   
      - CPU가 프로세스를 실행시키느라 바쁘게 되면, 작업은 완료된다.   
      - __작업의 양을 측정하는 것은 time unit당 완료되어 지는 프로세스의 수이다.__   
        - ex) 긴 프로세스의 경우, 이러한 비율이 시간당 하나의 프로세스일 수도 있고, 짧은 프로세스의 경우, 초당 10개의 프로세스일 수 있다.   
    
    - __반환 시간(Turnaround time)__   
      - 특정 프로세스의 관점에서, 해당 프로세스를 실행하는데 걸리는 시간은 중요하다.   
      - __프로세스의 진입 시간부터 완료 시간까지의 시간이다.__   
      - __ready queue에서 기다리는 시간, CPU에서 실행되는 시간, 입출력하는 시간, 메모리에 적재되는데 기다리는 데 걸리는 시간의 합이다.__   
      - 반환 시간은 보통 출력장치의 속도에 의해 제한된다.   
    
    - __대기 시간(Waiting time)__   
      - CPU 스케줄링 알고리즘은 프로세스가 실행 또는 입출력을 하는 동안의 시간의 양에 영향을 주지 않는다.   
      - 오직 ready queue에서 프로세스가 기다리는 시간의 양에만 영향을 준다.   
      - __ready queue에서 기다린 기간의 합이다.__   
      
    - __응답 시간(Response time)__   
      - 상호 작용 시스템에서, 반환 시간은 가장 중요한 기준이 아닐 수도 있다.   
      - 프로세스는 꽤 빠르게 결과를 만들어 낼 수 있고 이전 결과가 사용자에게 출력되는 동안 연산을 계속 할 수 있다.   
      - __요청의 제출 시점부터 첫 응답이 생산될 때 까지의 시간이다.__   
      - 응답을 출력하는데 걸리는 시간이 아니라 응답을 시작하는데까지 걸리는 시간이다.   
      - 반환 시간은 보통 출력장치의 속도에 의해 제한된다.   
      
  - __CPU 이용률, 처리량을 최대화 시키고, 반환 시간, 대기 시간, 응답 시간을 최소화하는 게 이상적이다.__
    - 최악의 경우, 평균 값을 최적화한다.   
      - 일부의 경우, 최대 값 또는 최소 값을 최적화한다.   
        - ex) 모든 사용자가 좋은 서비스를 받는 것을 보장하기 위해 최대 응답 시간을 최소화 시키기를 원할 수도 있다.   
        
    - 상호 시스템의 경우, 평균 반응 시간을 최소화 하는 것보다 반응 시간에서의 변화를 최소화 시키는 것이 더 중요하다.   
      - 예측 가능하고 합리적인 반응 시간을 가진 시스템은 일반적으로 더 빠른 시스템보다 더 이상적이라고 고려될 수 있지만, 변동이 심하다.   
      - 오직 적은 양의 작업만 변화를 최소화하는 CPU 스케줄링 알고리즘에서 완료된다.   
      
### 5.3 Scheduling Algorithms   
#### 5.3.1 First-Come, First-Served Scheduling(선입선처리 스케줄링)   

  - __FCFS__ :star:   
    - __가장 간단한 CPU 스케줄링 알고리즘__    
      - __CPU에 먼저 요청한 프로세스가 CPU에 먼저 할당된다.__   
      - __FIFO queue에 의해 관리된다.__   
        - 프로세스가 ready queue에 들어갈 때, 프로세스의 PCB는 queue의 끝에 연결된다.   
        - CPU가 free일 때, queue의 앞부분에 있는 프로세스에 할당된다.   
      - __평균 대기 시간이 길다.__   
      - __호위 효과(Convoy effect)__   
        - 모든 프로세스들이 하나의 큰 프로세스가 CPU를 해제할 때까지 기다리는 것   
        - 더 짧은 프로세스가 먼저 시작되게 하는 것보다 더 낮은 CPU와 장치 이용량을 산출한다.   
      - __비선점형__   
        - 프로세스가 종료되거나 입출력 요청함으로써 CPU를 해제할 때까지 CPU를 점유한다.   
        - 시분할 시스템에서 부적절하다.   
          - 시분할 시스템에서는 각 사용자가 규칙적인 간격으로 CPU의 몫을 얻는 것이 매우 중요하기 때문이다.   
   
        <table align="center">
          <thead>
            <tr>
              <th>프로세스</th>
              <th>버스트 시간</th>
            </tr>
          </thead>
          <tbody>
            <tr>
              <td>P1</td>
              <td>24</td>
            </tr>
            <tr>
              <td>P2</td>
              <td>3</td>
            </tr>
            <tr>
              <td>P3</td>
              <td>3</td>
            </tr>
          </tbody>
        </table>   
        
        <p align="center"><img src="https://github.com/seongbeenkim/CS-Interview/blob/master/OS/image/5_FCFS_Chart1.jpg"  title="5_FCFS_Chart1" alt="5_FCFS_Chart1"></img><br><strong> P1, P2, P3 순서로 도착했을 때</strong><br> 평균 대기시간 : (0 + 24 + 27) / 3 = 17 </p>   
        
        <p align="center"><img src="https://github.com/seongbeenkim/CS-Interview/blob/master/OS/image/5_FCFS_Chart2.jpg"  title="5_FCFS_Chart2" alt="5_FCFS_Chart2"></img><br><strong> P2, P3, P1 순서로 도착했을 때</strong><br> 평균 대기시간 : (6 + 0 + 3) / 3 = 3 </p>   

#### 5.3.2 Shortest-Job-First Scheduling(최단 작업 우선 스케줄링)   

  - __SJF__ :star:   
    - __다음 CPU 버스트의 길이가 가장 작은 프로세스를 먼저 CPU에 할당시킨다.__       
      - __프로세스의 CPU 실행시간이 같은 경우 FCFS 방식이 사용된다.__   
      - __FCFS보다 평균 대기 시간이 짧다.__   
      - __다음 CPU의 버스트 길이를 알 수 없다.__   
        - 일괄 처리 시스템에서 장기 스케줄링의 경우 사용자가 명시한 프로세스 제한 시간을 사용할 수 있다.   
        - 사용자들이 프로세스 시간 제한을 정확하게 예상하도록 동기를 부여받는다.   
          - 값이 낮을 수록 빠른 응답 속도를 나타내기 때문이다.   
          - 하지만 너무 작은 값은 시간 초과 오류를 일으키고 재실행이 필요하다.   
          - 일반적으로 장기 스케줄링에서 자주 사용된다.   
          
          <table align="center">
            <thead>
              <tr>
                <th>프로세스</th>
                <th>버스트 시간</th>
              </tr>
            </thead>
            <tbody>
              <tr>
                <td>P1</td>
                <td>6</td>
              </tr>
              <tr>
                <td>P2</td>
                <td>8</td>
              </tr>
              <tr>
                <td>P3</td>
                <td>7</td>
              </tr>
              <tr>
                <td>P4</td>
                <td>3</td>
              </tr>
            </tbody>
          </table>   
          <p align="center"><img src="https://github.com/seongbeenkim/CS-Interview/blob/master/OS/image/5_SJF_Chart.jpg"  title="5_SJF_Chart" alt="5_SJF_Chart"></img><br><strong> SJF</strong><br> 평균 대기시간 : (3 + 6 + 9 + 0) / 4 = 7 </p>   
        
      - __단기 스케줄링에서는 구현될 수 없다.__   
        - 단기 스케줄링에서 다음 CPU 버스트 길이를 알 방법이 없다.   
        - 하지만 이러한 길이를 예상할 수 있는 방법이 있다.   
          - 다음 CPU 버스트는 이전 CPU 버스트와 비슷할 것이라고 예측한다.   
          - 다음 CPU 버스트의 길이의 근사값을 계산해 가장 짧다고 예상된 CPU 버스트를 가진 프로세르를 선택할 수 있다.   
          - 일반적으로 다음 CPU 버스트는 이전 CPU 버스트들의 측정된 길이의 지수 평균으로 예상되어진다.    
          
          <p align="center"><img src="https://github.com/seongbeenkim/CS-Interview/blob/master/OS/image/5_3_BurstPrediction.jpg"  height = 400psx title="5_3_BurstPrediction" alt="5_3_BurstPrediction"></img><br><strong>5.3 다음 CPU 버스트 길이 예측</strong><br></p>   

      - __선점형 또는 비선점형__   
        - 이전 프로세스가 계속 실행되는 동안 ready queue에 새로운 프로세스가 도착할 경우 선택이 발생한다.   
        - __선점 - 새로운 프로세스의 다음 CPU 버스트가 현재 실행되고 있는 프로세스의 남은 CPU 버스트보다 작을 경우 선점한다.__   
          - 최소 잔여 시간 우선(shortest-remaining-time-first) 스케줄링이라고 불린다.   
          
          <table align="center">
            <thead>
              <tr>
                <th>프로세스</th>
                <th>도착 시간</th>
                <th>버스트 시간</th>
              </tr>
            </thead>
            <tbody>
              <tr>
                <td>P1</td>
                <td>0</td>
                <td>8</td>
              </tr>
              <tr>
                <td>P2</td>
                <td>1</td>
                <td>4</td>
              </tr>
              <tr>
                <td>P3</td>
                <td>2</td>
                <td>9</td>
              </tr>
              <tr>
                <td>P4</td>
                <td>3</td>
                <td>5</td>
              </tr>
            </tbody>
          </table>   
          
          <p align="center"><img src="https://github.com/seongbeenkim/CS-Interview/blob/master/OS/image/5_PreemptiveSJF_Chart.jpg"  title="5_PreemptiveSJF_Chart" alt="5_PreemptiveSJF_Chart"></img><br><strong> 선점 SJF</strong><br> 평균 대기시간 : {(10-1) + (1-1) + (17-2) + (5-3)} / 4 = 6.5 </p>   
          
        - __비선점 - 현재 실행하는 프로세스를 종료하고 새로운 프로세스에 CPU를 할당한다.__   
      
#### 5.3.3 Priority Scheduling(우선순위 스케줄링)   

  - __우선순위는 각 프로세스들과 연관되어 있으며 CPU는 가장 높은 우선순위를 가진 프로세스에게 할당된다.__ :star:   
    - 우선순위가 같은 프로세스들은 FCFS방식으로 스케줄된다.   
    
  - __SJF는 우선순위 스케줄링 알고리즘의 특별한 경우다.__   
    - 더 많은 CPU 버스트를 가질수록 우선순위가 낮아진다.   
  
  - __0이 가장 높은 우선순위일 수 있고 가장 낮은 우선순위일 수 있다__   
    - 아래의 내용부터는 값이 낮을수록 높은 우선순위로 가정   
    
    <table align="center">
      <thead>
        <tr>
          <th>프로세스</th>
          <th>버스트 시간</th>
          <th>우선순위</th>
        </tr>
      </thead>
      <tbody>
        <tr>
          <td>P1</td>
          <td>10</td>
          <td>3</td>
        </tr>
        <tr>
          <td>P2</td>
          <td>1</td>
          <td>1</td>
        </tr>
        <tr>
          <td>P3</td>
          <td>2</td>
          <td>4</td>
        </tr>
        <tr>
          <td>P4</td>
          <td>1</td>
          <td>5</td>
        </tr>
        <tr>
          <td>P5</td>
          <td>5</td>
          <td>2</td>
        </tr>
      </tbody>
    </table>   
    <p align="center"><img src="https://github.com/seongbeenkim/CS-Interview/blob/master/OS/image/5_PriorityChart.jpg"  title="5_PriorityChart" alt="5_PriorityChart"></img><br><strong> 우선순위 스케줄링</strong><br> 평균 대기시간 : 8.2 </p>   
    
  - __우선순위는 내부적 또는 외부적으로 정의될 수 있다.__   
    - __내부적__   
      - 프로세스의 우선순위를 연산하기 위해 측정 가능한 양을 사용한다.   
        - ex) 시간 제한, 메모리 필요성, 열린 파일의 수, CPU 버스트 평균과 입출력 버스트 평균의 비율이 사용된다.   
    
    - __외부적__   
      - OS 외부 기준으로 정의된다.   
        - ex) 프로세스의 중요도, 컴퓨터 사용을 위해 지불된 자금의 종류와 양, 작업을 원조하는 부서 등 다른 요소   
        
  - __선점형 또는 비선점형__ :star:    
    - __선점 - 새로 들어온 프로세스의 우선순위가 현재 실행되는 프로세스의 우선순위보다 높을 경우 CPU를 선점한다.__   
    - __비선점 - 새로운 프로세스를 ready queue의 앞 부분에 넣는다.__    
    
  - __우선순위 알고리즘의 문제__ :star:   
    - __무한 블락(Indefinite blocking)__   
      - 실행할 준비가 되어 CPU를 기다리는 프로세스가 block된 상태로 간주될 수 있다.   
    - __기아 현상(Starvation)__   
      - 우선순위가 높은 프로세스들이 계속 들어와 우선순위가 낮은 프로세스들이 CPU를 무한히 대기하게 될 수도 있다.   
      
    - __해결 방안__   
      - __Aging__   
        - 오랜동안 기다리고 있는 프로세스의 우선순위를 점차적으로 증가시킨다.    
        
#### 5.3.4 Round-Robin Scheduling(라운드-로빈 스케줄링)    

  - __Round-Robin__ :star:    
    - 시분할 시스템을 위해 고안되었다.   
    - __FCFS + 시간 할당량(Time quantum) or 시간 조각(Time slice) => 선점__   
      - 프로세스가 작업을 완료하지 못하더라도 할당된 시간이 지나면 다음 프로세스가 실행된다.   
      - 시간 할당량은 0 ~ 100ms가 일반적이다.   
    - __ready queue는 원형 큐로 동작한다.__   
      - 새로운 프로세스는 ready queue의 끝에 추가된다.   
      - __CPU 스케줄러는 가장 첫 번째 프로세스를 고르고 시간 할당량 이후 인터럽트를 위한 타이머를 설정한 뒤 프로세스를 디스패치한다.__   
      - 프로세스의 CPU 버스트가 1 time quantum 이하 경우   
        - 해당 프로세스는 자발적으로 CPU를 반환하고 CPU는 ready queue에 있는 다음 프로세스를 실행시킨다.   
      - 프로세스의 CPU 버스트가 1 time quantum보다 클 경우   
        - 타이머를 울려 OS에 인터럽트를 야기한다.   
        - 문맥교환이 일어나고 프로세스는 ready queue의 끝에 추가되며 ready queue에 있는 다음 프로세스를 실행시킨다.   
        
        <table align="center">
          <thead>
            <tr>
              <th>프로세스</th>
              <th>버스트 시간</th>
            </tr>
          </thead>
          <tbody>
            <tr>
              <td>P1</td>
              <td>24</td>
            </tr>
            <tr>
              <td>P2</td>
              <td>3</td>
            </tr>
            <tr>
              <td>P3</td>
              <td>3</td>
            </tr>
          </tbody>
        </table>   
        
        <p align="center"><img src="https://github.com/seongbeenkim/CS-Interview/blob/master/OS/image/5_RoundRobinChart.jpg"  title="5_RoundRobinChart" alt="5_RoundRobinChart"></img><br><strong> RR </strong><br> 평균 대기시간 : {(10-4) + (4) + (7)} / 3 = 5.66 </p>   
      
    - __평균 대기시간이 길다.__   
      - 각 프로세스는 자신의 다음 시간 할당까지 (n-1) * q(시간 할당량) 시간 이상 기다리지 않는다.   
      - __시간 할당량이 크면 FCFS와 같다.__   
      - __시간 할당량이 작으면 많은 문맥교환이 일어나 프로세스의 실행이 느려진다.__   
        - 문맥교환 시간이 시간 할당량의 약 10%일 경우 CPU의 10%는 문맥교환에 사용된다.   
        - 대부분의 현대 시스템은 시간 할당의 범위를 10 ~ 100ms로 설정한다.   
        - 문맥교환을 위한 시간을 10ms보다 작고 문맥교환 시간은 시간 할당량의 작은 일부이다.   
        
        <p align="center"><img src="https://github.com/seongbeenkim/CS-Interview/blob/master/OS/image/5_4_quantums.jpg"  title="5_4_quantums" alt="5_4_quantums"></img><br><strong>5.4 시간 할당량(quantum)에 따른 문맥 교환 증가 </strong><br></p>   
        
    - __반환 시간은 시간 할당량의 크기에 달려있다.__   
      - 평균 반환 시간은 시간 할당량의 크기가 증가한다고 향상될 필요는 없다.   
      <p align="center"><img src="https://github.com/seongbeenkim/CS-Interview/blob/master/OS/image/5_5_TurnaroundTime.jpg" height = 450px  title="5_5_TurnaroundTime" alt="5_5_TurnaroundTime"></img><br><strong>5.5 시간 할당량(quantum)에 따른 평균 반환시간의 변화 </strong><br></p>   
      - __단일 시간 할당량 시간보다 다음 CPU 버스트가 적을 경우에는 향상 될 수 있다.__   
      - 평균 반환 시간은 더 작은 시간 할당량을 가질 수록 더 증가한다.   
        - 더 많은 문맥교환이 일어나기 때문이다.   
        
    - __일반적으로 CPU 버스트의 80%는 시간 할당량보다 짧아야 한다.__   
    
#### 5.3.5 Multilevel Queue Scheduling(다단계 큐 스케줄링)   

  - __프로세스들이 다른 그룹으로 분류된 상황을 위한 스케줄링 알고리즘__   
    - 일반적인 분류는 foreground(interactive) 프로세스와 background(batch) 프로세스로 나뉜다.   
    - 두 종류의 프로세스는 다른 응답 시간을 요구하고 다른 스케줄링을 요구한다.   
    - __foreground 프로세스가 background 프로세스보다 높은 우선순위를 가질 수 있다.__   
    
  - __Multilevel Queue Scheduling__ :star:   
    - __ready queue를 여러 분리된 queue로 나눈다.__   
    - 프로세스는 프로세스 우선순위, 타입, 메모리 크기와 같은 프로세스의 일부 속성에 기반하여 하나의 queue 영구적으로 할당된다.   
    - __각 queue는 고유의 스케줄링 알고리즘을 가진다.__   
      - ex) foreground queue는 RR, background queue는 FCFS   
    - __queue 사이에도 스케줄링 알고리즘 필요하다.__   
      - 고정된 우선순위 선점 스케줄링   
        - ex ) 5가지 queue를 가진 다단계 큐 스케줄링 알고리즘의 예    
          <p align="center"><img src="https://github.com/seongbeenkim/CS-Interview/blob/master/OS/image/5_6_MultilevelQueueScheduling.jpg" height = 450px  title="5_6_MultilevelQueueScheduling" alt="5_6_MultilevelQueueScheduling"></img><br><strong>5.6 다단계 큐 스케줄링 </strong><br></p>   
        - 각각의 queue는 하위 우선수위 queue보다 절대적으로 높은 우선순위를 가진다.   
        
      - queue들 사이에 time slice를 할 수도 있다.   
        - 각 queue는 CPU 시간의 특정 양을 얻는다.   
          - ex) foreground queue는 RR 스케줄링을 위해 CPU 시간의 80%가 주어질 수 있고 background queue는 FCFS 스케줄링을 위해 20%를 얻을 수 있다.   
          
#### 5.3.6 Multilevel Feedback Queue Scheduling(다단계 피드백 큐 스케줄링)   

  - __다단계 큐는 프로세스가 시스템에 접속할 때, 하나의 queue에 영구적으로 할당되기 때문에 낮은 스케줄링 오버헤드를 가지지만 유연하지 못하다.__    
    - 즉, 프로세스가 다른 queue로 이동할 수 없다.   
    
  - __Multilevel Feedback Queue Scheduling__ :star:   
    - __queue사이에 이동이 가능하다.__    
      - __CPU 버스트의 특성에 따라서 프로세스를 분리하는 것이다.__   
      - 프로세스가 CPU 시간을 너무 많이 사용하면, 더 낮은 우선순위 큐로 이동된다.   
      - 입출력 중심과 대화형 프로세스가 높은 우선순위를 가진다.   
      - 프로세스가 낮은 우선순위 큐에서 너무 오랫동안 CPU를 기다리면 더 높은 우선순위 큐로 이동하게 된다.   
        - 기아 현상(Starvation) 예방   
        - ex) queue 0 : 시간 할당 8ms, queue 1 : 시간 할당 16ms, queue 3 : FCFS 있다고 가정할 경우   
          - queue 0에 있던 프로세스가 시간 할당량 8ms을 초과하면 queue 1의 끝에 추가된다.   
          - queue 0이 비고, queue 1에 있는 프로세스가 시간 할당량 16ms을 초과하면 queue 2의 끝에 추가된다.   
          - queue 0, 1이 비어있다면, queue 2에 있는 프로세스는 FCFS에 기반하여 실행된다.   
          <p align="center"><img src="https://github.com/seongbeenkim/CS-Interview/blob/master/OS/image/5_7_MultilevelFeedbackQueues.jpg" title="5_7_MultilevelFeedbackQueues" alt="5_7_MultilevelFeedbackQueues"></img><br><strong>5.7 다단계 피드백 큐</strong><br></p>   
          
          
      - __다단계 피드백 큐가 정의되는 파라미터__   
        - 큐의 수   
        - 각 큐를 위한 스케줄링 알고리즘   
        - 프로세스를 높은 우선순위 큐로 올려주는 시기를 결정하는 방법   
        - 프로세스를 낮은 우선순위 큐로 강등시키는 시기를 결정하는 방법   
        - 프로세스가 서비스를 필요로 할 때 프로세스가 들어갈 큐를 결정하는 방법   
        
      - __가장 일반적인 CPU 스케줄링 알고리즘이지만 모든 파라미터에 대한 값을 선택하기 때문에 가장 복잡한 알고리즘이다.__    

### 5.4 Thread Scheduling    

  - __커널 수준 스레드는 OS에 의해 스케줄링 된다.__   
  
  - __사용자 수준 스레드는 스레드 라이브러리에 의해 관리되고 커널의 개입이 없다.__   
  
  - __CPU에서 실행되기 위해서 사용자 수준 스레드는 관련된 커널 수준 스레드와 매핑어야만 한다.__   
    - 이러한 매핑이 간접적일 수도 있고 LWP를 사용할 수도 있다.   
    
#### 5.4.1 Contention Scope(경쟁 범위)   

  - __Process-Contention Scope(PCS)__   
    - __다대일과 다대다를 구현하는 시스템에서 스레드 라이브러리는 사용자 스레드를 이용가능한 LWP에서 실행시키기 위해 스케줄한다.__   
      - 스레드 라이브러리가 사용자 스레드를 이용가능한 LWP에 스케줄 한다는 것은 스레드가 실제로 CPU에서 실행되는 것을 의미하지 않는다.   
      - OS가 커널 스레드를 실제 CPU에 스케줄하는 것이 필요하다.   
    - __같은 프로세스에 속해있는 스레드 사이에 CPU를 얻기 위한 경쟁이 발생한다.__     
  
  - __System-Contention Scope(SCS)__   
    - __어느 커널 수준 스레드를 CPU에 스케줄할 지 결정하기 위해서 커널은 SCS를 사용한다.__   
    - __시스템에 있는 모든 스레드사이에 CPU를 얻기 위한 경쟁이 발생한다.__   
    - __일대일 모델을 사용하는 Windows, Linux, Solaris는 오직 SCS만 사용하여 스레드를 스케줄한다.__   
    
  - __PCS는 우선순위에 따라 수행된다.__   
    - 스케줄러는 가장 높은 우선순위를 가진 실행가능한 스레드를 선택한다.   
    - 사용자 수준 스레드 우선순위는 프로그래머에 의해 설정되고 스레드 라이브러리에 의해 조정되지 않는다.    
    - PCS는 실행되고 있는 스레드보다 높은 우선순위를 가진 스레드가 있다면 해당 스레드를 선점한다.   
      - 같은 우선순위를 가진 스레드 사이에 time slicing을 보장하지 않는다.   
      
#### 5.4.2 Pthread Scheduling   

  - __다대다 구현을 위해 PTHREAD_SCOPE_PROCESS 수법은 사용자 수준 스레드를 이용가능한 LWP에 스케줄한다.__   
    - LWP의 수는 스레드 라이브러리에 의해 유지된다.   
    
### 5.5 Multiple-Processor Scheduling    

  - __다수의 CPU가 이용가능하면 load sharing이 가능해진다.__   
    - ex) CPU가 2개 있을 경우   
      - load balancing은 작업이 2개의 CPU에 균등하게 분산된다.      
      - load sharing은 CPU 1개가 down되었을 때 해야할 작업을 다른 CPU에게 전달한다.   
    - 스케줄링은 더 복잡해진다.   
    
#### 5.5.1 Approaches to Multiple-Processor Scheduling(멀티프로세서 스케줄링 접근법)    

  - __멀티프로세서 시스템에서 CPU 스케줄링 접근법__ :star:   
    - __AMP(Asymmetric Multi-Processing)__   
      - __하나의 프로세서(메인 서버)에 의해 다른 시스템 활동, 입출력 처리, 모든 스케줄링 결정이 처리되게 하는 것이다.__    
        - 다른 프로세스는 오직 사용자 코드만 실행한다.   
      - __오직 하나의 프로세서만 시스템 자료구조에 접근할 수 있기 때문에 데이터 공유의 필요성을 감소시킨다.__   
      
    - __SMP(Symmetric Multi-Processing)__   
      - __각 프로세서가 각자 알아서 스케줄링한다.__   
      - __공동 ready queue에 모든 프로세스가 있거나 각각의 프로세서마다 자신 고유의 queue를 가질 수도 있다.__   
      - 각 프로세서가 ready queue를 검사하고 실행할 프로세스를 고르기 위한 스케줄러를 가짐으로써 스케줄링이 진행된다.   
      - 다수의 프로세서가 같은 자료구조에 접근하고 수정하지 않기 위해서 스케줄러는 반드시 세심하게 프로그램되어야만 한다.   
        - 별도의 프로세서가 같은 프로세스를 스케줄하지 않는 것과 프로세스들이 queue에서 사라지지 않는 것을 보장해야한다.   
        
#### 5.5.2 Processor Affinity(프로세서 친화성)       

  - __프로세스에 의해 접근된 가장 최근의 데이터는 프로세서를 위해 캐시로 이주된다.__ :star:   
    - 해당 프로세스에 의한 연속적인 메모리 접근은 종종 캐시 메모리에서 만족되어진다.   
    - 해당 프로세스가 다른 프로세서로 이전되면, 캐시 메모리의 내용은 첫 번째 프로세서에 대해서 무효화되고, 두 번째 프로세서를 위해 캐시는 재이주된다.   
    - 무효화하고 재이주시키는 높은 비용때문에 대부분 SMP는 프로세스를 하나의 프로세서에서 다른 프로세서로 이주하는 것을 피하려고 노력하며 같은 프로세서에서 계속 실행시키려고 시도한다.   
      - 프로세스는 현재 실행되고 있는 프로세서에 대한 친화성을 가진다.   
      
  - __Soft affinity__   
    - OS가 같은 프로세서에서 프로세스를 계속 실행시키려고 시도하지만 보장되지는 않는다.   
    - 프로세스가 프로세서 사이에서 이동되는 것이 가능하다.   
    
  - __Hard affinity__   
    - 프로세스가 실행될 수 있는 프로세서의 부분 집합을 명시하는 것이다.   
  
  - __대부분 시스템은 soft, hard 둘 다 제공한다.__   
    - ex) Linux   
  
  - __메인 메모리 구조는 프로세서 친화성에 영향을 줄 수 있다.__ :star:   
    - __NUMA(Non-Uniform Memory Access)__   
      - CPU가 다른 부분보다 메인 메모리의 일부에 더 빠르게 접근할 수 있다.   
      - CPU와 메모리가 결합된 보드를 포함하는 시스템에서 발생한다.   
      - 보드에 있는 CPU는 다른 보드에 있는 메모리에 접근하는 것보다 같은 보드에 있는 메모리에 더 빠르게 접근할 수 있다.   
      - OS의 CPU 스케줄러와 메모리 관리 알고리즘이 함께 동작한다면, 특정 CPU에 친화성이 할당된 프로세스는 CPU가 존재하는 보드에 있는 메모리에 할당될 수도 있다.   
      <p align="center"><img src="https://github.com/seongbeenkim/CS-Interview/blob/master/OS/image/5_9_NUMA_CPU_Scheduling.jpg" title="5_9_NUMA_CPU_Scheduling" alt="5_9_NUMA_CPU_Scheduling"></img><br><strong>5.9 NUMA와 CPU 스케줄링</strong><br></p>   
      
#### 5.5.3 Load Balancing(부하 분산)   

  - __SMP에서 하나 이상의 프로세스를 가지는 것의 이점을 최대한 활용하기 위해서 모든 프로세서 사이에 작업량을 균형되게 유지하는 것은 중요하다.__   
    - __부하 분산은 SMP 시스템내에서 모든 프로세서 사이의 작업량을 고르게 분산시켜 유지하도록 시도하는 것이다.__   
    - 각 프로세서가 자신의 private queue를 가지는 시스템에서만 필수적이다.    
      - 공동 실행 큐를 가지는 시스템에서는 하나의 프로세서가 idle 상태가 되면 즉시 공동 실행 큐로부터 실행가능한 프로세스를 추출하기 때문에 부하 분산이 필수적이지는 않다.   
      - SMP를 지원하는 대부분 현대 OS에서 각 프로세서는 적합한 private queue를 가진다.   
    
  - __부하 분산 방법 2가지__ :star:   
    - __Push migration__   
      - __특정 작업이 각 프로세서에 있는 작업량을 주기적으로 확인한다.__   
        - 불균형을 발견한다면, 공정하게 작업량을 덜 바쁘거나 idle인 프로세서로 프로세스를 push함으로써 분산시킨다.   

    - __Pull migration__   
      - __idle 프로세서가 바쁜 프로세서를 기다리고 있는 작업을 pull한다.__   
    
    - __push와 pull migration은 상호적으로 배제될 필요가 없고 사실 부하 분산 시스템에서 병렬로 구현되기도 한다.__   
    
  - __부하 분산이 프로세서 친화성의 이점을 상쇄시킬때도 있다.__   
    - 같은 프로세서에서 실행되는 프로세스를 유지하는 것의 이점은 프로세스가 해당 프로세서의 캐시 메모리에 있는 데이터를 이용할 수 있다는 것이다.   
    - pulling 또는 pushing으로 하나의 프로세스를 다른 프로세서로 옮기는 것은 위의 이점을 제거한다.   
    
#### 5.5.4 Multicore Processor(멀티코어 프로세서)   

  - __SMP 시스템은 다수의 물리적인 프로세서를 제공함으로써 다수의 스레드를 동시에 실행시킬 수 있다.__   
  
  - __현대 컴퓨터 하드웨어에서는 같은 물리직 칩에 다수의 프로세서 코어를 두어 멀티코어 프로세서가 된다.__   
    - 각 코어는 자신의 아키텍쳐 상태를 관리하고 별도의 물리적 프로세서로 OS 표시된다.   
    
  - __멀티코어 프로세서를 사용하는 SMP 시스템은 더 빠르고 각 프로세서가 고유의 물리접 칩을 가진 시스템보다 파워를 덜 소비한다.__   
  
  - __Memory stall__ :star:   
    - __프로세서가 메모리에 접근할 때 데이터가 이용가능할 때까지 상당히 큰 시간 동안 기다리는 것을 의미한다.__   
      - ex) cache miss(접근하는 데이터가 캐시 메모리에 없다)와 같은 여러 이유로 인해 Memory stall이 발생할 수 있다.   
      
    <p align="center"><img src="https://github.com/seongbeenkim/CS-Interview/blob/master/OS/image/5_10_MemoryStall.jpg" title="5_10_MemoryStall" alt="5_10_MemoryStall"></img><br><strong>5.10 Memory stall</strong><br></p>    
      - 프로세서는 데이터가 메모리로부터 이용가능할 때까지 자신의 시간의 50%를 기다리는 데 소비할 수 있다.   
        - 이러한 상황을 해결하기 위해서, 두 개 이상의 하드웨어 스레드가 각 코어에 할당되는 멀티스레드 프로세서 코어를 구현한 하드웨어가 필요하다.         
      - 하나의 스레드가 메모리를 기다리는 동안 stall하면, 코어는 다른 스레드로 전환한다.   
      <p align="center"><img src="https://github.com/seongbeenkim/CS-Interview/blob/master/OS/image/5_11_MultithreadedMulticore.jpg" title="5_11_MultithreadedMulticore" alt="5_11_MultithreadedMulticore"></img><br><strong>5.11 멀티스레드 멀티코어 시스템</strong><br></p>   
      
  - __OS의 관점에서 각 하드웨어 스레드는 소프트웨어 스레드를 실행하는데 이용가능한 논리적 프로세서이다.__   
    - ex) dual thread, dual core 시스템에서 4개의 논리적인 프로세서가 OS에 표시된다.   
    - ex) UltraSPARK T3 CPU는 하나의 칩에 16개의 코어를 가지고 코어당 8개의 하드웨어 스레드를 가지기 때문에 128개의 논리적인 프로세서가 OS에 표시된다.   
    
  - __멀티스레드 프로세싱 코어 2가지 방법__ :star:   
    - __Coarse-grained 멀티스레딩__   
      - Memory stall 발생과 같은 긴 대기 시간 event이 발생할 때 까지 스레드가 프로세서에서 실행된다.   
        - 긴 대기 시간 event에 의한 delay 때문에, 프로세서는 실행을 시작할 다른 스레드로 전환해야만 한다.   
        - 프로세서 코어에서 다른 스레드의 실행을 시작하기전에 명령어 파이프라인이 반드시 비워져야하기 때문에 스레드 사이의 문맥교환 비용이 많이 든다.   
        
    - __Fine-grained 멀티스레딩__   
      - 더 작은 단위에서 스레드 사이에 전환이 일어난다.   
        - 일반적으로 명령어 사이클의 경계   
      - fine-grained 구조는 스레드 전환을 위한 logic을 포함한다.   
        - 스레드 사이의 문맥교환 비용이 적게 든다.   
    
  - __멀티스레드 멀티코어 프로세서에서 필요한 2단계의 스케줄링__   
    - __1. 각 하드웨어 스레드에서 실행할 소프트웨어 스레드를 선택하는 것처럼 스케줄링 결정은 OS에 의해 결정되어야만 한다.__   
      - 이러한 스케줄링 단계를 위해서 OS는 어떠한 스케줄링 알고리즘도 선택할 수 있다.   
      
    - __2. 각 코어가 어느 하드웨어 스레드를 실행할 건지 결정하는 방법을 명시한다.__   
      - ex) Ultra SPARC T3는 RR을 사용하여 각 코어에서 8개의 하드웨어 스레드를 스케줄한다.   
       
### 5.6 Real-time CPU Scheduling   

  - __Soft real-time systems__   
    - 중요한 실시간 프로세스가 스케줄 되는 것을 보장하지 않는다.   
    - 오직 중요한 프로세스가 중요하지 않은 프로세스보다 선호되는 것만 보장한다.   
  
  - __Hard real-time systems__   
    - 작업은 deadline안에 완료되야만 한다.   
    - deadline이 지난 서비스는 서비스가 아예 존재하지 않는 것과 같다.   
    
#### 5.6.1 Minimizing Latency(지연 최소화)    

  - __실시간 시스템은 실시간으로 event가 발생하기를 기다린다.__   
    - event는 타이머가 만료될 때 소프트웨어에서 일어나거나 원격조종 차량이 장애물에 접근하는 것을 감지했을 때처럼 하드웨어에서 일어난다.   
    - 시스템은 event 발생 시 최대한 빠르게 반응하고 service해야한다.   
    
  - __Event latency__   
    - event가 발생한 시점부터 서비스 될 때까지 경과한 시간    
    <p align="center"><img src="https://github.com/seongbeenkim/CS-Interview/blob/master/OS/image/5_12_EventLatency.jpg" title="5_12_EventLatency" alt="5_12_EventLatency"></img><br><strong>5.12 Event latency</strong><br></p>   
    
  - __실시간 시스템의 성능에 영향을 미치는 2가지 지연__ :star:    
    - __1. 인터럽트 지연(Interrupt latency)__   
      - __CPU에 인터럽트 도착부터 인터럽트 서비스 루틴 시작까지 걸리는 시간__   
        - 인터럽트 발생 시, OS는 반드시 실행하고 있는 명령어를 완료하고 발생한 인터럽트의 종류를 확인해야한다.   
        - 현재 프로세스의 상태를 특정 ISR을 사용하여 인터럽트 서비스 하기 전에 저장해야한다.   
        - 이렇나 작업을 수행하기 위해 필요한 총 시간을 인터럽트 지연이라고 한다.   
      - __실시간 OS에서 실시간 작업이 즉시 처리되는 것을 보장하기 위해서 인터럽트 지연을 최소화 하는 것은 중요하다.__   
        - Hard real-time system에서는 엄격한 규칙도 충족해야만 한다.    
      - __인터럽트 지연에 기여하는 중요한 한 요소는 커널 자료구조가 갱신되고 있는 동안 인터럽트가 disable될 수 있는 시간의 양이다.__   
        - 실시간 OS는 아주 짧은 시간동안 인터럽트를 disable 해야한다.   
        <p align="center"><img src="https://github.com/seongbeenkim/CS-Interview/blob/master/OS/image/5_13_InterruptLatency.jpg" height = 450px title="5_13_InterruptLatency" alt="5_13_InterruptLatency"></img><br><strong>5.13 인터럽트 지연</strong><br></p>   

    - __2. 디스패처 지연(Dispatcher latency)__    
      - __디스패처가 하나의 프로세스를 중단하고 다른 프로세스를 시작하는데 걸리는 시간__   
        - 실시간 작업에게 CPU 즉시 접근을 제공하는 것은 실시간 OS가 디스패처 지연을 최소화 시킬수 있게 한다.    
      - __디스패처 지연을 낮게 유지하는 최선의 방법은 선점적인 커널을 제공하는 것이다.__   
      - __디스패처 지연의 conflict phase는 2가지 요소를 가진다.__   
        - __1. 커널에서 실행되는 프로세스를 선점__   
        - __2. 높은 우선순위 프로세스에 의해 요구되는 자원을 낮은 우선순위 프로세스가 해제__   
          - ex) Solaris에서 선점이 disable 됐을 경우 디스패치 지연은 100ms 이상, enable 됐을 경우 디스패치 지연은 1ms 이하   
        <p align="center"><img src="https://github.com/seongbeenkim/CS-Interview/blob/master/OS/image/5_14_DsipatchLatency.jpg" height = 450px title="5_14_DsipatchLatency" alt="5_14_DsipatchLatency"></img><br><strong>5.14 디스패치 지연</strong><br></p>   
          
#### 5.6.2 Priority-Based Scheduling(우선순위-기반 스케줄링)   

  - __실시간 OS를 위한 스케줄러는 반드시 선점이 가능한 우선순위 기반 알고리즘을 지원해야한다.__   
    - 실시간 시스템은 실시간 프로세스에 가장 높은 스케줄링 우선순위를 할당한다.   
    - 오직 Soft real-time system 기능만 보장한다.   
      - Hard real-time system은 deadline안에 실시간 작업들이 완료되어야 한다는 것을 보장하기 때문에, 이러한 보장을 하기 위한 추가적인 스케줄링 특성이 필요하다.   
      
  - __각 스케줄러의 세부사항을 알아보기 전에 스케줄 되기 위한 프로세스들의 특징들을 정의해야만 한다.__   
    - __프로세스가 주기적(periodic)이라고 고려된다.__   
      - 프로세스는 상수 간격마다 CPU를 요구한다.   
      - 주기적인 프로세스가 CPU를 얻으면 고정된 프로세싱 시간 : t, deadline : d, period : p를 가지게 된다.   
      - t, d, p의 관계는 0 <= t <= d <= p 로 표현된다.   
      - 주기적인 작업률은 1/p이다.   
      - 스케줄러는 이러한 특성을 이용할 수 있고 프로세스 deadline 또는 요구되는 프로세스 rate에 따라 우선순위를 할당한다.   
      
      <p align="center"><img src="https://github.com/seongbeenkim/CS-Interview/blob/master/OS/image/5_15_PeriodicTask.jpg" title="5_15_PeriodicTask" alt="5_15_PeriodicTask"></img><br><strong>5.15 주기적 프로세스</strong><br></p>   
      
  - __이러한 스케줄링 형식의 특이한 것은 프로세스가 스케줄러에게 자신의 deadline을 알려야만 할 수도 있다는 것이다.__   
    - 이러한 기술을 사용하는 것은 admission-control 알고리즘이라고 한다.   
      - 제 시간에 프로세스를 완료할 것을 보장하는 프로세스를 받아들이거나 deadline안에 서비스 되는 것을 보장하지 못하면 요청을 거절한다.   
      
#### 5.6.3 Rate-Monotonic Scheduling(비율-단조 스케줄링)   

  - __정적 우선순위 선점방식을 사용하여 주기적인 작업을 스케줄한다.__   
    - 낮은 우선순위 프로세스가 실행되고 있는데 높은 우선순위 프로세스를 실행할 수 있게 되면 높은 우선순위 프로세스가 선점한다.   
    - __시스템에 접속하는 시점에 각 주기적인 작업은 자신의 주기에 기반하여 역으로 우선순위가 할당된다.__   
      - 주기가 짧을수록, 더 높은 우선순위를 가지게 된다.   
      - CPU를 더 자주 필요로 하는 작업에 더 높은 우선순위를 할당하는 것이 뒷받침되어있다.   
    - __주기적인 프로세스의 처리 시간은 각 CPU 버스트와 같다.__   
      - 프로세스가 CPU를 얻을 때마다, CPU 버스트의 주기는 같다.   
        - ex) P1 = 50, P2 = 100, T1= 20, T2 = 35일 경우   P: 프로세스 주기, T: 프로세스 처리 시간   
          - 각 프로세스의 CPU 이용률은 T1/P1 = 0.4, T2,P2 = 0.35로 총 CPU 이용률은 0.4 + 0.35 = 0.75   
          - 두 프로세스 모두 자신의 deadline을 충족하고 CPU를 이용가능한 cycle에 놓는 방식에서 이러한 작업을 스케줄 할 수 있다.   
        <p align="center"><img src="https://github.com/seongbeenkim/CS-Interview/blob/master/OS/image/5_16_SchedulingTasks.jpg" title="5_16_SchedulingTasks" alt="5_16_SchedulingTasks"></img><br><strong>5.16 P2가 P1보다 높은 우선순위를 가질 경우</strong><br></p>     
           
          - P2가 35에 완료되고 P1이 55에 완료될 것 같지만 P1의 첫 번째 deadline이 50이었기 때문에 스케줄러는 P1이 자신의 deadline을 놓쳤다고 생각한다.    
          
        - Rate-Monotonic Scheduling을 사용하면 P1의 주기가 더 짧기 때문에 P2보다 더 높은 우선순위를 할당한다.   
          <p align="center"><img src="https://github.com/seongbeenkim/CS-Interview/blob/master/OS/image/5_17_RateMonotonicScheduling.jpg" title="5_17_RateMonotonicScheduling" alt="5_17_RateMonotonicScheduling"></img><br><strong>5.17 비율-단조 스케줄링</strong><br></p>     
          
            - P1은 먼저 시작하고 자신의 CPU 버스트를 20에 완료한다 그러므로 자신의 deadline을 충족시킨다.   
            - P2는 20에 시작하고 50까지 실행한다.   
              - P2의 CPU 버스트가 5 남았음에도 불구하고 50에 P1이 선점한다.   
            - P1은 70에 자신의 CPU 버스트를 완료한다.   
              - 스케줄러는 이 시점에 P2를 재개한다.   
            - P2는 자신의 첫 번째 deadline을 만족시키면서 75에 자신의 CPU 버스트를 완료한다.   
            - 시스템은 100까지 idle이 되고 P1이 다시 스케줄된다.   
            
    - __Rate-Monotonic Scheduling는 프로세스의 집합이 정적 우선순위를 할당하는 다른 알고리즘에 의해 스케줄 될 수 없을 때 최적으로 고려된다.__   
    
    - __문제점__   
      - CPU 이용률에 한계가 있고 CPU 자원을 항상 최대로 사용하지 못한다.   
        - 최악의 경우 CPU 이용률은 N * (2^(1/N)-1)이다.   
        - 시스템에 하나의 프로세스가 있다면 CPU 이용률은 100% 이지만 프로세스의 수가 무한대에 접근할 경우 69%까지 떨어질 수 있다.   
        - 두 개의 프로세스일 경우 CPU 이용률은 약 83%로 제한되어 있다.      
          - 5.16, 5.17에서 두 프로세스의 결합된 CPU 이용률 75%이다.   
            - Rate-Monotonic Scheduling은 프로세스들이 자신의 deadline을 충족시키게 하기 위해서 프로세스를 스케줄 하는 것이 보장된다.   
          - 5.18에서 CPU 이용률은 약 94%이다.
            - 그러므로 Rate-Monotonic Scheduling은 프로세스들이 자신의 deadline을 충족시키게 하기 위해서 자신들이 스케줄 될 수 있다는 것을 보장할 수 없다.   
            
#### 5.6.4 Earliest-Deadline-First Scheduling(최단 마감 우선 스케줄링)   

  - __EDF는 동적으로 deadline에 따라 우선순위를 할당한다.__   
    - deadline이 가까울수록 높은 우선순위를 가진다.   
    - 프로세스가 실행가능한 상태가 될 때, 시스템에 deadline 사항을 알려야만 한다.   
    - 우선순위는 새롭게 실행가능한 프로세스의 deadline을 반영하여 조정될 수 있다.   
    - 우선순위가 고정된 비율 단조 스케줄링과 다르다.   
      <p align="center"><img src="https://github.com/seongbeenkim/CS-Interview/blob/master/OS/image/5_18_MissingDeadlines.jpg" title="5_18_MissingDeadlines" alt="5_18_MissingDeadlines"></img><br><strong>5.18 비율-단조 스케줄링시 deadline 놓침</strong><br></p>     
      <p align="center"><img src="https://github.com/seongbeenkim/CS-Interview/blob/master/OS/image/5_19_EarliestDeadlineFirstScheduling.jpg" title="5_19_EarliestDeadlineFirstScheduling" alt="5_19_EarliestDeadlineFirstScheduling"></img><br><strong>5.19 최단 마감 우선 스케줄링</strong><br></p>     

        - P1이 가장 가까운 deadline을 가진다 그러므로 가장 높은 우선순위가 할당된다.   
        - P2는 P1의 CPU 버스트가 끝나는 시점에 실행을 시작한다.   
          - 비율 단조 스케줄링은 P1이 P2를 50에 선점할 수 있게 하였지만 EDF는 P2가 계속해서 실행할 수 있게 한다.   
          - P2는 다음 deadline이 80이고 P1은 100이기 때문에 P1보다 더 높은 우선순위를 가지게 된다.   
          - P1, P2 둘 다 자신의 첫 번째 deadline을 충족한다.   
        - P1이 60에 실행을 시작하고 자신의 두 번째 CPU 버스트가 85에 완료되고 두 번째 deadline 100을 충족시킨다.   
        - P2는 85에 실행을 시작하고 100에 P1에게 선점당한다.   
          - 100에 P1의 deadline이 150, P2의 deadline이 160이기 때문에 P1의 우선순위가 더 높아지게 된다.   
        - P1은 100에 시작하여 125에 자신의 CPU 버스트를 완료한다.   
        - P2는 125에 실행을 재개하고 145에 완료한다.   
        - 시스템은 150까지 idle 상태로 있고 P1이 다시 스케줄된다.   
        
  - __프로세스가 주기적이거나 프로세스가 버스트당 상수의 CPU 시간을 요구할 필요가 없다.__   
    - 오직 필요한 요건사항은 프로세스가 실행가능상태가 될 때, 스케줄러에 자신의 deadline을 알리는 것이다.   
  
  - __이론적으로는 최적이다.__   
    - 이론적으로는 각 프로세스가 자신의 deadline을 충족하기 위해서 프로세스를 스케줄할 수 있고 CPU 이용률이 100% 될 것이다.   
    - 하지만 실제로는 이러한 수준의 CPU 이용을 달성하는 것은 인터럽트 처리와 프로세스 사이의 문맥교환의 비용때문에 불가능하다.   
    
#### 5.6.5 Proportional Share Scheduling   

  - __모든 app 사이에 T개의 몫를 할당함으로써 작동한다.__   
    - app은 N개의 몫를 받을 수 있다.   
      - 총 프로세서 시간의 N/T를 가지는 것을 보장한다.   
        - ex) T = 100개의 몫이 3개의 A, B, C 프로세스에 나뉘고 A = 50, B = 15, C = 20를 할당받는다면   
          - A는 전체 프로세서 시간의 50%, B는 15%, C는 20%를 가질 것 이다.   
    
  - __app은 할당된 시간의 몫을 얻는 것을 보장하기 위해서 admission-control과 결합되어 동작해야한다.__   
    - admission-control은 충분한 몫이 이용가능할 경우에만 특정 수의 몫을 요청하는 Client를 허용한다.   
      - ex) 100개의 몫 중 50 + 15 + 20 = 85개의 몫이 할당되어 있고 D가 몫 30을 요청하면 D의 시스템 접근을 거부한다.    

#### 5.6.6 POSIX Real-Time Scheduling(POSIX 실시간 스케줄링)    

  - __실시간 스레드를 위한 RR, FIFO 두 가지 스케줄링이 있다.__   
  
### 5.7 Operation System Examples   
  
  - __Linux, Windows, Solaris 스케줄링의 예가 있지만 자세히 다루지 않고 넘어감..__   
  
### 5.8 Algorithm Evaluation   

  - __알고리즘을 선택하기 위해서 반드시 각 요소들의 상대적인 중요성을 먼저 정의해야 한다.__   
    - 상대적인 중요성을 정의하는데 있어서의 이러한 아래의 기준이 포함된다.   
      - __1. 최대 반응 시간이 1초인 제한아래 최대 CPU 이용률__    
      - __2. 반환 시간이 선형적으로 전체 실행시간에 비례할 때 최대 처리량__   

#### 5.8.1 Deterministic Modeling(결정론적 모델링)   

  - __분석적 평가(Analytic evaluation)__   
    - 시스템 작업량과 주어진 알고리즘을 사용하여 해당 작업량에 대한 알고리즘의 성능을 평가할 수 있는 숫자 또는 공식을 만들어낸다.   
  
  - __결정론적 모델링(Deterministic Modeling)__   
    - 분석적 평가의 유형이다.   
    - 미리 정해진 특정 작업량을 가지고 그 작업량에 대한 각 알고리즘의 성능을 정의한다.   
      - ex) 특정 프로세스들과 FCFS, SJF, RR을 사용하여 각 알고리즘의 성능을 알아본다.   
    - 간단하고 빠르며 정확한 숫자를 주어 알고리즘을 비교할 수 있게 한다.   
      - 입력으로 정확한 숫자가 필요하고 결과는 해당 케이스에 대해서만 적용된다.   
      
#### 5.8.2 Queueing Models(큐잉 모델)   
  
  - __많은 시스템에서 프로세스는 그날그날 다르게 실행되기 때문에 결정론적 모델링을 위해서 사용되는 정적인 프로세스는 없다.__   
    - 그러나 CPU 와 입출력 버스트의 분포는 확인할 수 있다.   
    - 이러한 분포는 측정될 수 있고 쉽게 평가될 수 있다.   
      - 이러한 분포 측정의 결과는 특정 CPU 버스트의 확률을 나타내는 수학 공식이다.  
    - 프로세스가 시스템에 도착할 때, 시간의 분포(도착 시간 분포)를 묘사할 수 있다.   
    - 위의 두 분포로부터, 대부분의 알고리즘을 위한 평균 대기 시간, CPU 이용률, 처리량을 연산하는 것이 가능하다.   
  
  - __컴퓨터 시스템은 서버의 네트워크로 묘사된다.__   
      - 각 서버는 기다리는 프로세스들의 큐를 가지고 있다.   
      - 입출력 시스템이 장치 큐를 가진것 처럼 CPU는 ready queue를 가진 서버다.   
      - __Queueing-network analysis__   
        - 도착 속도와 서비스 속도를 알고 있으면, 이용률, 평균 큐 길이, 평균 대기 시간 등을 알 수 있다.   
        
#### 5.8.3 Simulations(시뮬레이션)   

  - __시뮬레이터는 시계를 나타내는 변수를 가진다.__   
    - 변수의 값이 커지면, 시뮬레이터는 스케줄러, 프로세스, 장치의 활동을 반영하기 위해 시스템의 상태를 수정한다.   
    
  - __시뮬레이션이 실행되면 알고리즘 성능을 나타내는 통계치가 합쳐지고 출력된다.__   
  
  - __시뮬레이션을 주도하는 데이터는 여러 방식에서 생성될 수 있다.__   
    - 가장 흔한 방법은 확률분포에 따라 출발지, 목적지, CPU 버스트 시간, 프로세스를 생성하기 위해 프로그램된 난수 생성기를 사용한다.   
    - 분포는 수학적으로 또는 경험적으로 정의될 수 있다.   
      - 분포가 경험적으로 정의되어지기 위해서는, 연구 아래 실제 시스템의 측정들이 취해진다.   
        - 결과는 실제 시스템에서의 사건들의 분포를 정의하고 이러한 분포는 시뮬레이션을 주도하는데 사용될 수 있다.   
        
  - __분포 주도 시뮬레이션은 실제 시스템에서 연속적인 사건 사이의 관계 때문에 부정확할 수 있다.__   
    - 빈도 분포는 각 사건의 인스턴스가 얼마나 많이 발생하는지만 나타내고 발생의 순서에 관해서는 나타내지 않는다.
    - 이러한 문제를 바로잡기 위해서, trace tape을 사용할 수 있다.   
      - 실제 시스템을 모니터하고 실제 연속적인 사건을 기록함으로써 trace tape를 생성한다.   
      <p align="center"><img src="https://github.com/seongbeenkim/CS-Interview/blob/master/OS/image/5_25_SchedulerSimulation.jpg" height = 400px title="5_25_SchedulerSimulation" alt="5_25_SchedulerSimulation"></img><br><strong>5.25 시뮬레이션에 의한 CPU 스케줄러 평가</strong><br></p>     
      
      - trace tape은 실제 입력의 정확히 같은 집합에 기반한 두 알고리즘을 비교할 수 있는 좋은 방법을 제공한다.  
      - 입력에 대한 정확한 결과를 만들어 낼 수 있다.   
  
  - __컴퓨터 시간을 더 요구하여 비용이 비쌀 수 있다.__   
    - 더 디테일한 시뮬레이션은 더 정교한 결과를 제공하지만 더 많은 컴퓨터 시간을 소비한다.   
    - trace tape는 저장 공간의 거대한 양이 필요할 수 있다.   

#### 5.8.4 Implementation(구현)   

  - __스케줄링 알고리즘을 평가하기 위한 가장 완벽하고 정확한 방법은 코드를 짜서 OS에 넣는 것이다.__   
  
  - __이러한 방법의 문제점__
    - __알고리즘을 코딩하고 알고리즘을 지원하기 위해 OS를 수정하는 것 뿐만 아니라 계속해서 변경되는 OS에 사용자의 반응을 봐야하기 때문에 비용이 많이 든다.__   
      - 대다수의 사용자는 더 나은 OS를 만드는데 관심이 없고 단지 자신의 프로세스가 실행되고 결과를 사용하기를 원한다.   
        - 계속 변경되는 OS는 사용자가 작업을 완료하는데 도움을 주지 않는다.      

    - __알고리즘이 사용되는 환경이 변경되어 새로운 프로그램이 작성되고 문제의 유형이 변경될 뿐만 아니라 스케줄러의 성능의 결과도 변경된다.__     

  - __가장 유연한 스케줄링 알고리즘은 특정 app을 위해 조정될 수 있게 사용자나 시스템 관리자에 의해 변경될 수 있는 알고리즘이다.__   
    - ex) Workstation, Solaris   
    
    - __스레드나 프로세스의 우선순위를 조정할 수 있는 API를 사용하는 것도 또 다른 방법이 될 수 있다.__   
      - ex) Java, POSIX, Windows API   
      - 그러나 이러한 방법은 대부분의 상황에서 향상된 성능을 가져오지 않기 때문에 몰락했다.   
