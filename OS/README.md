# :bookmark_tabs: Operating System Concepts 9th edition      

* [1.Introduction](#1-introduction)   
   - [1.1 What Operating Systems do](#11-what-operating-systems-do)   
      - [1.1.1 User View(사용자 관점)](#111-user-view사용자-관점)   
      - [1.1.2 System View(시스템 관점)](#112-system-view시스템-관점)   
      - [1.1.3 Defining Operating Systems(OS 정의)](#113-defining-operating-systemsos-정의)   
   - [1.1 What Operating Systems do](#11-what-operating-systems-do)   
      - [1.1.1 User View(사용자 관점)](#111-user-view사용자-관점)   
      - [1.1.2 System View(시스템 관점)](#112-system-view시스템-관점)   
* [2.](#2)   
   - [](#)   
   - [](#)   
   - [](#)   
   - [](#)   
   
   
## 1. Introduction   
   - __Operating System__   
      - 컴퓨터의 하드웨어를 관리하는 프로그램   
      - 컴퓨터 사용자와 하드웨어 사이의 중개자 역할을 하며 응용프로그램을 위한 기초를 제공   
      - OS mainframe은 주로 하드웨어의 이용률을 최대한으로 하기 위해 설계되었다.   
 
### 1.1 What Operating Systems do   
   - __컴퓨터 시스템을 대략 4가지 구성요소로 나눌 수 있다.__   
      - __Hardware__    
         - 시스템을 위한 기본적인 컴퓨터 자원들을 제공한다.    
         - CPU(Central Processing Unit), Memory, I/O devices   
      - __Application programs__   
         - 사용자의 컴퓨팅 문제를 해결하기 위해 자원들을 사용하는 방식을 정의한다.   
         - ex) Word processor, spreadsheets...   
      - __Operating system__   
         - 하드웨어를 통제하고 다양한 사용자들을 위한 다양한 app 사이에 하드웨어 사용을 조정한다.  
      - __Users__   
      
#### 1.1.1 User View(사용자 관점)   
   - __사용자를 위한 시스템은 보통 한 유저가 시스템 자원을 독점할 수 있게 설계되어진다.__   
      - __목표는 사용자가 실행하는 작업을 최대화하는 것이다.__   
      - __이 경우, OS는 자원의 이용보다 성능에 중점을 두면서 대부분 사용의 용의함을 위해 설계된다.__   
      - __다수 사용자의 요구 사항보다 한 사용자에 경험에 더 최적화 되어있다.__   
   
   - __사용자는 mainframe 또는 minicomputer에 연결된 터미널에 있을 수 있다.__   
      - __다른 사용자들은 다른 터미널을 통해 같은 컴퓨터에 접근한다.__    
         - __사용자들은 자원을 공유하고 정보를 교환할 수도 있다.__   
      - __이 경우, OS는 자원 활용을 극대화하기 위해 설계되어있다.__   
      
   - __사용자는 서버와 다른 워크스테이션의 네트워크에 연결된 워크스테이션에 있을 수 있다.__    
      - __사용자가 마음대로 사용할 수 있게 자원들을 할당한다.__   
      - __파일, 프린트 서버를 포함하는 서버와 네트워킹같은 자원을 공유한다.__   
    
#### 1.1.2 System View(시스템 관점)   
   - __OS는 하드웨어와 가장 밀접하게 연관된 프로그램이며 자원 관리자의 역할을 한다.__   
   - __OS는 다양한 사용자 프로그램과 I/O 장치를 통제하기 위한 Control Program이다.__   
   - __Control program__   
      - 컴퓨터의 부적절한 사용과 에러를 예방하기 위해 사용자 프로그램의 실행을 관리한다.   

#### 1.1.3 Defining Operating Systems(OS 정의)   
   - __일반적인 OS의 정의는 컴퓨터에서 항상 실행되는 하나의 프로그램이다. = Kernel   
   - __커널의 2가지 종류 프로그램__   
      - __System program__   
         - OS와 관련이 있지만 커널의 필수적인 부분은 아니다.   
      - __Application program__   
         - 시스템의 작동과 관련되지 않은 모든 프로그램    
   - __Mobile OS는 핵심 커널뿐만 아니라 middleware도 포함할때도 있다.__   
      - __Middleware__   
         - app 개발자에게 추가적인 서비스를 제공해주는 소프트웨어 프레임워크의 집합   
         - ex) IOS, Android   

### 1.2 Computer-System Organization(컴퓨터 시스템 구성)    
   - 

####
