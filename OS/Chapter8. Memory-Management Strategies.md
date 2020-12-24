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

  - __메인 메모리와 프로세서(CPU)에 내장되어 있는 레지스터들은 CPU가 직접 접근할 수 있는 유일한 저장 장치이다.__   
    - 모든 실행되는 명령어와 자료들은 CPU가 직접 접근할 수 있는 메인 메모리와 레지스터에 있어야 한다.   
    - 만약 자료가 메모리에 없다면 CPU가 처리하기 전에 메모리로 적재해야 한다.   

  - __CPU에 내장된 레지스터들은 일반적으로 CPU clock의 1 사이클 내에 접근가능하다.__   
    - __대부분의 CPU는 명령어를 해석하고 clock마다 하나 이상의 수행을 할 수 있는 속도로 레지스터 내용물에 대한 작업을 수행할 수 있다.__   
    - __하지만 메모리 버스에 있는 트랙잭션을 통해 접근되는 메인 메모리의 경우에는 똑같이 수행된다고 말할 수 없다.__   
      - 메모리에 접근하는 것은 CPU clock의 많은 사이클이 소요된다.   
      - 실행되고 있는 명령어를 완료하기 위해서 요구되는 데이터를 가지고 있지 않기 때문에 지연(stall)현상이 발생한다.   
      - 이러한 경우에 메모리 접근 빈도가 잦아지기 때문에 견딜수 없는 상황이 된다.   
    
    - __해결법은 CPU와 메인 메모리 사이에 빠른 메모리를 추가하는 방법이 있다. = Cache__   
      - 일반적으로 빠른 접근을 위해서 CPU 칩에 추가된다.   
      - CPU에 추가된 Cache를 관리하기 위해서 하드웨어는 자동적으로 OS 통제없이 메모리 접근 속도를 높인다.  
      
  - __물리적 메모리 접근 속도뿐만 아니라 정확한 수행을 보장해야 한다.__   
    - 반드시 사용자 프로세스에 의한 접근으로부터 OS를 보호해야 한다.   
    - 다수 사용자 시스템에서,, 반드시 다른 사용자 프로세스로부터 자신의 프로세스를 보호해야 한다.   
    - 이러한 보호는 반드시 하드웨어에 의해 제공되야 한다.   
      - OS는 일반적으로 CPU와 CPU의 메모리 접근사이에 간섭을 하지 않기 때문이다.   
    - __하드웨어가 이러한 수행을 할 수 있게 구현하는 방법__   
      - __각 프로세스가 독립된 메모리 공간을 가지게 해야 한다.__   
      - __독립된 프로세스 메모리 공간은 병행 수행을 위해 여러 프로세스를 메모리에 적재하는데 있어 필수적이다.__   
      - __각 프로세스는 적합한(legal) 메모리 주소 영역을 설정하고 접근한다.__
      - __메모리 공간을 분리하기 위해서, 프로세스가 접근할 수도 있는 적합한 주소의 범위를 알아내고 오직 이런 적합한 주소에만 접근할 수 있게 보장하는 능력이 필요하다.__   
      - __이러한 보호는 base, limit 레지스터를 사용함으로써 제공할 수 있다.   
      [#8.1 base, limit 레지스터 사진 첨부]   
      - __Base register__   
        - 가장 작은 적합한 물리적 메모리 주소를 담고 있다.   
      - __Limit register__   
        - 범위의 크기를 나타낸다.   
    [#8.2 base, limit 레지스터를 통한 메모리 주소 보호 사진 첨부]   
    - __메모리 공간 보호는 CPU 하드웨어가 사용자 모드에서 만들어진 모든 주소와 레지스터를 비교함으로써 수행된다.__   
      - 사용자 모드에서 실행되는 프로그램이 OS 메모리 또는 다른 사용자 메모리에 접근하려고 할 경우, 오류(Fatal error)로 간주하여 OS에 트랩(trap)을 발생 시킨다.   
      - 이러한 구조는 사용자 프로그램이 고의적으로 또는 우연히 OS나 다른 사용자의 데이터 구조 또는 코드를 수정하는 것을 방지한다.   
    - __Base 레지스터와 limit 레지스터는 특별 권한 명령어을 사용하는 OS에 의해서만 불려올 수 있다.__   
      - 특별 권한 명령어는 커널 모드에서만 실행될 수 있고, OS는 커널 모드에서만 실행되기 때문에 OS가 base와 limit 레지스터를 불러올 수 있다.__     
    - __이러한 구조는 OS가 레지스터의 값을 변경하게 하지만, 사용자 프로그램이 레지스터의 내용을 변경하게 하는 것을 막아준다.__   
  
  - __커널 모드에서 실행하는 OS는 OS 메모리와 사용자 메모리 모두에 비제한적인 접근을 가진다.__   
    - 사용자 프로그램을 사용자 메모리에 적재할 수 있다.   
    - 에러가 날 경우 해당 사용자 프로그램들을 덤프할 수 있다.   
    - 시스템 호출의 매개변수 값을 변경하거나 접근할 수 있다.   
    - 사용자 메모리로부터 입출력을 수행할 수 있다.   
    - __예를 들어 멀티프로세싱 시스템을 위한 OS가 반드시 문맥 교환을 해야할 경우, 다음 실행될 프로세스의 문맥이 메인 메모리로부터 레지스터로 적재되기전에 현재 실행되고 있는 프로세스 상태를 레지스터로부터 메인 메모리로 저장해야 한다.__   