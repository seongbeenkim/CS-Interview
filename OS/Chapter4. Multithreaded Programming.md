# :bookmark_tabs: Operating System Concepts 9th edition      
## 4장 Multithreaded Programming   
__중요하다고 생각되는 목차에는 :star: 표시해놓았습니다.__   
__:star:되어 있는 목차를 클릭하시면 클릭하신 목차의 내용이 있는 페이지로 넘어가며__   
__해당 페이지 내에 있는 중요 개념 옆에 :star: 표시해놓았습니다.__   

__혹시 잘못된 내용이 있거나 보완해야할 점이 있으면 `issue` 해주시거나 알려주시면 감사하겠습니다.:bow:__   

* [4.Multithreaded Programming](#4multithreaded-programming)   
  - [4.1 Overview](#41-overview) :star:  
    - [4.1.1 Motivation(동기)](#411-motivation동기) :star:   
    - [4.1.2 Benefit(이득)](#412-benefit이득) :star:   
  - [4.2 Multicore Programming](#42-multicore-programming) :star:   
    - [4.2.1 Programming Challenges(프로그래밍 과제)](#421-programming-challenges프로그래밍-과제)   
  - [4.3 Multithreading Models](#43-multithreading-models) :star:   
    - [4.3.1 Many-to-One Model(다대일 모델)](#431-many-to-one-model다대일-모델)   
    - [4.3.2 One-to-One Model(일대일 모델)](#432-one-to-one-model일대일-모델)   
    - [4.3.3 Many-to-Many Model(다대다 모델)](#433-many-to-many-model다대다-모델)   
  - [4.4 Thread Libraries](#44-thread-libraries) :star:   
    - [4.4.1 Pthreads](#441-pthreads)      
    - [4.4.2 Windows Threads](#442-windows-threads)   
    - [4.4.3 Java Threads](#443-java-threads) :star:   
  - [4.5 Implicit Threading](#45-implicit-threading)   
    - [4.5.1 Thread Pools(스레드 풀)](#451-thread-pools스레드-풀) :star:        
    - [4.5.2 OpenMP](#452-openmp)     
    - [4.5.3 Grand Central Dispatch(GCD)](#453-grand-central-dispatchgcd)     
  - [4.6 Threading Issues](#46-threading-issues)     
    - [4.6.1 The fork() and exec() System Calls(fork(), exec() 시스템 호출)](#461-the-fork-and-exec-system-callsfork-exec-시스템-호출)   
    - [4.6.2 Signal Handling(신호 처리)](#462-signal-handling신호-처리)       
    - [4.6.3 Thread Cancellation(스레드 취소)](#463-thread-cancellation스레드-취소)       
    - [4.6.4 Thread-Local Storage(TLS)](#464-thread-local-storagetls) :star:     
    - [4.6.5 Scheduler Activations](#465-scheduler-activations) :star:       
  
## 4.Multithreaded Programming   
### 4.1 Overview   
  
  - __Thread__ :star:      
    - CPU 사용의 기본 단위   
    - 스레드 id, 프로그램 카운터, 레지스터 집합, 스택으로 구성   
    - 동일 프로세스에 속한 다른 스레드와 코드, 데이터 영역, OS 자원(열린 파일, 신호)를 공유한다.   
    <p align="center"><img src="https://github.com/seongbeenkim/CS-Interview/blob/master/OS/image/4_1_ThreadDiagram.jpg" width = 500px height = 250px title="4_1_ThreadDiagram" alt="4_1_ThreadDiagram"></img><br><strong>4.1 프로세스 메모리</strong></p>    
    
#### 4.1.1 Motivation(동기)   

  - __Web Server가 단일 스레드 프로세스로 동작할 경우__   
    - 한번에 한 명의 Client에게만 서비스 제공하여 다른 Client는 오랜 시간을 기다려야 할 수도 있다.   
    
    - __해결 방법__ :star:   
      - __1. 프로세스 생성__   
        - __서버를 요청만 받는 단일 프로세스로 실행하게 하고 서비스를 제공할 별도의 프로세스를 생성한다.__  
        - 스레드가 유명해지기 전에 많이 사용되던 일반적인 방법   
        - __프로세스 생성은 시간 소모가 크고 자원을 많이 사용한다.__   
          - 새로운 프로세스가 존재하는 프로세스와 같은 작업을 할 경우 오버헤드를 초래할 수 있다.   
          - 그러므로 여러 스레드를 포함하는 하나의 프로세스를 사용하는 것이 훨씬 효율적이다.   
          
      - __2. 스레드 생성__   
        - __서버는 클라이언트 요청을 듣는 별도의 스레드를 생성하고, 요청을 받을 시 서비스를 제공할 스레드를 생성한다.__   
        <p align="center"><img src="https://github.com/seongbeenkim/CS-Interview/blob/master/OS/image/4_2_MultithreadedArchitecture.jpg" title="4_2_MultithreadedArchitecture" alt="4_2_MultithreadedArchitecture"></img><br><strong>4.2 멀티 스레드 서버 구조</strong></p>    
        
    - __스레드는 RPC에서 중요한 역할을 한다__   
      - 일반적으로 RPC 서버는 멀티스레드이다.   
      - 서버가 메세지를 받을 때, 별도의 스레드를 사용하여 해당 메세지를 서비스한다.   
      - 서버가 다수의 병행 요청에 대한 서비스를 할 수 있게 한다.   
      
    - __OS 커널도 멀티스레드이다.__   
      - 메모리 관리, 인터럽트 처리, 장치 관리 등 각 스레드가 수행한다.   
      
#### 4.1.2 Benefit(이득)    
  
  - __멀티스레드 프로그래밍 장점__ :star:      
    - __응답성(Responsivness)__   
      - 대화식 프로그램의 일부분이 block되거나 긴 작업을 수행하는 경우에도 프로그램 수행을 계속하게 함으로써 사용자에 대한 응답성을 증가시킨다.   
    - __자원 공유(Resource Sharing)__   
      - 프로세스는 오직 공유 메모리와 메세지 전달과 같은 기술을 통해 자원을 공유하며 이러한 기술은 프로그래머에 의해 명백하게 처리되어야 한다.   
      - 하지만, 스레드는 자동적으로 그들이 속한 프로세스의 자원들과 메모리를 공유한다.   
        - 코드와 자료 공유의 이점은 한 응용 프로그램이 같은 주소 공간 내에 여러 개의 다른 작업을 하는 스레드를 가질 수 있게 한다.   
        
    - __경제성(Economy)__   
      - 프로세스 생성을 위해 메모리와 자원을 할당하는 것은 비용이 많이 든다.   
      - 스레드는 자신이 속한 프로세스의 자원들을 공유하기 때문에 스레드를 생성하고 스레드간 문맥 교환하는 것이 훨씬 더 경제적이다.   
      - 오버헤드의 차이를 측정하는 것은 힘들 수 있으나 일반적으로 프로세스를 생성하고 관리하는데 더 많은 시간이 소요된다.   
        - ex) Solaris에서 프로세스 생성은 스레드 생성보다 3배 느리고, 문맥 교환은 5배 느리다.   
      
    - __확장성(Scalability)__   
      - 다중 스레드의 이점은 다중 처리기 구조에서 더욱 증가 된다.   
      - 각각의 스레드가 다른 처리기(프로세싱 코어)에서 병렬로 수행 될 수 있기 때문이다.   
      - 단일 스레드는 CPU가 많다고 하더라도 단지 한 CPU에서만 실행된다.   
        - 다중 CPU에서 다중 쓰레딩을 하면 병렬성이 증가 된다.   
        
      <p align="center"><img src="https://github.com/seongbeenkim/CS-Interview/blob/master/OS/image/4_3_ConcurrentSingleCore.jpg" title="4_3_ConcurrentSingleCore" alt="4_3_ConcurrentSingleCore"></img><br><strong>4.3 단일 코어 병행 수행</strong></p>    
      <p align="center"><img src="https://github.com/seongbeenkim/CS-Interview/blob/master/OS/image/4_4_ParralelMulticore.jpg" title="4_4_ParralelMulticore" alt="4_4_ParralelMulticore"></img><br><strong>4.4 멀티 코어 병렬 수행</strong></p>    
        
### 4.2 Multicore Programming    
  
  - __하나의 칩에 다수의 연산 코어가 들어간다.__   
    - 각 코어는 OS에서 별도의 프로세서로 표현된다.   
    
  - __멀티스레드 프로그래밍은 멀티 코어의 더 효율적인 사용과 더 향상된 병행성을 위한 메카니즘을 제공한다.__   
    - 하나의 프로세싱 코어는 한 번에 하나의 스레드만 실행시킬 수 있다.   
  
  - __단일 코어에서 병행성은 단지 스레드의 실행이 시간이 흐르면서 교차 배치되는 것을 의미한다.__   
  
  - __멀티 코어에서 병행성은 스레드를 병렬로 실행시킬 수 있는 것을 의미한다.__   
  
  - __병렬성과 병행성의 차이__ :star:      
    - __병렬성(Parallelism)__   
      - 물리적(기계) 성질   
      - 실제로 동시에 하나 이상의 작업이 수행된다.   
      - 오직 멀티 코어에서만 가능하다.   
      - ex) OpenMP, MPI, CUDA   
      
    - __병행성(Concurrency)__   
      - 논리적(프로그램) 성질    
      - 단일 코어에서는 물리적으로 병렬이 아닌 순차적으로 동작한다.   
        - ex) 멀티스레드 프로그램이 시분할 시스템처럼 CPU를 나눠 사용함으로써 사용자가 병행성을 느낄 수 있도록 해준다.   
        - 멀티스레드로 작성된 프로그램이라 하더라도 멀티코어가 아니면 병렬로 작동하지 못한다.   
      - ex) Mutex, Deadlock   
      
    - __멀티스레드 프로그램에서 벌어지는 버그를 concurrency bug라고 부르는데, 하드웨어가 반드시 병렬성을 지원하지 않더라도 논리적으로 발생하기 때문에 concurrency라고 한다.__   
    
#### 4.2.1 Programming Challenges(프로그래밍 과제)   

  - __멀티 코어 시스템을 위한 프로그래밍 과제 5가지__   
    - __작업 확인(Identifying tasks)__   
      - 분리되고 병행 작업으로 나뉠 수 있는 영역을 찾기 위해 app을 검사하는 것과 관련있다.   
      - 작업들은 각각 독립적이고 각 코어에서 병렬 수행 될 수 있다.   
    - __균형(Balance)__   
      - 병렬로 실행할 수 있는 작업들을 확인하는 동안, 프로그래머는 반드시 작업들이 동등하게 수행되는 것을 보장해야 한다.   
      - 일부 상황에서, 특정 작업이 다른 작업들보다 더 가치있게 고려될 수도 있다.   
        - 이러한 작업을 위해서 별도의 실행 코어를 사용하는 것은 비용적인 면에서 가치있지 않다.   
    - __데이터 분리(Data splitting)__   
      - app이 여러 작업들로 나뉘는 것처럼, 작업들에 의해 조작되고 접근되는 데이터는 별도의 코어에서 실행되기 위해 반드시 나뉘어져야 한다.   
    - __데이터 의존성(Data dependancy)__
      - 작업들에 의해 접근되는 데이터는 반드시 두 개 이상의 작업 사이에 의존성을 위해서 검사되어야 한다.   
      - 하나의 작업이 다른 작업의 데이터에 의존할 때, 프로그래머는 반드시 작업의 실행이 데이터 의존성을 이루기 위해서 동기화되게 보장해야 한다.   
    - __테스트, 디버그(Testing and debugging)__   
      - 프로그램이 다수 코어에서 병렬 수행될 때, 많은 다른 실행 경로가 존재한다.   
      - 병행 프로그램과 같은 디버그와 테스트는 단일 스레드 app을 테스트, 디버그하는 것 보다 훨씬 어렵다.   
     
### 4.3 Multithreading Models   
  
  - __스레드를 위한 지원은 사용자 스레드(user thread)와 커널 스레드(kernel thread)가 있다.__ :star:   
    - __사용자 스레드__   
      - 커널 위에서 지원되며 커널의 지원없이 관리된다.   
    - __커널 스레드__   
      - OS에 의해 직접적으로 지원되고 관리되며 대부분 OS가 커널 스레드를 지원한다.   

#### 4.3.1 Many-to-One Model(다대일 모델)   

  - __하나의 커널 스레드에 다수의 사용자 수준 스레드가 매핑된다.__   
    - 스레드 관리는 사용자 공간에서 스레드 라이브러리에 의해 된다.   
      - 스레드 관리에는 효유율적이지만 하나의 스레드가 긴 작업을 실행 할 동안에 다른 스레드의 나머지 작업들은 대기하게 된다.    
    - __스레드 하나가 blocking 시스템 호출을 할 경우, 모든 프로세스가 block된다.__   
    - 한 번에 오직 하나의 스레드가 커널에 접근할 수 있기 때문에 다수의 스레드는 멀티 코어 시스템에서 병렬로 수행될 수 없다.    
    - 멀티 프로세싱 코어의 장점을 불능으로 만들기 때문에 대다수 OS가 사용하지 않는다.   
      - ex) Solaris를 위한 Green thread 스레드 라이브러리는 다대일 모델을 사용했다.     
      <p align="center"><img src="https://github.com/seongbeenkim/CS-Interview/blob/master/OS/image/4_5_ManyToOne.jpg" height = 250px title="4_5_ManyToOne" alt="4_5_ManyToOne"></img><br><strong>4.5 다대일 모델</strong></p>
    
#### 4.3.2 One-to-One Model(일대일 모델)    

  - __하나의 커널 스레드에 하나의 사용자 수준 스레드가 매핑된다.__   
    - 하나의 스레드가 긴 작업을 실행 할 동안에도 다른 스레드의 나머지 작업들은 실행된다.    
    - 스레드 하나가 blocking 시스템 호출을 할 경우, 다른 스레드가 실행될 수 있기 때문에 다대일 모델보다 더 많은 병렬성을 제공한다.   
    - 멀티 코어 프로세서에서 다수의 스레드가 병렬 수행될 수 있다.   
    - 유일한 결점은 사용자 수준 스레드 생성 시, 대응되는 커널 스레드를 생성해야 한다는 것이다.   
      - 커널 스레드 생성의 오버헤드는 app의 수행에 부담을 줄 수 있기 때문에, 이 모델의 대부분의 구현은 시스템에 의해 지원되는 스레드의 수를 제한한다.   
        - ex) Linux, Windows는 일대일 모델을 구현    
      <p align="center"><img src="https://github.com/seongbeenkim/CS-Interview/blob/master/OS/image/4_6_OneToOne.jpg" height = 250px title="4_6_OneToOne" alt="4_6_OneToOne"></img><br><strong>4.6 일대일 모델</strong></p>   
        
#### 4.3.3 Many-to-Many Model(다대다 모델)    
  
  - __다수의 사용자 수준 스레드가 더 작거나 동등한 수의 커널 스레드에 다중화 된다.__   
    - 커널 스레드의 수는 특정 기계나 app에 의해 결정된다.   
      - 개발자는 필요한 만큼 많은 사용자 수준 스레드를 생성할 수 있으며 그에 상응하는 커널 스레드가 멀티 코어 프로세서에서 병렬로 수행된다.   
    - 스레드가 긴 작업등의 호출을 발생 시켰을 때 커널이 다른 스레드의 작업을 스케줄링 한다.   
    - 스레드가 blocking 시스템 호출을 할 때, 커널은 실행을 위한 다른 스레드를 스케줄링 한다.   
    <p align="center"><img src="https://github.com/seongbeenkim/CS-Interview/blob/master/OS/image/4_7_ManyToMany.jpg" height = 250px title="4_7_ManyToMany" alt="4_7_ManyToMany"></img><br><strong>4.7 다대다 모델</strong></p>   
    
  - __Two-level model(두 단계 모델)__   
    - 다대다 모델의 변형   
    - 다대다 모델의 특성도 가지고 있지만, 하나의 사용자 스레드가 하나의 커널 스레드에 종속되는 것도 허용한다.   
    <p align="center"><img src="https://github.com/seongbeenkim/CS-Interview/blob/master/OS/image/4_8_TwoLevel.jpg" height = 250px title="4_8_TwoLevel" alt="4_8_TwoLevel"></img><br><strong>4.8 두 단계 모델</strong></p>    

### 4.4 Thread Libraries   
  
  - __스레드를 관리하고 생성하기 위한 API를 제공한다.__   
  
  - __스레드 라이브러리 구현하는 두 가지 방법__   
    - __1. 커널 지원없이 사용자 공간에만 라이브러리 제공__   
      - 라이브러리를 위한 모든 코드, 자료 구조가 사용자 공간에 존재한다.   
      - 라이브러리의 함수를 호출하는 것은 시스템 호출이 아닌 사용자 공간의 지역 함수를 호출이다.   
    - __2. OS에 의해 직접적으로 지원된 커널 수준 라이브러리 구현__   
      - 라이브러리를 위한 모든 코드, 자료 구조가 커널 공간에 존재한다.   
      - 라이브러리 API를 호출하는 것은 시스템 호출이다.   
      
  - __Posix Pthreads__   
    - 사용자 또는 커널 라이브러리로서 제공될 수 있다.   
      - ex) UNIX, Linux    
  
  - __Windows thread library__   
    - Windows 시스템에서 이용가능한 커널 라이브러리   
  
  - __Java thread API__   
    - Java 프로그램에서 직접적으로 관리되고 생성된다.   
    - 대부분의 경우에서 JVM은 host OS의 제일 위에서 실행되기 때문에 Java thread API는 host 시스템에서 이용가능한 thread library를 사용하여 구현된다.   
      - ex) Windows 시스템에서 Java thread는 Windows API를 사용하여 구현된다.   
      
  - __Posix와 Windows thread에서 전역으로 선언된 데이터는 같은 프로세스에 속하는 모든 스레드가 공유할 수 있다.__    
  
  - __Java는 전역 데이터의 개념이 없기 때문에 공유 데이터의 접근은 반드시 스레드들 사이에 명백히 합의되어야 한다.__   
  
  - __함수에 지역 데이터로 선언된 것은 스택에 저장된다.__ :star:   
    - 각 스레드는 자신만의 스택을 가지기 때문에, 각 스레드는 자신만의 지역 데이터 복사본을 가진다.   
    
  - __다수의 스레드를 생성하기 위한 2가지 전략__ :star:    
    - __1.비동기 스레딩(Asynchronous threading)__   
      - 부모가 자식을 생성한 뒤 부모는 실행을 재개하여 부모와 자식이 동시에 실행된다.   
      - 각 스레드는 다른 스레드와 독립적으로 실행되기 때문에 부모는 자식이 종료될 때를 알 필요가 없다.   
      - 스레드 독립적이기 때문에, 스레드 사이에 아주 작은 데이터만 공유한다.   
       
    - __2.동기 스레딩(Synchronous threading)__   
      - fork-join 전략   
      - 부모가 하나 이상의 자식을 생성하면, 부모는 실행을 재개하기 위해서 모든 자식이 종료될 때까지 기다려야 한다.   
        - 부모에 의해 생성된 스레드는 동시에 실행되지만, 부모는 이러한 스레드의 실행이 완료될 때까지 계속 진행할 수 없다.   
      - 스레드 사이에 많은 데이터를 공유한다.   
      - ex) 자식들이 계산한 결과값을 부모가 합산한다.   

#### 4.4.1 Pthreads    
  
  - __POSIX(IEEE 10031.1c)가 스레드 생성과 동기화를 위해 정의한 표준 API__   
    - 스레드의 동작에 관한 명세이지 구현한 것은 아니다.   
    - 많은 시스템이 Pthread 명세를 보고 구현을 한다.   
      - 대부분 UNIX 형태의 시스템   
        - ex) Linux, Mac OS X, Solaris   
      - Windows는 Pthread를 지원하지 않지만, Windows를 위한 제3의 구현이 있다.   
        - 퍼블릭 도메인(Public Domain)에서 쉐어웨어(shareware)형태   
        
  - __각 스레드는 속성 집합을 가진다.__   
    - 스택 크기와 스케줄링 정보를 포함한다.   
    
#### 4.4.2 Windows Threads   
#### 4.4.3 Java Threads    
  
  - __Java 스레드는 JVM을 제공하는 어느 시스템에서나 이용가능하다.__   
  
  - __모든 Java 프로그램은 적어도 하나의 단일 제어 스레드를 포함한다.__   
    - main() 함수로만 이루어진 단순한 Java 프로그램조차 JVM 내에서 하나의 단일 스레드로 수행된다.   
  
  - __Java 스레드 생성 방법__ :star:      
    - __1. Thread Class를 상속받는 새로운 Class를 생성하고 run() 함수를 오버라이드한다.__   
    
    - __2. Runnable interface를 구현하는 Class를 정의하고, run() 함수를 정의한다.__   
      - run() 함수를 구현하는 코드는 별도의 스레드로 실행된다.   
    
    - __2번의 방법이 더 많이 사용된다.__   
      - 스레드 생성은 Thread Class의 객체 인스턴스를 생성하고 Runnable 객체 생성자를 전달함으로써 수행된다.   
      - 스레드 객체를 생성하는 것이 새로운 스레드를 생성하지 않고 start() 함수가 새로운 thread를 생성한다.   
      - 새로운 객체를 위해 start() 함수를 호출하는 것이 하는 2가지 작업   
        - __1. JVM에서 새로운 스레드의 메모리를 할당하고 초기화한다.__   
        - __2. run() 함수를 호출한다.__   
          - 스레드가 JVM에 의해 실행될 수 있는 자격을 준다.   
          - 절대 직접적으로 run() 함수를 호출하지 않는다.   
            - start() 함수를 호출하면 run() 함수가 호출된다.   
            
  - __Windows와 Pthread에서는 공유 데이터가 전역 변수로 선언되기 때문에 스레드간의 자료 공유를 쉽게 할 수 있다.__   
    - Java는 전역 변수의 개념이 없기 때문에, Java 프로그램에서 둘 이상의 스레드가 자료를 공유해야 한다면 공유 객체에 대한 참조를 적당한 스레드에게 전달해야 한다.   
      - ex) Summmation 스레드와 메인 스레드가 Sum class의 객체를 공유 할 경우, 공유된 객체는 getSum(), setSum()을 통해 참조되어진다.   
      
  - __Windows와 Pthread에서 부모 스레드는 WaitForSingleObject(), Pthread_join()을 사용하여 자식 스레드가 종료될 때까지 기다린다.__   
    - Java는 Interrupted Exception을 발생시킬 수 있는 Join()을 사용하지만 Exception을 무시하는 것을 선택할 수 있다.   
    
### 4.5 Implicit Threading   

  - __멀티 스레드 app의 더 나은 설계를 위해서 스레드의 생성, 관리를 app 개발자로부터 컴파일러와 런타임 라이브러리에게 맡기는 전략을 Implicit Threading이라고 한다.__   
  
#### 4.5.1 Thread Pools(스레드 풀)   
  
  - __멀티 스레드 서버의 잠재적인 문제__   
    - __1. 스레드를 생성하기 위해 소요되는 시간의 양과 작업을 완료해야지만 종료되어지는 사실__   
    - __2. 모든 요청마다 새로운 스레드를 생성하여 서비스 하게 할 경우, 동시에 실행할 수 있는 스레드의 한계를 정해야 하는데 시스템에서 동시에 실행되는 스레드 수의 한계를 정하지 못한다.__   
      - 무제한적인 스레드는 CPU 시간 또는 메모리같은 시스템 자원을 다 써버릴 수도 있다.   
      - __이러한 문제를 해결하기 위한 방법이 Thread pool이다.__    
      
  - __Thread Pool__ :star:    
    - 프로세스를 시작 할 때 미리 일정한 수의 스레드를 만들어 pool에 두는 것이다.    
    - 프로세스 시동 시 다수의 스레드를 생성하고 pool에 저장한 뒤, 서버가 요청을 받을 때 pool에서 이용가능한 스레드가 있다면 스레드 하나를 깨워 해당 요청에 대한 서비스를 수행하고, 수행은 마친 스레드는 pool에 들어오고 다른 작업을 기다린다.   
    - 이용가능한 스레드가 없다면, 서버는 하나의 스레드가 이용가능해질 때까지 기다린다.   
    - __Thread Pool의 장점__   
      __1. 스레드 생성을 위해 기다리는 것보다 존재하는 스레드를 사용하여 요청에 대해 서비스하는 것이 더 빠르다.__    
      __2. Thread pool이 어느 한 시점에 존재할 수 있는 스레드의 수를 제한한다.__   
        - 많은 병행 스레드를 지원하지 못하는 시스템에서 중요하다.   
      __3. 작업 실행을 위해 다른 전략을 사용할 수 있게 해준다.__    
        - ex) 작업이 time delay 후 또는 주기적으로 실행되게 스케줄될 수 있다.   
        
    - __Pool에 있는 스레드의 수는 동시에 발생될 수 있다고 예상된 Client의 요청의 수, 물리적 메모리의 양, 시스템에서 CPU의 수와 같은 요소에 기반하여 스스로 설정될 수 있다.__   
      
    - __더 정교한 Thread pool 구조는 사용 패턴에 따라 Pool에 있는 스레드의 수를 동적으로 조절할 수 있다.__   
    
#### 4.5.2 OpenMP   
  
  - __공유 메모리 환경에서 병렬 프로그래밍을 지원하는 FORTRAN 또는 C, C++에서 작성된 프로그램을 위한 API이며 컴파일러 지시문의 집합이다.__   
  - __병렬 실행될 수 있는 코드의 영역을 구분한다.__   
    - app 개발자는 병렬 영역에 컴파일러 지시문을 넣고 이러한 지시문은 병렬로 해당 구역을 실행하기 위해서 OpenMP 런타임 라이브러리를 삽입한다.   
  - __시스템에 있는 프로세싱 코어 수 만큼 스레드를 생성하고 모든 스레드는 동시에 병렬 영역을 실행한다.__   
    - 각 스레드가 병렬 영역을 벗어나면, 스레드는 제거된다.   
    
  - __병렬용 for문도 제공한다.__   
  
  - __개발자가 수동으로 스레드의 수를 정할 수도 있다.__    
    - 어떤 데이터가 스레드에 공유되고 숨겨져야할 지 정의할 수 있다.    
    
  - __Linux, Windows, Mac OS X에서 이용가능하다.__    
  
### 4.5.3 Grand Central Dispatch(GCD)    

  - __Mac OS X와 ios를 위한 기술로 app 개발자가 병렬로 실행할 수 있는 코드의 부분을 정의하게 하는 런타임 라이브러리, API, C언어의 확장된 조합이다.__   
  
  - __GCD는 blocks으로 알려진 C, C++ 언어 확장자를 확인한다.__   
    - "^", "{}"를 사용한다.   
      - ex) ^{ printf("i am a block"); }    
  
  - __GCD는 dispatch queue에 block을 둠으로써 런타임 실행을 위한 block들을 스케줄한다.__    
    - queue로부터 block이 제거될 때, block을 Thread pool로부터 이용가능한 스레드에 할당한다.   
  
  - __dispatch queue 2가지 종류__   
    
    - __1. 직렬 큐(Serial queue)__   
      - FIFO 순서에 의해 block들이 제거된다.   
      - block이 제거되면 다른 block이 제거되기전에 반드시 수행을 완료해야한다.   
      - 각 프로세스는 자신 고유의 Serial queue를 가진다.   
        - Main queue라고도 알려져 있다.   
      - 개발자는 특정 프로세스에 local인 추가 Serial queue를 생성할 수 있다.   
      - 다수 작업의 연속적인 실행을 보장하는데 유용하다.   
      
    - __2. 동시 큐(Concurrent queue)__   
      - FIFO 순서에 의해 block들이 제거된다.   
      - 한 번에 여러 block이 제거될 수 있다.   
        - 여러 block들이 병렬로 수행될 수 있다는 의미   
      - 3개의 시스템 전역 concurrent dispatch queue가 있다.   
        - 3가지 큐는 우선순위 low, default, high에 따라 구분된다.   
          - 우선순위는 상대적으로 중요한 block들을 대략 표시해준다.   
        - 높은 우선순위를 가진 block이 높은 순위 dispatch queue에 놓여야한다.   
        
  - __GCD의 Thread Pool은 POSIX threads로 구성된다.__   
  
  - __GCD는 시스템 수용량과 app의 요구에 따라 thread의 수를 줄이고 늘리면서 pool을 관리한다.__   
  
### 4.6 Threading Issues   
#### 4.6.1 The fork() and exec() System Calls(fork(), exec() 시스템 호출)   

  - __fork()와 exec()의 의미는 멀티스레드 프로그램에서 달라진다.__   
    - __프로그램에서 하나의 스레드가 fork()를 호출할 때, 새로운 프로세스가 모든 스레드를 복사하는지? 또는 새로운 프로세스가 싱글 스레드인지에 따라서 일부 UNIX 시스템은 두 종류의 fork() 중 하나를 선택한다.__    
      - __1. 모든 스레드 복사__   
      - __2. fork()를 호출한 스레드만 복사__   
      
    - __하나의 스레드가 exec()을 호출하면, exec()의 파라미터로 지정된 프로그램이 모든 스레드를 포함한 전체 프로세스를 대체한다.__    
      - fork()를 한 후, exec()을 즉시 호출할 경우, 모든 thread를 복사하는 것이 불필요하다.    
        - exec()의 파라미터로 지정된 프로그램이 모든 것을 대체할 것이기 때문이다.   
      - 이럴 경우에는 2번의 fork() 방식이 적절하다.   
      - fork()를 한 후, exec()을 하지 않는다면, 1번의 fork() 방식을 사용해 모든 스레드를 복사해야한다.   
      
#### 4.6.2 Signal Handling(신호 처리)   

  - __신호(Signal)__   
    - 특정 event가 발생한 프로세스를 알려주기 위해 UNIX에서 사용된다.    
    - 신호는 동기식 또는 비동기식으로 전달되어진다.    
    
    - __모든 신호가 따라야 할 패턴__   
      - __1. 신호는 특정 event의 발생에 의해 생성되어야 한다.__   
      - __2. 신호는 프로세스에게 전달되어야 한다.__   
      - __3. 신호가 전달되면, 반드시 처리되어야 한다.__   
      
    - __동기식 신호(Synchronous signal)__   
      - 0에 의한 나누기, 불법적인 메모리 접근   
      - 신호를 발생시킨 연산을 수행하는 같은 프로세스에 전달된다.   
      
    - __비동기식 신호(Asynchronous signal)__   
      - 실행하는 프로세스 외부에 의해 생성된 신호   
      - Control + c와 같은 특정 키 입력으로 프로세스를 강제 종료하거나, 타이머가 만료되어 종료되는 경우   
      - 다른 프로세스에 전달된다.   
      
    - __신호는 2가지 핸들러 중 하나에 의해 처리될 수 있다.__   
      - __1. default 신호 처리기__   
        - 모든 신호는 해당 신호가 처리될 때 커널이 실행하는 default 신호 처리기를 가진다.   
      - __2. 사용자 정의 신호 처리기__   
        - 신호를 처리하기 위해서 호출되는 사용자 정의 신호 처리기에 의해 default 동작이 오버라이드 될 수 있다.   
      
    - __싱글 스레드 프로그램에서 신호 처리는 간단하다.__   
      - 신호는 항상 프로세스에 전달된다.   
    
    - __멀티 스레드 프로그램에서 신호 전달은 복잡하다.__   
      - __신호 전달 방식 종류__   
        - __1. 신호가 적용되는 스레드에 신호를 전달한다.__   
        - __2. 프로세스의 모든 스레드에 신호를 전달한다.__   
        - __3. 프로세스의 특정 스레드에 신호를 전달한다.__   
        - __4. 특정 스레드를 프로세스를 위해 모든 신호를 받을 수 있게 지정한다.__   
        
    - __생성된 신호 유형에 따라 신호 전달 방식이 다르다.__   
      - 동기식 신호는 프로세스 내에서 신호를 야기한 스레드에게만 전달되야한다.    
      - 비동기식 신호는 명확하지 않기 때문에 프로세스 내 모든 스레드에 전달되야한다.   
      
#### 4.6.3 Thread Cancellation(스레드 취소)   

  - __스레드가 완료되기전에 강제 종료시키는 것이다.__   
    - ex) 다수의 스레드가 동시에 DB 검색 중, 하나의 스레드가 결과 반환 시, 나머지 스레드를 종료한다.   
    
  - __목표 스레드(Target thread)__   
    - 종료되어야 할 스레드   
    - 목표 스레드 취소 방식 2가지   
      - __비동기식 취소(Asynchronous cancellation)__   
        - 하나의 스레드가 즉시 목표 스레드를 종료한다.   
        
      - __지연 취소(Deffered cancellation)__   
        - 목적 스레드는 주기적으로 자신이 종료되어야 하는지 확인하고 종료되어야 할 경우, 종료한다.   

  - __자원이 취소 스레드의 할당되어 있거나 스레드가 다른 스레드와 공유하고 있는 데이터를 갱신하는 중간에 취소될 경우 스레드 취소에 어려움이 생긴다.__    
    - 특히 비동기 취소에서 더 골치아파진다.   
      - OS는 취소된 스레드로부터 자원을 회수하지만 모든 자원을 회수하지는 않는다.   
        - 그러므로, 비동기적으로 스레드를 취소하는 것은 필요한 시스템 전역 자원을 free하게 하지 못할 수 있다.   
        
    - 지연 취소에서는 하나의 스레드가 목표 스레드가 취소되어야 한다고 표시하지만, 취소는 목표 스레드가 취소되어야 하는지 확인하기 위한 플래그를 확인한 후에만 발생한다.    
      - 안전하게 취소될 수 있는 시점에 이러한 확인을 할 수 있으며 Pthread는 이러한 시점을 취소점(Cancellation point)이라고 한다.   
      
  - __pthread_cancle() 호출은 단지 목표 스레드 취소를 위한 요청만을 나타낸다.__   
    - 실제 취소는 목표 스레드가 해당 요청을 어떻게 다루는지에 달려있다.   
    - Pthread가 지원하는 3가지 취소 모드가 있으며 각 mode는 state와 type으로 정의되어 있다.   
      - mode / state / type   
      - off, Disabled, -
      - Deffered, Enabled, Deffered   
      - Asynchronous, Enabled, Asynchronous   
      
      - __Pthread는 스레드가 취소를 할 수 있게 하거나 할 수 없게 한다.__   
        - state가 Disable일 경우, 스레드는 취소될 수 없다.   
        - 하지만, 취소 요청은 보류상태로 남아있어 해당 스레드가 취소를 Enable하면 취소 요청에 반응하여 취소될 수 있다.   
        
    - __default 취소는 지연 취소이다.__   
      - 취소는 오직 스레드가 취소점에 도달할 때 일어난다.   
      - 취소점을 형성하기 위한 방법은 pthread_testcancel() 함수를 호출하는 것이다.   
      - 취소 요청이 보류 상태로 발견된다면, cleanup handler 라고 불리는 함수가 호출된다.    
        - 스레드가 종료되기전에 할당되어 있는 모든 자원을 해재한다.   
    
    - __Pthread를 사용하여 스레드를 취소하는 것은 신호를 통해 처리된다.__   
    
#### 4.6.4 Thread-Local Storage(TLS)    

  - __각 스레드는 자신만의 특정 데이터를 가져야 할 상황이 생길 수도 있다.__    
    - 이러한 데이터를 TLS라고 한다.   
    - __지역 변수와 TLS를 혼동하기 쉽다.__ :star:      
      - 지역 변수는 단일 함수 호출동안만 보여지고, TLS는 함수 호출을 초월하여 보여진다.   
    - __TLS는 Static 데이터와 비슷하지만, TLS는 각 스레드에 고유한 데이터다.__   
      - ex) 거래 처리 시스템에서, 별도의 스레드에서 각 거래 업무를 해야할 경우   
        - 각 거래 업무는 고유한 식별자를 할당받을 수 있고, 이러한 고유한 식별자를 각 스레드와 연관시키기 위해 TLS를 사용할 수 있다.   
        
#### 4.6.5 Scheduler Activations    
  
  - __커널과 스레드 라이브러 사이에 통신이 고려되어야 한다.__   
    - 다대다, 두 단계 모델이 필요하다.   
    - 최고의 성능을 보장할 수 있게 도와주기 위해 커널 스레드의 수가 동적으로 적용되게 할 수 있게 해준다.   
    
  - __많은 시스템은 사용자 스레드와 커널 스레드 사이에 중간 자료구조를 둔다.__ :star:   
    - __이러한 자료구조를 경량 프로세스(lightweight process) 또는 LWP라고 부른다.__   
    - __app에서 가상 프로세서로 보이는 LWP는 실행시킬 사용자 스레드를 스케줄한다.__   
    - __커널 스레드가 block(입출력 작업이 완료되기를 기다리는 경우)되면 LWP도 block된다.__    
       - LWP에 연결된 사용자 스레드도 block된다.   
    <p align="center"><img src="https://github.com/seongbeenkim/CS-Interview/blob/master/OS/image/4_13_Lightweight.jpg" title="4_13_Lightweight" alt="4_13_Lightweight"></img><br><strong>4.13 경량 프로세스(LWP)</strong></p>
       
  - __app은 효율적으로 실행하기 위해서 다수의 LWP가 필요할 수도 있다.__    
    - 단일 프로세서에서 실행되는 CPU 중심 app일 경우, 한 번에 하나의 스레드만 실행되기 때문에 LWP 하나면 충분하다.   
    - 입출력 중심 app은 다수의 LWP가 필요하다.   
      - 각 병행 blokcing 시스템 호출을 위해서 LWP가 필요할 수 있다.   
      - 5개의 다른 파일 읽기 요청이 동시에 발생된 경우   
        - 모든 요청이 커널에 있는 입출력 완료를 기다릴 수 있기 때문에 5개의 LWP가 필요하다.   
        - 프로세스가 4개의 LWP만 가지고 있다면, 다섯번째 요청은 LWP 중 하나가 커널로부터 돌아올 때까지 기다려야만 한다.   
        
  - __Scheduler Activations__   
    - __사용자 스레드 라이브러리와 커널 사이의 통신을 위한 스키마__   
    - __커널은 가상 처리기(LWP) 집합을 제공하고 app은 사용자 스레드를 이용가능한 가상 처리기에 스케줄 할 수 있다.__   
      - __커널은 app에게 반드시 특정 event에 대해 알려주어야 한다.__    
        - 이러한 절차를 upcall 이라고 한다.   
      - __upcall은 스레드 라이브러리의 upcall 처리기에 의해 처리된다.__   
        - upcall 처리기는 반드시 가상 처리기(LWP)에서 실행되어야 한다.   
      - __app 스레드가 block되려고 할 때, upcall을 발동시키는 event가 발생한다.__   
        - 커널은 app에게 어떤 스레드가 block이 되려고하는지 알리기 위해서 upcall을 만든다.   
        - 커널은 app에게 새로운 가상 처리기(LWP)를 할당한다.   
        - app은 이 새로운 가상 처리기에서 upcall 처리기를 실행시켜 upcall 처리기는 blocking된 스레드의 상태를 저장하고, blocking된 스레드가 실행되고 있는 가상 처리기를 반환한다.   
        - upcall 처리기는 새로운 가상 처리기에서 실행 가능한 다른 스레드를 스케줄한다.   
        - blocking된 스레드가 기다리고 있던 사건이 발생하면, 커널은 이전에 blocked되었던 스레드가 실행가능하다는 사실을 알려주는 또다른 upcall을 스레드 라이브러리에 전달한다.   
        - 이러한 event를 위한 upcall 처리기는 가상 처리기가 필요하고, 커널은 새로운 가상 처리기를 할당하거나 또는 사용자 스레드의 처리기 중 하나를 선점하여 해당 가상 처리기에서 upcall 처리기를 실행시킨다.   
        - unblocked된 스레드를 실행 가능하다고 표시한 후에, app은 이용가능한 가상 처리기에서 실행할 적합한 스레드를 스케줄한다.   
