# :bookmark_tabs: Operating System Concepts 9th edition      
## 3장 Process Concept   
__중요하다고 생각되거나 알고 있으면 좋을 것 같다는 내용이 있던 챕터에는 :star: 표시를 해놓았고    
해당 챕터안에 중요 개념 옆에 :star: 표시 해놓습니다.   
혹시 잘못된 내용이 있거나 보완해야할 점이 있으면 `issue` 해주시거나 알려주시면 감사하겠습니다.:bow:__   

* [3.Process Concept](#3process-concept)   
   - [3.1 Process Concept](#31-process-concept) :star:   
      - [3.1.1 The Process(프로세스)](#311-the-process프로세스) :star:
      - [3.1.2 Process State(프로세스 상태)](#312-process-state프로세스-상태) :star:
      - [3.1.3 Process Control Block(프로세스 제어 블록)](#313-process-control-block프로세스-제어-블록) :star:   
      - [3.1.4 Thread(스레드)](#314-thread스레드)   
   - [3.2 Process Scheduling](#32-process-scheduling) :star:
      - [3.2.1 Scheduling Queues(스케줄링 큐)](#321-scheduling-queues스케줄링-큐) :star:   
      - [3.2.2 Schedulers(스케줄러)](#322-schedulers스케줄러)   
      - [](#)   
      - [](#)   
      - [](#)   
      - [](#)   
      - [](#)   
      - [](#)   
      - [](#)   
      - [](#)   
      - [](#)   
      
      
## 3.Process Concept   
   
   - __Process__ :star:   
      - __현대 시분할 시스템에서 작업의 단위__   
      - __실행중인 프로그램__   
      
   - __시스템은 프로세스의 집합으로 구성__    
      - __OS 프로세스__   
         - 시스템 코드 실행   
      - __사용자 프로세스__   
         - 사용자 코드 실행    
         
### 3.1 Process Concept   

   - __batch 시스템__   
      - job을 실행시킨다.   
         - 해당 챕터에서 job은 process와 거의 같은 용어로 사용한다.   
         - 대부분의 OS 이론과 용어들은 OS의 주요 activity가 job processing일때 발전되었다.    
         - process가 job이라는 단어를 대체하기 때문에 일반적으로 받아들여지는 job scheduling 같은 단어 job을 포함하는 용어들의 사용을 피하는 것이 오해를 불러 일으킬 수도 있다.    
      
   - __시분할 시스템__   
      - 사용자 프로그램 또는 task를 가진다.   
      
#### 3.1.1 The Process(프로세스)   

   - __Process는 text 영역으로 알려진 program code뿐만 아니라 processor 레지스터의 내용과 program counter의 값에 의해 보여지는 현재 activity를 포함한다.__   
   
   - __Process in 메모리__ :star:   
      - __Stack__   
         - 함수 매개변수, 반환 주소, 지역 변수 같은 일시적 데이터   
      - __Data__   
         - 전역 변수   
      - __Heap__    
         - Process 실행 동안 동적으로 할당된 메모리   
      - __Text__   
         - Program code   
      #### 프로세스 메모리 이미지   
      
   - __Program = 수동적 개체__   
      - ex) 디스크에 저장된 명령어 목록을 가진 파일 (= 실행가능한 파일)   
   
   - __Process = 능동적 개체__   
      - ex) 실행할 다음 명령어를 명시하는 Program counter를 가지고 있으며 관련된 자원의 집합을 포함한다.    
      
   - __실행가능한 파일이 메모리로 적재될 때 Program은 Process가 된다.__ :star:   
      - 실행 시키는 방법   
         - 아이콘 더블 클릭   
         - 명령어 라인에서 실행가능한 파일 이름 입력, 엔터   
         
   - __두 개의 Process가 같은 Program과 관련이 있더라도, 두 개의 분리된 실행으로 고려된다.__ :star:   
      - ex) 다수의 사용자가 하나의 메일 프로그램을 여러 개 실행 또는 한 사용자가 하나의 웹브라우저 프로그램을 여러 개 실행할 경우, 각각의 분리된 Process이다.   
      - __Text 영역은 같지만 Stack, Data, Heap 영역은 다르다__   
      
   - __프로세스 자체가 다른 코드를 위한 실행 환경이 될 수 있다.__   
      - ex) Java 프로그래밍 환경   
         - 실행가능한 Java 프로그램은 JVM 내에서 실행된다.   
         - JVM은 적재된 Java 코드를 해석하고 그 코드를 위해서 기계 언어를 통해 그 코드를 실행하는 프로세스로서 실행된다.   
         
#### 3.1.2 Process State(프로세스 상태)   

   - __Process 상태__ :star:   
      - __New__   
         - Process가 생성된 상태   
      - __Running__   
         - 명령어가 실행중인 상태   
      - __Waiting__   
         - Process가 일부 event를 위해 입출력 완료 또는 신호를 기다림과 같은 어떤 사건이 발생하기를 기다리는 상태
      - __Ready__   
         - Processor에 할당되기를 기다리는 상태   
      - __Terminated__   
         - Process의 실행이 종료된 상태   
      
      #### 프로세스 상태 이미지   
      
#### 3.1.3 Process Control Block(프로세스 제어 블록)   

   - __각 Process는 PCB에 의해서 OS안에 표현된다.__   
      - PCB를 Task Control Block이라고도 한다.   
      
      #### PCB 이미지   
      
   - __PCB가 가진 정보__ :star:   
      - __프로세스 상태(Process state)__   
         - New, Ready, Running, Waiting(Halted)상태 등이 있다.   
      - __프로그램 카운터(Program counter)__   
         - 프로세스를 위해 실행되어야 할 다음 명령어의 주소를 가리킨다.   
      - __CPU 레지스터(CPU register)__   
         - 컴퓨터 구조에 따라 종류와 수가 다르다.   
         - 누산기(accumulator), 색인 레지스터(index register), 스택 레지스터(stack register), 범용 레지스터(general-purpose register), 상태 코드 정보(condition-code information)를 포함한다.   
         - Program Counter와 함께 상태 정보는 나중에 프로세스가 계속 올바르게 수행되도록 하기 위해서 인터럽트 발생시마다 반드시 저장되어야 한다.   
      - __CPU-스케줄링 정보(CPU-scheduling information)__   
         - 프로세스 우선순위, 스케줄링 큐를 위한 포인터, 다른 스케줄링 파라미터를 포함한다.   
      - __메모리-관리 정보(Memory-management information)__   
         - OS에 의해 사용되는 메모리 시스템에 따라 Segment table 또는 Page table과 base register, limit register의 값 같은 항목을 포함한다.   
      - __계산 정보(Accounting information)__   
         - CPU의 수, 사용된 시간, 시간 제한, 계정 정보, 작업(job) 또는 프로세스 번호 등을 포함한다.   
      - __입출력 상태 정보(I/O status information)__   
         - 프로세스에 할당된 일출력 장치들과 열린 파일의 목록 등을 포함   
      
      #### CPU 스위칭 이미지    

#### 3.1.4 Thread(스레드)   
   
   - __단일 스레드는 프로세스가 한번에 하나의 작업만 수행할 수 있게 한다.__   
   
   - __현대 OS는 프로세스가 다수의 스레드를 가질 수 있게 하고 한번에 하나 이상의 작업을 수행할 수 있게 한다.__   
      - 멀티 코어 시스템에서 효율적이다.   
      
   - __이러한 다수의 스레드를 지원하기 위해 시스템에서 PCB는 각 스레드를 위한 정보도 포함한다.__ :star:   
   
### 3.2 Process Scheduling   

   - __멀티 프로그래밍의 목적__   
      - 항상 어떤 프로세스가 실행되게하여 CPU의 사용을 최대화하는데 있다.   
      
   - __시분할의 목적__   
      - Process들 사이에 빈번하게 CPU를 변경해주어 각 프로그램이 실행되는 동안 사용자가 각 프로그램과 교류할 수 있게 하기 위해서이다.   
      
   - __이러한 목적을 달성하기 위해서 Process Scheduler가 존재하며, Process Scheduler는 CPU에서 프로그램 실행을 시키기 위해 이용가능한 프로세스들 중 하나의 프로세스를 선택한다.__   
      - 단일 처리기 시스템의 경우, 실행 중인 프로세스가 한 개 이상 있을 수 없다. 만약 단일 처리기에 여러 프로세스들이 있다면 프로세스들은 CPU가 작업이 끝날 때까지 대기하여야 한다.   
      
#### 3.2.1 Scheduling Queues(스케줄링 큐)   
   
   - __스케줄링 큐의 종류__ :star:   
      - __작업 큐(Job queue)__  
         - 프로세스가 시스템에 들어오면 작업 큐에 놓여 진다.   
         - 시스템에 있는 모든 프로세스로 구성된다.   

      - __준비 완료 큐(ready queue)__   
         - 메인 메모리에 있고 준비 완료되고 실행되기를 기다리는 프로세스들은 준비 완료 큐에 저장되어 있다.   
         - 연결 리스트로 저장되어 있다.   
         - 준비 완료 큐의 헤더는 연결 리스트의 첫 번째와 마지막 PCB를 가리키는 포인터를 포함한다.   
         - 각 PCB는 준비 완료 큐에서 다음 PCB를 가리키는 포인터 필드를 가진다.   
         
      - __장치 큐(Device queue)__   
         - 특정 입출력 장치를 위해 기다리고 있는 프로세스들의 목록    
         - 각 장치는 자신 고유의 장치 큐를 가진다.   
         
      #### 3.5 각 큐 이미지 
      
   - __새로운 프로세스는 초기에 ready queue에 넣어지고 실행을 위해 선택되기 전 또는 dispatched 될 때까지 대기한다.__   
      - __dispatch__ :star:   
         - 준비 리스트의 맨 앞에 있던 프로세스가 CPU를 점유하게 되는 것, 즉 준비 상태에서 실행 상태로 바뀌는 것   
         - dispatch : ready → running   
   
   - __프로세스가 CPU에 할당되면 프로세스는 실행되고 아래의 event들 중 하나가 발생할 수 있다.__   
      __1. 입출력 요청을 하고 입출력 큐로 들어간다.__   
      __2. 자식 프로세스를 생성하고 자식이 종료될 때까지 기다린다.__   
      __3. 인터럽트의 결과로 CPU로부터 강제적으로 제거되고 ready queue에 다시 놓여진다.__   
      
      - __1~2는 프로세스가 결국 Wait -> ready 상태로 바뀌고 ready queue에 다시 놓여진다.__   
      - __프로세스는 종료될 때까지 이러한 cycle을 계속한다.   
         - 프로세스가 모든 queue로부터 제거되고 각 PCB와 자원이 해제되는 순간 종료된다.   
         
#### 3.2.2 Schedulers(스케줄러)   
   
