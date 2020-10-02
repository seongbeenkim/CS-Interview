# :bookmark_tabs: Operating System Concepts 9th edition      
## 5장 Process Scheduling   
__중요하다고 생각되는 목차에는 :star: 표시해놓았습니다.__   
__:star:되어 있는 목차를 클릭하시면 클릭하신 목차의 내용이 있는 페이지로 넘어가며__   
__해당 페이지 내에 있는 중요 개념 옆에 :star: 표시해놓았습니다.__   

__혹시 잘못된 내용이 있거나 보완해야할 점이 있으면 `issue` 해주시거나 알려주시면 감사하겠습니다.:bow:__   

* [5.Process Scheduling](#4process-scheduling)   
  - [5.1 Basic Concepts](#51-basic-concepts)   
    - [5.1.1 CPU-I/O Burst Cycle(CPU 입출력 버스트 사이클)](#511-cpu-io-burst-cyclecpu-입출력-버스트-사이클)   
    - [5.1.2 CPU Scheduler(CPU 스케줄러)](#512-cpu-schedulercpu-스케줄러)   
  - [](#)   
  - [](#)   
  - [](#)   
  - [](#)   
  - [](#)   
  - [](#)   
  
## 5.Process Scheduling   
### 5.1 Basic Concepts   

  - __멀티 프로그래밍은 CPU의 대기 시간을 활용하기 위해 한 순간에 다수의 프로세스들을 메모리에 저장시킨다.__   
    - 하나의 프로세스가 기다려야만 할 때, OS는 해당 프로세스를 CPU에서 제거하고 다른 프로세스에 CPU를 할당한다.   
    
#### 5.1.1 CPU-I/O Burst Cycle(CPU 입출력 버스트 사이클)   

  - __프로세스 실행은 CPU 실행과 입출력 사이클로 구성된다.__   
    - 프로세스들은 두 상태 사이에서 변경된다.   
    - 프로세스 실행은 CPU 버스트로 시작하고 입출력 버스트 - > CPU 버스트 -> 입출력 버스트 순으로 계속된다.   
      - 마지막 CPU 버스트는 실행 종료를 위한 시스템 요청과 함께 끝난다.   
    - 입출력 중심 프로그램은 짧은 CPU 버스트를 많이 가진다.   
    - CPU 중심 프로그램은 다수의 긴 CPU 버스트를 가진다.   
    - 이러한 버스트의 분포는 CPU 스케줄링 알고리즘을 선택하는데 매우 중요하다.   

#### 5.1.2 CPU Scheduler(CPU 스케줄러)   

  - ____   
    - 
    - 
