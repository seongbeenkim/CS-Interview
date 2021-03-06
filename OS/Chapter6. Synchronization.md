# :bookmark_tabs: Operating System Concepts 9th edition      
## 6장 Synchronization      
__중요하다고 생각되는 목차에는 :star: 표시해놓았습니다.__   
__:star:되어 있는 목차를 클릭하시면 클릭하신 목차의 내용이 있는 페이지로 넘어가며__   
__해당 페이지 내에 있는 중요 개념 옆에 :star: 표시해놓았습니다.__   

__혹시 잘못된 내용이 있거나 보완해야할 점이 있으면 `issue` 해주시거나 알려주시면 감사하겠습니다.:bow:__   

- [6. Synchronization](#6-synchronization)   
  - [6.1 Background](#61-background) :star:   
  - [6.2 The Critical-Section Problem](#62-the-critical-section-problem) :star:   
  - [6.3 Petersons's Solution](#63-petersonss-solution)   
  - [6.4 Synchronization Hardware](#64-synchronization-hardware)   
  - [6.5 Mutex Locks](#65-mutex-locks) :star:    
  - [6.6 Semaphores](#66-semaphores) :star:      
    - [6.6.1 Semaphore Usage(세마포어 사용)](#661-semaphore-usage세마포어-사용) :star:   
    - [6.6.2 Semaphore Implementation(세마포어 구현)](#662-semaphore-implementation세마포어-구현) :star:   
    - [6.6.3 Deadlocks and Starvation(데드락과 기아 현상)](#663-deadlocks-and-starvation데드락과-기아-현상) :star:   
    - [6.6.4 Priority Inversion(우선순위 역전)](#664-priority-inversion우선순위-역전) :star:   
  - [6.7 Classic Problems of Synchronization](#67-classic-problems-of-synchronization)   
    - [6.7.1 The Bounded-Buffer Problem(유한 버퍼 문제)](#671-the-bounded-buffer-problem유한-버퍼-문제)   
    - [6.7.2 The Readers-Writers Problem(저자-독자 문제)](#672-the-readers-writers-problem저자-독자-문제)      
    - [6.7.3 The Dining-Philosophers Problem(철학자들의 만찬)](#673-the-dining-philosophers-problem철학자들의-만찬)   
  - [6.8 Monitors](#68-monitors)      
    - [6.8.1 Monitor Usage(모니터 사용)](#681-monitor-usage모니터-사용)  
  - [6.10 Alternative Approaches](#610-alternative-approaches)      
    - [6.10.1 Transactional Memory(트랜잭션 메모리)](#6101-transactional-memory트랜잭션-메모리) :star:   
    - [6.10.2 OpenMP](#6102-openmp)   
    - [6.10.3 Functional Programming Language(실용적인 프로그래밍 언어)](#6103-functional-programming-language실용적인-프로그래밍-언어)   

## 6. Synchronization   

  - __Cooperating process__   
    - 하나의 프로세스가 다른 프로세스에 영향을 주거나 영향을 받을 수 있는 프로세스    
    - 논리적 주소 공간(code, data)을 직접적으로 공유하거나 메세지나 파일을 통해 data를 공유할 수 있다.   
    
### 6.1 Background   
  
  - __3.4.1장에서 생산자-소비자 문제에서 프로세스들이 메모리를 공유할 수 있게 하기 위해서 유한 버퍼가 사용되었고 이 유한 버퍼 사이즈-1 만큼의 아이템을 허용하도록 했다.__   
    - __유한 버퍼를 사용하는 결점을 해결하기 위해서 Counter를 사용하여 알고리즘을 수정할 수 있다.__   
      - Counter는 0으로 초기화되고 아이템이 추가될 때 증가하고, 제거될 때 감소한다.   
        - 하지만, 소비자의 count--, 생산자의 count++를 병행하게 수행시키게되면 올바른 값이 도출되지 않는다.   
          - ex) `counter = 5`이고 `소비자 count--`, `생산자 count++`을 병행 수행하면 값은 4 또는 5 또는 6의 값이 나올것이며 올바른 값은 5다.   
        <p align="center"><img src="https://github.com/seongbeenkim/CS-Interview/blob/master/OS/image/6_RaceCondition.jpg" height = 400px title="6_RaceCondition" alt="6_RaceCondition"></img><br><strong>생산자-소비자 문제에서의 경쟁 상태</strong> </p>   
        
        - `count++`은 지역 CPU 레지스터 중 하나인 `register1`에 저장되고 `register1 = count`, `register1 += 1`을 한 뒤 `counter = register1`   
        - `count--`은 지역 CPU 레지스터 중 하나인 `register2`에 저장되고 `register2 = count`, `register2 -= 1`을 한 뒤 `counter = register2`   
        - `register1`, `register2`가 같은 물리적 레지스터(누산기)라고 하더라도 이러한 레지스터의 내용은 인터럽트 처리기에 의해 저장되고 회복된다.   
        - `count++`, `count--`를 병행 실행하면 위의 결과와 같이 생산자는 6, 소비자는 4라는 값을 얻게 된다.   
        
        - __이렇게 두 프로세스가 병행하게 counter 변수를 조작하게 했기 때문에 잘못된 값을 도출한다.__   
        
        - __경쟁 상태(Race condition)__ :star:   
          - __여러 프로세스가 동시에 같은 데이터를 조작하고 접근할 수 있고 접근이 발생하는 순서에 따라 실행의 결과가 달라질 수 있는 상황을 경쟁 상태라고 한다.__   
          - 이러한 경쟁 상태를 방지하기 위해서 한 번에 하나의 프로세스만 Counter 변수를 조작하게 해야 한다.   
            - __이것을 보장하기 위해서 여러 방식에서 프로세스를 동기화해야 한다.__   
          
#### 6.2 The Critical-Section Problem   

  - __임계 영역(Critical section)__ :star:   
    - n 개의 프로세스 {P0, P1, P2, … Pn}이 있는 시스템에서 각 프로세스는 임계 영역(critical section)이라고 부르는 코드 부분을 포함한다.   
      - 임계 영역안에서는 다른 프로세스와 공유하는 변수를 변경하거나 테이블을 갱신하거나 파일을 쓰거나 하는 등의 작업을 수행한다.   
    - __하나의 프로세스가 임계 영역에 실행중일 때, 다른 프로세스는 임계 영역에 들어갈 수 없다.__   
    
  - __임계 영역 문제(Critical-section problem)__ :star:   
  
    - __프로세스들이 협동하기 위해 사용할 수 있는 협약을 설계하는 것이다.__   
    
    - __각 프로세스는 반드시 임계 영역에 접근하기 위해서 진입 허가를 요청해야 한다.__   
    
    <p align="center"><img src="https://github.com/seongbeenkim/CS-Interview/blob/master/OS/image/6_1_GeneralStructureOfProcess.jpg" title="6_1_GeneralStructureOfProcess" alt="6_1_GeneralStructureOfProcess"></img><br><strong>6.1 프로세스의 일반적인 구조</strong></p>   
    
    - __진입 영역(Entry section)__   
      - 이러한 진입 요청을 구현하는 코드 부분   
      
    - __퇴출 영역(Exit section)__   
      - 임계 영역 뒤에 오는 코드 부분   
      
    - __나머지 역(Reminder section)__   
      - 나머지 코드 부분  
    
  - __임계 영역 문제의 해결책이 만족시켜야하는 3가지 요소__ :star:   
    - __상호 배제(Mutual exclusion)__    
      - 하나의 프로세스가 임계 구역에서 실행된다면, 다른 프로세스들은 임계 구역에서 실행될 수 없다.   
    
    - __진행(Progress)__   
      - 임계 영역에서 실행되는 프로세스가 없고 다수의 프로세스가 임계 영역에 진입하기를 원한다면, 나머지 구역에서 실행되고 있지 않은 프로세스들만 어느 프로세스가 다음에 임계 영역으로 진입할 지 결정하는데 참여할 수 있으며 이러한 선택은 무기한적으로 연기될 수 없다.   
      
    - __한정된 대기(Bounded waiting)__   
      - 프로세스가 임계 영역에 진입 요청을 하고 그 요청이 승인되기 전까지 다른 프로세스들이 임계 영역에 진입하도록 허용되는 횟수에 제한이 있어야 한다.   
  
  - __한 순간에 많은 커널 모드 프로세스들이 OS 안에서 활성화 될 수 있다.__   
    - 그 결과, OS를 구현하는 코드(커널 코드)는 다수의 프로세스를 경쟁 상태로 만들 가능성이 있다.   
    
    - __OS에서 임계 영역을 다루기 위한 두 가지 방법__ :star:   
      - __1. 선점형 커널__   
        - 프로세스가 커널 모드에서 실행되는 동안 선점되는 것을 허용한다.   
        - 공유된 커널 자료구조가 경쟁 상태로부터 자유로워야 한다는 것을 보장할 수 있게 설계되어야 한다.   
          - 특히 SMP 구조를 위해 설계하는 것이 어렵다.   
      
      - __2. 비선점형 커널__   
        - 프로세스가 커널 모드에서 실행되는 동안 선점되는 것을 허용하지 않는다.   
          - 커널 모드 프로세스는 커널 모드를 빠져나가거나 block되거나 또는 자발적으로 CPU의 제어를 양보할 때까지 계속 실행된다.   
        - 비선점형 커널은 한 순간에 커널 안에서 실행중인 프로세스는 하나밖에 없기 때문에 커널 자료구조에 대한 경쟁 상태로부터 자유롭다.   
        
      - __선점형 커널을 비선점형 커널보다 선호하는 이유__   
        - 선점형 커널은 응답이 더 빠를 수 있다.    
          - 커널 모드 프로세스가 대기중인 프로세스에게 프로세서를 양도하기 전에 오랫동안 실행할 위험이 적기 때문이다.   
            - 커널 모드 프로세스가 이런 식으로 행동하지 않도록 커널 코드를 설계하여 이러한 위험을 최소화 할 수 있다.   
        - 선점형 커널은 실시간 프로세스가 현재 커널에서 실행중인 프로세스를 선점할 수 있기 때문에 실시간 프로그래밍에 더 적당하다.   
        
### 6.3 Petersons's Solution   

  - __임계 영역 문제를 위한 소프트웨어 기반 해결책__   
    - 하지만, load, store 같은 기계언어 명령어를 수행하는 현대 컴퓨터 구조에서는 정확하게 동작한다는 보장이 없다.   
    
  - __임계 영역과 나머지 영역 사이에서 번갈아 실행되는 프로세스가 2개로 한정된다.__   
    - __두 프로세스는 두 데이터를 공유해야 한다.__   
      - 두 프로세스는 `Pi, Pj`라고 표현하고 `j = i - 1`과 같다고 가정한다.   
      - `turn`   
        - 어느 프로세스가 임계 영역에 진입할 차례인지를 나타낸다.   
          - ex) `if turn == i`   
            - Pi가 임계 영역에서 실행되는 것이 허락된 것이다.      
      - `boolean flag[2]`   
        - 프로세스가 임계 영역에 진입할 준비가 되어있는지를 나타낸다.   
          - ex) `if flag[i] == true`   
            - Pi가 임계 영역에 진입할 준비가 되었다는 뜻이다.   
      
      <p align="center"><img src="https://github.com/seongbeenkim/CS-Interview/blob/master/OS/image/6_2_Petersons.jpg" title="6_2_Petersons" alt="6_2_Petersons"></img><br><strong>6.2 Pi의 피터슨 솔루션</strong></p>   
    
      - __`Pi`이 임계 영역에 진입하기 위해서는 먼저 `flag[i] = true`가 되야하고, `turn = j`로 설정해야 한다.__   
        - `turn = j`로 해주는 이유는 `Pj`가 임계 영역에 진입하기를 바랄 경우, 그렇게 할 수 있게 해주기 위해서이다.   
        - while문이 계속 실행될 경우, `Pi`는 `Pj`가 임계 영역에서 벗어날 때 까지 기다려야한다. = busy wating     
        - `Pi`가 임계 영역을 벗어난 후 `flag[i] = false`는 기다리고 있는 `Pj`가 임계 영역에 진입할 수 있게 한다.   
        
      - __두 프로세스가 동시에 접속하기를 시도할 경우__   
        - __`turn`은 거의 동시에 i, j로 설정된다.__   
          - __이러한 할당의 오직 하나만 지속된다.__   
            - __나머지 하나는 즉시 덮어씌어진다.__   
          - __`turn`의 최후의 값이 두 프로세스중 어느 프로세스가 임계 영역에 먼저 접근할지를 결정한다.__   
          
  - __피터슨 알고리즘이 정확하다고 증명하기 위해서 충족해야 할 3가지__   
    - __상호 배제가 보존되어야 한다.__   
      - `Pi`는 오직 `flag[j] == false` 이거나 `turn == i`일 경우에만, `Pj`는 `flag[i] == false` 이거나 `turn == j`일 경우에만 임계 영역에 접근이 가능하다.   
      - 두 프로세스가 동시에 임계 영역에서 실행되어질 수 있다면, `flag[i] == true`, `flag[j] == true`이다.   
      - 하지만 `Pi, Pj`에서 `turn`의 값이 `i` 또는 `j`로 통일되고 각자 다른 값을 가질 수 없기 때문에, 위의 두 의견은 `Pi`와 `Pj`는 동시에 자신의 while문을 벗어나지 못하게 된다는 것을 암시한다.   
      
      - __예를 들어 `Pi, Pj`가 동시에 실행되었는데 `Pj`가 while문을 벗어나서 임계 영역에 진입했다면, `Pi`는 `flag[j] == true`, `turn == j` 조건에서 계속 block된다.__      
      - __이 시점에서 `Pi`의 `flag[j] == true`, `turn == j`는 `Pj`가 임계 영역에 있는 동안 지속될 것이고 이로 인해 상호 배제는 보존된다.__      
      
    - __진행 요건이 만족되어야 한다.__   
      - __임계 영역에 어떤 프로세스의 접근도 없을 때 항상 접근이 가능해야 한다.__   
      - `flag[j] == true`, `turn == j`여야만 `Pi`가 임계 영역에 진입할 수 없다.   
      - `Pj`가 임계 영역에 진입할 준비가 되어있지 않다면 `flag[j] == false`이고 `Pi`는 임계 영역에 진입할 수 있다.   
      - `Pj`가 `flag[j] = true`로 설정하고 while문을 실행중이라면, `turn == i` 이거나 `turn == j`이다.   
        - `turn == i`일 경우 `Pi`는 임계 영역으로 진입한다.   
        - `turn == j`일 경우 `Pj`가 임계 영역으로 진입한다.   
          - 하지만, `Pj`가 임계 영역을 벗어나면 `flag[j] = false`가 되고 `Pi`가 임계 영역으로 진입할 수 있게 된다.   
          - `Pj`가 `flag[j] = true`로 바꾸면 `turn = i`로 바꿔야만 한다.   
          - `Pi`는 while문을 실행하는 동안 `turn`의 값을 변경하지 않기 때문에, `Pi`는 `Pj`에 의한 최대 한번의 과정 후에 임계 영역으로 진입할 수 있다. = 유한 대기  
    
    - __유한 대기 요건이 충족되어야 한다.__   
      - 무한 대기가 일어나지 않아야 한다.   
        - 일부의 프로세스만 계속 임계 영역에 진입할 경우, 그렇지 못한 프로세스가 생겨 고아 현상이 발생한다.   
    
    
### 6.4 Synchronization Hardware   

  - __Locking을 통해 임계 영역을 보호한다.__   
  
  - __싱글 프로세서에서는 공유되는 변수가 수정되는 동안 인터럽트 발생을 막음으로써 임계 영역을 보호할 수 있다.__   
    - 연속적인 명령어는 선점없이 순서대로 실행되어지기 때문에 예상치 못한 수정이 공유 변수에서 일어날 수 없다.   
    - 비선점 커널에서 종종 이러한 방법을 사용한다.   
    - 멀티 프로세서에서는 메세지가 모든 프로세서에 전달되기 때문에 인터럽트를 disable하는 것은 시간 소모가 커 이러한 방식을 사용하는 것이 거의 불가능하다.   
      - 메세지 전달은 각 임계 영역 진입을 지연시키고 시스템 효율성을 떨어뜨린다.   
      
  - __많은 현대 컴퓨터 시스템은 하나의 word의 내용을 수정하고 테스트하거나 두 개의 word의 내용을 atomically하게 교체하게 하는 특별한 하드웨어 명령어를 제공한다.__   
  
    - __test_and_set()__   
      <p align="center"><img src="https://github.com/seongbeenkim/CS-Interview/blob/master/OS/image/6_4_TestAndSet.jpg" height = 400px title="6_4_TestAndSet" alt="6_4_TestAndSet"></img><br><strong>6.3~4 test_and_set()</strong></p>   

      - test_and_set() 명령어는 atomically하게 실행된다.   
      - __두 개의 test and set()이 다른 CPU에서 각각 동시에 실행되면, 무작위 순서로 연속하여 실행되어진다.__   
      - lock 변수를 선언함으로써 상호 배제를 구현할 수도 있다.   
    
    - __compare_and_swap()__   
      <p align="center"><img src="https://github.com/seongbeenkim/CS-Interview/blob/master/OS/image/6_5_CompareAndSwap.jpg" height = 400px title="6_5_CompareAndSwap" alt="6_5_CompareAndSwap"></img><br><strong>6.5~6 compare_and_swap()</strong></p>   
      
      - test_and_set()과 달리 3개의 피연산자가 필요하다.   
      - 항상 변수 value의 원래의 값만 반환한다.   
      - atomically하게 실행된다.   
      - 상호 배제는 전역 변수 lock을 선언하고 0으로 초기화 함으로써 제공될 수 있다.   
      - compare_and_swap()을 호출하는 첫 번째 프로세스는 lock의 원래 값이 예상된 0의 값과 같았기 때문에 lock을 1로 설정하고 임계 영역으로 진입한다.   
      - 이 후의 compare_and_swap() 호출은 lock이 0과 같지 않기 때문에 실행될 수 없다.   
      - 임계 영역에 진입한 프로세스가 임계 영역을 벗어날 때, lock을 0으로 설정하여 다른 프로세스가 임계 영역으로 진입할 수 있게 한다.   
    
  - __이러한 알고리즘이 상호 배제 요건을 충족하더라도, 유한 대기 요건을 만족시키지 못한다.__   
  
    - __bound-waiting mutual__   
      - test_and_set()을 사용한 또 다른 알고리즘으로 모든 임계 영역 요건을 만족시킨다.   
      - `boolean waiting[n]`과 `boolean lock`은 false로 초기화된다.   
        <p align="center"><img src="https://github.com/seongbeenkim/CS-Interview/blob/master/OS/image/6_7_TestAndSet2.jpg" height = 400px title="6_7_TestAndSet2" alt="6_7_TestAndSet2"></img><br><strong>6.7 test_and_set()을 통한 유한 대기 상호 배제</strong></p>   
          
        - __상호 배제 요건이 충족되는 것을 증명하기 위해서, Pi는 waiting[i] == false 또는 key == false 일 경우에만 임계 영역에 진입할 수 있다고 알아두어야 한다.__   
          - key의 값은 test_and_set()이 실행될 경우에만 false로 변할 수 있다.   
        - test_and_set()을 실행할 첫 번째 프로세스는 key == false 인 것을 발견하고, 다른 프로세스는 모두 기다린다.   
        - waiting[i]는 다른 프로세스가 임계 영역을 벗어날 경우에만 false가 될 수 있다.   
          - 오직 하나의 waiting[i]만 false로 설정되어 있어 상호 배제 요건을 유지한다.   

        - __진행 요건이 충족되는 것을 증명하기 위해서, 임계 영역을 벗어나는 프로세스가 lock을 false로 또는 waiting[i]를 false로 설정하기 때문에 상호 배제를 위해서 표현된 주장들이 여기에서도 적용된다는 것을 알야야 한다.__   
          - lock과 waiting[i] 둘 다 임계 영역에 진입하기 위해 기다리고 있는 프로세스가 계속해서 진행될 수 있게 해준다.   
        - __유한 대기 요건이 충족되는 것을 증명하기 위해서, 프로세스가 임계 영역을 벗어날 때, 순환 순서(i+1, i+2, ..., n , 0, ..., i-1)로 waiting 배열을 스캔한다.__   
          - 이러한 순서에 있는 첫 번째 프로세스가 임계 영역에 진입하기 위한 다음 프로세스처럼 진입 영역에 있다는 것을 명시한다.   
            - 임계 영역에 진입하기 위해 기다리고 있는 모든 프로세스는 n - 1번 안에 진입할 수 있다.   
          
### 6.5 Mutex Locks   

  - __Mutex lock__ :star:   
    - __OS 설계자가 임계 영역 문제를 해결하기 위해서 만든 가장 간단한 소프트웨어적인 동기화 툴__   
    - __임계 영역을 보호하고 race condition을 예방하기 위해 사용한다.__        
      - 프로세스는 임계 영역에 진입하기 전에 lock을 획득해야 하고 벗어날 때 lock을 해제해야한다.   
        - acquire(), release(), available 변수를 활용한다.   
        <p align="center"><img src="https://github.com/seongbeenkim/CS-Interview/blob/master/OS/image/6_8_MutexLocks.jpg" title="6_8_MutexLocks" alt="6_8_MutexLocks"></img><br><strong>6.8 mutex lock을 사용한 임계 영역 문제 해결법</strong></p>   
        
          - __프로세스가 이용가능하지 않은 lock에 대해 acquire()를 할 경우 lock이 해제될 때까지 block된다.__   
    
    - __단점__   
      - __Busy waiting__   
        - __Spinlock__   
          - 하나의 프로세스가 임계 영역에 있는 동안 임계 영역에 진입을 시도하는 다른 프로세스들은 acquire()를 계속 loop를 돌려야한다.   
          - 즉, 프로세스가 lock이 이용가능해질 때까지 계속 loop를 돌려야한다.   
            - ex) 이전의 test_and_set(), compare_and_swap() 명령어의 예제에서도 spinlock이 있다.   
        - Busy waiting은 CPU 사이클을 낭비한다.   
        
    - __장점__    
      - __No context switch__   
        - 프로세스가 lock을 기다려야만 할 때, 문맥 교환이 일어나지 않는다.   
        - 문맥 교환이 일어나는 데 상당한 시간이 소요될 수 있다.   
        - lock은 짧은 시간동안만 점유될 것으로 예상되기 때문에, spinlock이 실용적이다.   
    
    - __다른 스레드가 다른 프로세서에 있는 임계 영역에서 수행되는 동안 하나의 스레드가 하나의 프로세서에서 계속 loop를 돌릴 수 있는 멀티프로세서 시스템에서 사용된다.__   
    
### 6.6 Semaphores   

  - __프로세스들을 Mutex lock보다 더 정교하게 동기화할 수 있는 툴__ :star:   
    - __S__   
      - 정수   
      - __초기화할 때를 제외하고 wait()과 signal()에 의해서만 접근된다.__   
        - __변수 값을 수정하는 wait(), signal() 연산은 모두 원자성을 만족해야 한다.__    
        - 즉, 한 프로세스(또는 스레드)에서 세마포어 값을 변경하는 동안 다른 프로세스가 동시에 이 값을 변경해서는 안 된다.   
      
    - __wait()__   
      - P라는 용어로 불린다.   
      - 임계 영역에 들어가기 전에 수행된다.   
      - _`S >= 0`일 경우에만 `S--`의 기능을 하며 인터럽트 없이 수행이 되어야만 한다.__       
      
    - __signal()__    
      - V라는 용어로 불린다.   
      - 임계 영역에서 벗어날 때 수행된다.   
      - __`S++`의 기능을 한다.__   
      
    <p align="center"><img src="https://github.com/seongbeenkim/CS-Interview/blob/master/OS/image/6_Semaphores.jpg" title="6_Semaphores" alt="6_Semaphores"></img><br><strong>세마포어</strong></p>   
      
#### 6.6.1 Semaphore Usage(세마포어 사용)   

  - __계수형 세마포어(Counting semaphore)__ :star:       
    - __값의 범위가 정해져 있지 없다.__   
    - __유한한 수의 자원에 접근을 통제하기 위해 사용될 수 있다.__   
      - 세마포어는 이용가능한 자원의 수로 초기화된다.   
      - 자원을 사용하고 싶은 각 프로세스는 wait()을 수행하여 세마포어의 값을 1 감소시킨다.   
      - 프로세스가 자원을 해제할 때, signal() 연산을 수행하여 세마포어의 값을 1 증가시킨다.   
      - 세마포어의 값이 0으로 된다는 것은 모든 자원이 사용되고 있다는 것을 의미한다.   
        - 0으로 된 이후, 자원을 사용하고 싶은 프로세스가 wait()을 수행하였을 때, 세모포어의 값이 0보다 커질 때까지 block된다.   

  - __이진 세마포어(Binary semaphore)__ :star:   
    - __값의 범위는 오직 0과 1 사이이다.__   
    - __mutex lock과 비슷하게 동작한다.__   
      - mutex lock을 제공하지 않는 시스템에서 이진 세마포어는 상호 배제를 제공하기 위해 대신 사용될 수 있다.   

#### 6.6.2 Semaphore Implementation(세마포어 구현)   

  - __busy waiting을 극복하기 위해서 wait(), signal() 연산을 수정해야 한다.__   
    - __프로세스가 wait() 연산을 수행하고 세마포어의 값이 양수가 아니라는 것을 발견할 때, 프로세스는 반드시 기다려야만 한다.__   
      - 그러나 busy waiting에 관여되는 것 대신에 프로세스 자체가 스스로 block할 수도 있다.    
      - block 연산은 프로세스를 세마포어와 관련된 대기 큐에 넣고 프로세스의 상태는 대기 상태로 전환된다.   
        - 프로세스에 대한 제어는 CPU 스케줄러로 넘어가서 CPU 스케줄러는 실행할 다른 프로세스를 선택한다.   
      - 세마포어 S를 기다리면서 block이 된 프로세스는 다른 프로세스가 signal() 연산을 실행할 때 재시작된다.   
      - 프로세스는  대기 상태에 있는 프로세스를 준비 상태로 변경하는 wakeup() 연산에 의해 재시작된다.   
        - 프로세스는 준비 큐에 놓여지게된다.(CPU 알고리즘에 따라 실행을 하기 위해 선택될 수도 있고 아닐 수도 있다.)   
        
    - __이러한 위의 정의 아래에서 busy waiting 해결하기 위한 구현 방법__   
      - __세마포어 정의__   
        - __각 세마포어는 정수 값을 가지고 프로세스 리스트를 가진다.__   
          - 프로세스가 세마포어를 기다려야만 할 때, 프로세스의 리스트에 추가된다.   
          - signal() 연산은 기다리는 프로세스 리스트에서 하나의 프로세스를 제거하고 해당 프로세스를 깨운다.   

      - __wait() 정의__   
        - block()연산은 block() 연산을 호출한 프로세스를 중지시킨다.   
        - wakeup(P) 연산은 프로세스 P의 실행을 재개한다.   
        - block(), wakeup() 연산은  OS의 기본적인 시스템 호출로서 제공된다.    
        
      - __이러한 구현 내에서, 세마포어 값은 음수일 수 있다.__   
        - 하지만, busy waiting을 가진 세마포어의 고전적 정의 아래에서는 세마포어 값이 절대 음수일 수가 없다.   
        - 세마포어 값이 음수라면, 세마포어의 음수 값은 해당 세마포어를 기다리고 있는 프로세스의 수이다.   
      
      - __기다리는 프로세스의 리스트는 각 PCB에 있는 link 필드에 의해 쉽게 구현될 수 있다.__   
        - 각 세마포어는 정수 값과 PCB의 리스트에 대한 포인터를 포함한다.   
        - 리스트에서 프로세스를 제거하고 추가하는 방법은 유한 대기가 FIFO 큐를 사용하는 것과 같다.   
          - 하지만 세마포어는 큐에 head 포인터와 tail 포인터 둘 다 있다.   
        - 일반적으로 프로세스 리스트는 모든 큐 방법을 사용할 수 있다.   
          - 세마포어의 정확한 사용은 세마포어 리스트를 위한 특정 큐 방법에 달려있지 않다.   
        
      - __세마포어 연산이 원자적으로 실행되는 것은 중요하다.__   
        - __두 프로세스가 wait()과 signal() 연산을 동시에 같은 세마포어에 실행할 수 없게 보장해야만 한다.__   
          - 임계 영역 문제   
          - __단일 프로세스 환경에서는 wait()과 signal()연산을 실행하는 동안 인터럽트를 금지함으로써 쉽게 해결할 수 있다.__   
            - 인터럽트가 금지되면, 다른 프로세스로부터의 명령어가 개입될 수 없다.   
            - 오직 현재 실행하는 프로세스는 인터럽트가 재활성화될 때까지 실행하고 스케줄러는 제어를 다시 얻을 수 있다.   
          
          - __멀티 프로세스 환경에서는 인터럽트가 모든 프로세서에서 비활성화되어야만 한다.___   
            - 다른 프로세스(다른 프로세서에서 실행되는)부터의 명령어는 무작위로 개입될 수도 있다.   
              - 모든 프로세서에서 인터럽트를 비활성화하는 것은 힘든 작업이 될 수 있고 게다가 성능을 심각하게 약화시킬 수 있다.   
              - 그러므로 SMP 시스템에서는 wait()과 signal()이 원자적이게 수행되게 보장하기 위해서 반드시 대채적인 locking 기법 (compare_and_swap() 또는 spinlock)을 제공해야만 한다.   
              
       - __지금까지 wait()과 signal() 연산의 정의에서 busy waiting을 완전히 제거하지는 못했다.__   
        - 오히려 busy waiting을 진입 영역으로부터 임계 영역으로 옮겨놨다.   
        - busy waiting을 wait()과 signal() 연산의 임계 영역으로 제한해 놓았고 이러한 영역들은 짧다.   
          - 적절하게 프로그램되어 있다면, 약 10개 이하의 명령어만 존재하게 된다.   
        - 게다가 임계 영역은 거의 차지되지 않고, busy waiting은 드물게 일어나며 일어나더라도 아주 짧은 시간동안만 일어난다.   
          - 임계 영역이 길거나 거의 항상 차지되어지는 상황이 존재할 수 있고 이러한 경우에는 busy waiting은 완전히 비효율적이다.   

#### 6.6.3 Deadlocks and Starvation(데드락과 기아 현상)   

  - __대기 큐를 사용한 세마포어의 구현은 두 개 이상의 프로세스가 기다리는 프로세스 중 하나에 의해서만 일어나는 사건을 무기한으로 기다리는 상황을 낳는다.__   
    - 이렇게 무기한으로 기다리게 되는 프로세스를 교착 상태에 빠졌다고 이야기한다.   
    - 교착 상태와 관련된 또 다른 문제는 무한 blocking 또는 기아 현상이다.   
      - 무한 blocking은 세마포어와 관련된 LIFO 구조의 프로세스 리스트로부터 프로세스를 제거할 경우 발생할 수 있다.   
    
#### 6.6.4 Priority Inversion(우선순위 역전)   

  - __더 낮은 우선 순위 프로세스에 의해 접근되고 있는 커널 데이터를 더 높은 우선 순위 프로세스가 수정하거나 읽어야 할 필요가 있을 때, 커널 데이터는 lock으로 보호되어 있기 때문에, 더 높은 우선순위 프로세스는 더 낮은 우선 순위 프로세스가 해당 데이터 사용을 끝내기를 기다려야만 한다.__   
  
  - __우선순위 역전__   
    - ex) 우선 순위가 `L < M < H` 인 프로세스가 있을 경우    
      - __일반적으로, `L`이 사용하고 있는 자원 `R`을 `H`가 필요로 할 때, 원래는 `H`가 `L`이 `R`의 사용을 끝내기를 기다려야 한다.__    
        - 이 상황에서 `M`이 실행 가능한 상태가 된다고 가정한다면 `M`은 `L`을 선점하게 되고, `H`가 자원 `R`을 얻는 데 걸리는 시간에 간접적으로 영향을 미치게 된다.    
        - 이러한 상황을 우선순위 역전이라고 한다.   
    - 두 개 이상의 우선순위가 존재하는 시스템에서만 발생한다.   
    - __해결 방법__    
      - __1. 오직 두 개의 우선순위만 가지게 한다.__    
        - 대다수의 일반용 OS에 적용하기에는 적합하지 않은 방법이다.   
        - __우선순위 상속을 구현함으로써 문제를 해결할 수 있다.__   
          - 높은 우선 순위 프로세스가 요구하는 자원에 접근하고 있는 모든 프로세스는 자원의 사용을 끝낼 때까지 더 높은 우선순위를 상속받는다.   
          - 자원의 사용이 끝나면, 원래의 우선순위로 돌아가게 된다.   
            - ex) `L`은 `H`의 우선순위를 임시로 상속받아, `M`이 자신을 선점하는 것을 막는다.   
              - `L`이 자원 `R`의 사용을 끝냈을 때, 원래의 우선순위로 돌아가게 되고 `R`은 이용가능해지기 때문에 `M`이 아닌 `H`가 `R`을 사용하게 된다.   
             
### 6.7 Classic Problems of Synchronization    

  - __동기화를 위해서 세마포어를 사용하지만 실제 구현에 있어서 이진 세마포어 대신 mutex lock을 사용할 수도 있다.__   

#### 6.7.1 The Bounded-Buffer Problem(유한 버퍼 문제)    

  - __생산자 소비자 프로세스는 아래의 자료구조를 공유한다.__      
    - `int n`   
    - `semaphore mutex = 1;`   
      - 버퍼 풀 접근에 대한 상호 배제를 제공한다.   
    - `semaphore empty = n;`     
    - `semaphore full = 0;`   
    #### 6.9 생산자가 소비자를 위해 버퍼를 생산하는 그림   
    #### 6.10 생산자가 소비자를 위해 버퍼를 생산하는 그림   

#### 6.7.2 The Readers-Writers Problem(저자-독자 문제)    

  - __다수의 병행 프로세스 사이에 공유되는 데이터베이스가 있을 경우__   
    - __Readers__   
      - 데이터베이스 읽기만 하는 프로세스    
    - __Writers__   
      - 데이터베이스 수정도 할 수 있는 프로세스   
  
  - __두 독자가 동시에 공유된 데이터에 접근할 경우__    
    - 어떠한 부정적인 영향도 일어나지 않는다.   
    
  - __하나의 저자와 다른 프로세스들이 동시에 접근할 경우, 문제가 발생하게 된다.__   
    - 이러한 문제를 발생하지 않게 보장하기 위해서, 저자가 데이터베이스를 수정하는 동안에는 데이터베이스에 대한 독점적인 접근을 가져야만 한다.   
  - __저자-독자 문제의 유형__   
  
    - 모두 우선순위와 관련이 있다.    
    - __1. 첫 번째 독자-저자 문제__   
      - 가장 단순한 형태    
      - 저자가 공유 객체에 대한 사용 허가를 가지고 있지 않은 한, 독자는 기다릴 필요가 없다.   
        - 즉, 저자가 공유 객체에 접근하고 있지 않는 한 독자들 사이에서는 서로 기다릴 필요가 없다.      
    
    - __2. 두 번째 독자-저자 문제__    
      - 저자가 준비되면, 저자의 수행은 가능한 한 빨리 수행되야 한다.   
        - 즉, 저자가 공유 객체 접근을 하기 위해 기다리고 있다면, 모든 새로운 독자는 읽기를 시작할 수 없다.   
  
  - __문제 해결을 위한 방법은 모두 기아 현상으로 이어질 수 있다.__   
  
    - 첫 번째 독자-저자 문제에서는, 저자가 기아    
    - 두 번째 독자-저자 문제에서는, 독자가 기아    
    
    - __첫 번째 독자-저자 문제에 대한 해결법__    
      - __독자 프로세스는 아래의 자료 구조를 공유해야 한다.__   
        - `semaphore rw_mutex = 1;`   
          - 독자와 저자 두 프로세스에 모두 속한 세마포어   
          - 저자를 위한 상호배제의 기능을 한다.   
          - 임계 영역에 진입하거나 나가는 첫 번째 또는 마지막 독자에 의해 사용된다.   
        - `semaphore mutex = 1;`   
          - read_count가 수정될 때, 상호배제를 보장하기 위해 사용된다.   
        - `int read_count = 0;`   
          - 얼마나 많은 프로세스가 현재 객체를 읽고 있는지 나타낸다.   
          
        <p align="center"><img src="https://github.com/seongbeenkim/CS-Interview/blob/master/OS/image/6_12_ReaderWriterStructures.jpg" title="6_12_ReaderWriterStructures" alt="6_12_ReaderWriterStructures"></img><br><strong>6.11~12 저자, 독자 프로세스 구조</strong></p>   
        
          - 저자가 임계 영역에 있고, n개의 독자가 기다리고 있으면, 하나의 독자는 rw_mutex에 큐잉되고, n-1개의 독자는 mutex에 큐잉된다.   
          - 저자가 signal(mutex)를 실행할 때, 기다리고 있는 독자들 또는 하나의 저자의 실행이 재개된다.   
            - 독자들 또는 하나의 저자에 대한 선택은 스케줄러에 의해서 결정된다.   
        
        
  - __독자-저자 문제와 해결 방법은 일부 시스템에서 독자-저자 lock을 제공하게 보편화되었다.__   
    - __독자-저자 lock을 얻는 것은 lock의 모드를 구체화하는 것이 필요하다.__    
      - read / write 접근 모드   
    - 프로세스가 공유된 자료를 읽기만을 원할 때, 프로세스는 read 모드에 있는 독자-저자 lock을 요청한다.   
    - 공유된 자료를 수정하기를 원하는 프로세스는 반드시 write 모드에 있는 독자-저자 lock을 요청해야만 한다.   
    - 다수의 프로세스가 read 모드에 있는 독자-저자 lock을 동시에 얻을 수 있다.   
      - 하지만 오직 하나의 프로세스만 write를 위한 lock을 얻을 수 있다.   
      
  - __독자-저자 lock은 아래의 상황에서 굉장히 유용하다.__    
    - 공유된 자료를 읽기만 하는 프로세스와 쓰기만 하는 프로세스를 식별하기 쉬운 app    
    - 저자보다 독자가 많은 app   
      - 독자-저자 lock은 일반적으로 상호 배제 lock 또는 semaphore보다 더 많은 오버헤드를 요구하기 때문이다.    
 
#### 6.7.3 The Dining-Philosophers Problem(철학자들의 만찬)   

  <p align="center"><img src="https://github.com/seongbeenkim/CS-Interview/blob/master/OS/image/6_13_DiningPhilosophers.jpg" title="6_13_DiningPhilosophers" alt="6_13_DiningPhilosophers"></img><br><strong>6.13 철학자들의 만찬</strong></p>   
  
  - __철학자는 혼자서 생각하다가 배고파지면 좌측과 우측의 철학자 사이에 있는 젓가락을 집을 수 있으며 한 번에 하나의 젓가락만 집을 수 있다.__   
    - 다른 철학자가 사용 중인 젓가락은 집을 수 없다.   
    - 배고픈 철학자가 두 개의 젓가락을 가질 때, 음식을 먹을 수 있다.   
    - 음식을 다 먹으면 사용했던 젓가락을 내려놓고 다시 생각을 시작한다.   
      - 내려놓은 젓가락은 다른 철학자가 집을 수 있다.    
      
  - __deadlock과 기아 현상이 없는 방식에서 다수 프로세스 사이에 여러 자원을 할당시키는 간단한 예이다.__    
  
  - __각 젓가락을 세마포어로 표현할 수 있다.__   
    - 철학자는 세마포어에 대한 wait() 연산을 실행함으로써 젓가락을 잡으려고 시도한다.   
    - 적절한 세마포어에 signal() 연산을 실행함으로써 젓가락 두 개를 내려놓는다.    
    - 공유되는 데이터 = `semaphore chopstick[5];`   
      - 모두 1로 초기화된다.    
    <p align="center"><img src="https://github.com/seongbeenkim/CS-Interview/blob/master/OS/image/6_14_Philosopher_i.jpg" title="6_14_Philosopher_i" alt="6_14_Philosopher_i"></img><br><strong>6.14 철학자 i의 구조</strong></p>   
    
    - __이러한 방법은 인접하는 두 철학자가 동시에 먹을 수 없게 하는 것을 보장하지만 deadlock을 생성시킬 수 있기 때문에 반드시 거부되어져야 한다.__   
      - ex) 5명의 철학자 모두가 동시에 배고파지게 되고, 각각이 왼쪽 젓가락을 집는다고 가정할 경우    
        - chopstick의 모든 요소는 0과 같아진다.   
        - 각 철학자가 오른쪽 젓가락을 잡으려고 시도할 때, 다른 철학자가 내려놓기를 평생동안 기다리게 된다.   
        - __deadlock 문제를 해결하기 위한 방법__    
          - __1. 최대 4명의 철학자만 동시에 테이블에 앉을 수 있게 한다.__    
          - __2. 자신의 좌측, 우측 두 젓가락 모두 이용가능할 때만 철학자가 젓가락을 집을 수 있게 한다.__    
          - __3. 홀수번 째의 철학자가 먼저 자신의 왼쪽 젓가락을 집은 후 오른쪽 젓가락을 집게하고, 짝수번 째의 철학자는 자신의 오른쪽 젓가락을 집은 후, 왼쪽 젓가락을 집게 한다.__    
          
  - __철학자들의 만찬 문제에 대한 모든 만족스러운 해결 방안은 반드시 철학자 중 한명이 배고파죽게 되는 상황이 발생하지 않도록 대비해야 한다.__    
  
  - __deadlock이 없는 해결 방안은 기아 현상의 발생 가능성을 반드시 제거할 필요는 없다.__   
  
### 6.8 Monitors    

  - __세마포어는 프로세스 동기화에 있어서 편리하고 효율적인 메카니즘이지만, 잘못 사용하게 되면 감지하기 어려운 타이밍 에러를 일으킬 수 있다.__   
    - __타이밍 에러__   
      - __1. Signal(mutex) - 임계 영역 - Wait(mutex)__   
        - 여러 프로세스가 동시에 자신의 임계 영역에서 실행될 수 있어 상호배제 조건을 위반하게 된다.   
      - __2. Wait(mutex) - 임계 영역 - Wait(mutex)__   
        - deadlock이 발생한다.   
      - __3. Signal(mutex) 또는 Wait(mutex)을 생략할 경우__   
        - 상호배제 조건 위반 또는 deadlock이 발생한다.   
        
    - __이러한 문제를 다루기 위한 더 높은 단계의 동기화 구성 요소를 Monitor라고 한다.__   

#### 6.8.1 Monitor Usage(모니터 사용)    

  - __추상적 자료형(Abstracct Data Type)__    
    - 어떠한 구현도 되어있지 않은 독립적인 데이터를 캡슐화한다.    
      - 데이터는 여러 함수를 포함하고 있다.   
      
    - __모니터의 자료형은 ADT__    
      - 모니터 내에서 상호배제가 제공되는 프로그래머가 정의한 기능들의 집합을 포함한다.   
      - 모니터의 인스턴스 상태를 정의하는 변수를 선언하며, 이 변수에 따라 기능들이 작동한다.   
      <p align="center"><img src="https://github.com/seongbeenkim/CS-Interview/blob/master/OS/image/6_15_Monitor.jpg" height = 400px title="6_15_Monitor" alt="6_15_Monitor"></img><br><strong>6.15 모니터 구문</strong></p>   
      
      - __모니터 내에 정의된 함수들은 모니터 내에 정의된 변수들에만 접근할 수 있다.__   
        - 모니터의 지역 변수들도 모니터의 지역 함수들에 의해서만 접근될 수 있다.   
      - __모니터 구조는 모니터 내에서 한 번에 오직 하나의 프로세스만 active되게 보장한다.__   
      - __위의 모니터 구조만으로는 동기화 모델링 하기에 충분치 않기 때문에 추가적인 동기화 메커니즘인 condition 구조를 정의해야 한다.__    
        - __프로그래머는 Condition 자료형 변수를 하나 이상 정의할 수 있다.__    
          - ex) `condition x, y;`    
        - __condition 변수가 호출하는 연산은 wait()과 signal() 뿐이다.__    
          - signal()은 오직 하나의 중단된 프로세스만 정확하게 재개시킨다.   
            - 어떠한 프로세스도 중단되어 있지 않다면, signal()은 아무런 영향을 끼치지 않는다.   
            - 즉, x의 상태가 signal() 연산을 실행하지 않은 것과 같다.   
            - 반대로, semaphore의 signal()은 항상 영향을 끼친다.   
            <p align="center"><img src="https://github.com/seongbeenkim/CS-Interview/blob/master/OS/image/6_17_MonitorConditions.jpg" height = 400px title="6_17_MonitorConditions" alt="6_17_MonitorConditions"></img><br><strong>6.17 condition 변수를 가진 모니터</strong></p>   
            
            
        - __프로세스 P가 x.signal()을 호출하고 x와 관련된 프로세스 Q가 중단되어 존재할 경우__      
          - __중단되어 있는 Q는 자신의 실행을 재개하는 것이 허락된다면, signal() 호출한 P는 반드시 기다려야만 한다.__   
            - 그렇지 않으면, P와 Q 둘 다 모니터 내에서 동시에 active 하게 된다.   
            - __이럴 경우 개념적으로 두 프로세스가 자신의 실행을 진행 할 수 있게 되는데 두 가지의 가능성이 존재한다.__   
              - __1. Signal and wait__   
                - P가 Q가 모니터를 벗어날 때까지 기다리거나, 다른 condition을 기다린다.   
              - __2. Signal and continue__   
                - Q가 P가 모니터를 벗어날 때까지 기다리거나, 다른 condition을 기다린다.   
              
              - P가 이미 모니터에서 실행되고 있었기 때문에, 2번의 방법이 더 합리적인 것처럼 보인다.   
              - __두 방법의 절충안은 Pascal 언어에서 채택되었고, P가 signal하면 즉시 모니터를 벗어나고 Q가 즉시 재개된다.__    
                
### 6.10 Alternative Approaches   
#### 6.10.1 Transactional Memory(트랜잭션 메모리)    
  
  - __메모리 트랜잭션__ :star:      
    - __일련의 메모리 읽기-쓰기 연산__   
    - __트랜잭션 내 모든 연산이 완료되어야만, 메모리 트랜잭션이 커밋된다.__    
      - 그렇지 않을 경우, 중단(abort) 되거나 이전으로 roll back 된다.   
    - 트랜잭션 메모리의 이점은 프로그래밍 언어에 추가된 특성들을 통해서 얻어질 수 있다.   
    - __개발자나 트랜잭션 메모리 시스템이 원자성을 보장해야할 필요가 없다.__   
    - __어떠한 lock도 관련되지 않기 때문에, deadlock이 발생할 가능성이 없다.__   
    - 원자성 block안에 있는 어떤 명령문이 병행하게 실행될 수 있을 지 확인할 수 있게 해준다.   
      - ex) 공유 변수에 대한 병행 읽기 접근   
      - 프로그래머가 이러한 상황을 알 수 있게 해주며 독자-저자 lock을 사용할 수 있게 해준다.   
        - 하지만, app내의 스레드의 수가 증가할수록 작업은 훨씬 어려워지게 된다.   
    
  - __트랜잭션 메모리 구현 방법__   
    - __1. Softwar Transactional Memory(STM)__   
      - __특수한 하드웨어의 도움없이 소프트웨어만으로 트랜잭션 메모리를 구현한다.__   
      - __트랜잭션 block내에 instrmentation 코드를 삽입함으로써 STM이 수행된다.__   
      - 코드는 컴파일러에 의해 삽입되고 어느 명령문이 병행하게 실행할 수 있을지, 어떤 low-level locking이 요구되는지를 검사함으로써 각 트랜잭션을 관리한다.   
    
    - __2. Hardware Transactional Memory(HTM)__   
      - __하드웨어 캐시 계층 구조와 캐시 일관성 프로토콜을 사용하여 별도의 프로세서 캐시에 존재하는 공유 데이터와 관련된 문제들을 해결하고 관리한다.__   
      - 특별한 코드 instrmentation이 필요하지 않아 STM보다 오버헤드가 적다.   
        - 하지만, 트랜잭션 메모리를 지원할 수 있게 수정된 캐시 일관성 프로토콜, 캐시 계층 구조가 필요하다.   

#### 6.10.2 OpenMP   

  - __OpenMP는 컴파일러 지시자와 API를 포함한다.__   
    - OpenMP의 장점으로는 스레드 생성과 관리가 OpenMP 라이브러리에 의해 다뤄져 개발자의 부담이 없다.   
 
  - __OpenMP는 스레드가 경쟁 상태를 발생시키지 않는 것을 보장한다.__    
    - #pragma omp critical은 한 번에 하나의 스레드만 실행되는 임계 영역 명령어 코드 영역를 나타낸다.   
    
  - __표준 mutex lock보다 사용하기 더 쉽지만 단점으로는 개발자가 반드시 발생가능한 경쟁 상태를 확인해야 하고, 컴파일러 명령어를 사용하여 공유 데이터를 적절하게 보호해야 한다.__    
    - 임계 영역 명령어는 mutex lock처럼 행동하기 때문에, 두 개 이상의 임계 영역이 확인 될 때 deadlock이 발생할 가능성이 있다.    
    
#### 6.10.3 Functional Programming Language(실용적인 프로그래밍 언어)   

  - __기능적인 프로그래밍 언어는 상태를 유지하지 않는다.__   
    - 변수가 한번 정의되고 값이 할당되면, 값을 더 이상 변경할 수 없다.   
    - 기능적인 프로그래밍 언어는 변경가능한 상태를 허락하지 않기 때문에, 경쟁 상태와 deadlock과 같은 문제들을 고려할 필요가 없다.   
      - ex) Erlang, Scala   
