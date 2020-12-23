# :bookmark_tabs: Operating System Concepts 9th edition      
## 8장 Memory-Management Strategies   
__중요하다고 생각되는 목차에는 :star: 표시해놓았습니다.__   
__:star:되어 있는 목차를 클릭하시면 클릭하신 목차의 내용이 있는 페이지로 넘어가며__   
__해당 페이지 내에 있는 중요 개념 옆에 :star: 표시해놓았습니다.__   

__혹시 잘못된 내용이 있거나 보완해야할 점이 있으면 `issue` 해주시거나 알려주시면 감사하겠습니다.:bow:__   

* [8.Memory-Management Strategies](#8memory-management-strategies)   
  - [8.1 Background](#81-background)   
    - [8.1.1 Basic Hardware](#811-basic-hardware기본-하드웨어)   
  - [](#)   
  - [](#)   
  - [](#)   
  - [](#)   
  - [](#)   
  - [](#)   
  - [](#)   
  - [](#)   
  - [](#)   
  - [](#)   
  - [](#)   
  - [](#)   

         
## 8.Memory-Management Strategies   
### 8.1 Background   

  - __Memory__   
    - 각자 자신의 고유 주소를 가진 byte의 거대한 배열   
  
  - __CPU__   
    - PC(Program Counter)의 값에 따라 메모리로부터 명령어를 가져온다.   
    - 이러한 명령어는 특정 메모리 주소에 추가적인 자료를 저장하거나 불러올 수 있게 할 수 있다.

  - __명령어 실행 사이클__   
    __1. 메모리로부터 명령어를 fetch한다.__   
    __2. 명령어가 해석되고 메모리로부터 fetch되어질 피연산자를 가져온다.__   
    __3. 명령어가 해당 피연산자를 연산한다.__   
    __4. 연산된 결과는 메모리에 저장된다.__   
    
  - __메모리 unit은 오직 메모리의 주소 스트림만 본다.__   
    - 어떻게 생성되는지 또는 무엇을 위한 것인지 알 수 없다.   
    - 그러므로 프로그램이 어떻게 메모리 주소를 생성하지는지에 대해 상관할 필요가 없다.   
    - 오직 실행되는 프로그램에 의해 생성된 메모리 주소의 결과에 대해서만 관심을 가지면 된다.   
    
#### 8.1.1 Basic Hardware(기본 하드웨어)   
  - ____   
  
