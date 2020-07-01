# :bookmark_tabs: Network Interview Questions   
   
* [REST API](#rest-api란-무엇인가)    
* [라우터에서 발생하는 Packet delay](#라우터에서-패킷-교환간-발생하는-delay)   
* [쿠키,세션,캐시](#쿠키세션캐시)   
* [HTTP와 HTTPS](#http와-https)   
* [TCP 3-way-handshake와 4-way-handshake](#tcp-3-way-handshake와-4-way-handshake)   
* [브로드캐스트, 멀티캐스트, 유니캐스트](#브로드캐스트-멀티캐스트-유니캐스트)   
* [도메인과 DNS](#도메인과-dns)      
* [정적 라우팅, 동적 라우팅](#정적-라우팅-동적-라우팅)    
* [TCP와 UDP의 차이점](#tcp와-udp의-차이점)   
* [OSI 7 Layer](#osi-7-layer)   
* [라우팅과 포워딩](#라우팅과-포워딩)   
* [ARQ](#arq)   
* [TCP 흐름제어, 혼잡제어](#tcp-흐름제어-혼잡제어)   
* [TCP 타이머 종류](#tcp-타이머-종류)   
* [GET, POST](get-post)   
* [:star:네트워크 주소 체계 - Line, Naver](#네트워크-주소-체계)   
* [Packet switching, Circuit switching](#packet-switching-circuit-switching)   
* [NAT](#nat)   
* [](#)   
* [](#)   
* [](#)   


## REST API란 무엇인가?       
  - __서버에 HTTP 요청을 보낼 때 각 요청이 어떤 동작이나 정보를 위한 것인지 그 요청의 모습 자체로 추론 가능한 API__   
  - 과거의 SOAP(Simple Object Access Protocol)이란 복잡한 형식 대체   
    - __SOAP__   
      - 일반적으로 널리 알려진 HTTP, HTTPS, SMTP 등을 통해 XML 기반의 메시지를 컴퓨터 네트워크 상에서 교환하는 프로토콜이다. SOAP은 웹 서비스에서 기본적인 메시지를 전달하는 기반이 된다.   
      - SOAP에는 몇가지 형태의 메시지 패턴이 있지만, 보통의 경우 원격 프로시져 호출(Remote Procedure Call:RPC) 패턴으로, 네트워크 노드(클라이언트)에서 다른 쪽 노드(서버)로 메시지를 요청 하고, 서버는 메시지를 즉시 응답하게 된다.    

  - __프로그램을 만들 때 기능 자체만 중요하게 생각한다면 REST를 생각할 필요 없이 동작만 되게 하면 된다.__   
    - Ex) 어떤 학원의 반과 학생들에 대한 API를 만들 때    
    - `https://(도메인)/1` = 학원의 반 리스트 요청   
    - `https://(도메인)/hello` = 반의 학생들 리스트 요청   
    - `https://(도메인)/seongbeen` = 학생의 정보 수정 요청   
    
  - __하지만 REST를 왜 사용하는가?__   
    - 서비스를 혼자 만드는 것이 아니기 때문이다. 위에 처럼 기능 자체만 중요하게 만들 경우 당장은 혼자라도, 나중에 이 일을 인계받을 개발자나, 해당 API를 사용해서 다른 제품을 만들 개발자들은 일하기 힘들어진다.   
    - 그러므로 RESTful하게 만든 API는 요청 보내는 주소만으로 어떤 요청인지 파악이 가능하게 하여 다른 개발자들이 일을 더 편하게 할 수 있게 해준다.    
    - Ex) 
    - `https://(도메인)/classes` => 학원의 반 목록 요청 추론 가능   
    - `https://(도메인)/classes/2` => 2반에 관한 정보 요청 추론 가능 (초급반, 중급반 등)   
    - `https://(도메인)/classes/2/students` => 2반 학생들에 대한 정보 요청 추론 가능   
    - `https://(도메인)/classes/2/students/15` => 2반의 15번째 학생에 대한 정보 요청 추론 가능   
    - `https://(도메인)/classes/2/students?sex=male` => 2반의 남학생에 대한 정보 요청 추론 가능   
    - `https://(도메인)/classes/2/students?page=2&count=10` => 2반의 2 page의 학생 10명에 대한 정보 요청 추론 가능   

    __여기서 `classes/2/students`와 같이 자원을 구조와 같이 나타내는 형태의 구분자를 “URI”라고 한다.__   

  - 위처럼 조회 작업 뿐만 아니라 정보를 새로 넣거나 수정, 삭제를 할 경우도 필요하다.   
    - Create(생성), Read(조회), Update(수정), Delete(삭제)   

  - __서버에 REST API로 요청을 보낼 경우 HTTP 규약에 따라 신호를 전송한다.__    
    - HTTP로 요청을 보낼 경우 여러 메소드들이 사용되는데 __REST API는 GET, POST, PUT(또는 PATCH), DELETE를 사용__ 한다.   
    - __POST, PUT, PATCH에는 body라는 컨테이너가 있어 GET이나 DELETE보다 비교적 안전하게 데이터를 감춰서 실어보낼 수 있다.__   
    - __이 기능들은 특정 용도에 제한되어 있지는 않다.__   
      - Ex) POST로 데이터를 CRUD를 모두 다 할 수 있다. 용량이 적고 중요도가 낮은 정보들은 GET으로 CRUD 모두 할 수 있다.   
    - __하지만, 누구든 각 요청의 의도를 쉽게 파악할 수 있도록 RESTful하게 API를 만들기 위해서는 이러한 기능들을 목적에 따라 구분해서 사용해야 한다.__   
    - __GET__   
      - Read(조회)   
      - Ex)   
      - GET `https://(도메인)/classes/2/students` => 2반 학생들을 조회하는 요청   
    - __POST__   
      - Create(생성)   
      - Ex)   
        - POST `https://(도메인)/classes/2/students` 일 경우   
        - POST의 body에 새 학생에 대한 정보를 담아 보낸다. => 2반에 새로운 학생 데이터 추가 요청 (idx가 생성될 것이기 때문에 idx를 표시할 필요가 없다)
    - __PUT 또는 PATCH__   
      - Update(수정)   
      - Ex)    
        - PUT 또는 PATCH `https://(도메인)/classes/2/students/14` 일 경우   
        - PUT 또는 PATCH의 body에 수정할 정보들을 담아 보낸다. => 2반의 14번 학생 데이터 수정 요청   
      - __PUT으로 모두 사용하는 경우도 있지만 정석으로는 PUT은 보통 정보를 완전히 수정할 때 사용하며 PATCH는 정보의 일부분만 수정할 때 사용해야한다.__   
    - __DELETE__   
      - Delete(삭제)    
      - Ex) DELETE `https://(도메인)/classes/2/students/14` = 2반 14번 학생에 대한 데이터 삭제 요청   

    - __위에서 말했다시피 POST 또는 GET이 CRUD 모두 할 수 있다고 하였는데 이렇게 사용해야 할 경우 URI에 이에 대한 동작도 명시해야 한다. 이 경우는 RESTful 하지 못한 경우이다.__    
      - Ex)   
      - POST `https://(도메인)/classes/2/students/create`   
      - POST `https://(도메인)/classes/2/students/read`   
      - POST `https://(도메인)/classes/2/students/update`   
      - POST `https://(도메인)/classes/2/students/delete`   
    - 이럴 경우 깔끔하지가 않기 때문에 이런 것들을 지양하기 위한 __REST의 규칙 중 하나로 URI는 동사가 아닌 명사__ 로 이뤄저야 한다.   
    
    - __REST 특징__   
      - __Stateless (무상태성)__   
        - 세션 정보나 쿠키 정보를 별도로 저장하고 관리하지 않기 때문에 API 서버는 들어오는 요청만을 단순히 처리한다.   
      - __Uniform Interface (유니폼 인터페이스)__   
        - URI로 지정한 자원에 대한 조작을 통일되고 한정적인 인터페이스로 수행하는 아키텍처 스타일을 말한다.   
      - __Cacheable (캐시 가능)__   
        - HTTP가 가진 캐싱 기능이 적용 가능합니다. HTTP 프로토콜 표준에서 사용하는 Last-Modified태그나 E-Tag를 이용하면 캐싱 구현이 가능하다.   
      - __Self-descriptiveness (자체 표현 구조)__   
        - REST API 메시지만 보고도 이를 쉽게 이해 할 수 있는 자체 표현 구조   
      - __Client - Server 구조__   
        - 자원이 있는 쪽을 Server이고 자원 요청하는 쪽이 Client    
        - Server와 Client에서 개발해야 할 내용이 명확해지고 서로간 의존성이 줄어들게 된다.   
      - __Layered system (계층형 구조)__   
        - REST 서버는 다중 계층으로 구성될 수 있으며 보안, 로드 밸런싱, 암호화 계층을 추가해 구조상의 유연성을 둘 수 있고 PROXY, 게이트웨이 같은 네트워크 기반의 중간매체를 사용할 수 있게 한다.   
        - Client는 Rest API 서버만 호출   
        
  __즉, REST API란, HTTP 요청을 보낼 때 URI에 어떤 메소드(자원 등)를 사용할 지 개발자들 사이에 널리 지켜지는 약속이다.__    
  
  __형식이기 때문에 기술 또는 프레임워크에 구애받지 않는다.__   
  
  __앱을 만들든, 웹사이트를 만들든 어떤 언어로 써서 만들든 소프트웨어간 HTTP로 정보를 주고 받는 부분이 있다면 이러한 규칙을 준수해서 RESTful한 서비스를 만들 수 있다.__   
  
  
## 라우터에서 패킷 교환간 발생하는 delay      
  - __1. Nodal processing delay(처리 지연)__   
     - 라우터에서 패킷을 받았을 때 검사(최종 목적지가 어디인지 다음 라우터는 무엇인지)하고 output link를 결정하는 등 작업을 하는데 걸리는 delay   
     
  - __2. Queuing delay(큐잉 지연)__   
    - output link에 전송될 때까지 큐에서 대기하는 시간, 라우터의 혼잡도에 따라 다르다.   
    
  - __3. Transmission delay(전송 지연)__   
    - 전송 시 걸리는 delay, output link로 첫번째 비트가 나가는 순간부터 마지막 비트가 나가는 순간까지 걸리는 시간   
    - R = 링크 대역폭(bps), L = 패킷 크기(bits), L/R = 비트를 링크로 보내는데 걸리는 시간(전송 지연)   
    - R이 클수록 transmission delay가 적게 걸린다.   

  - __4. Propagation delay(전파 지연)__   
    - 데이터가 output link로 전달된 후 다음 라우터까지 도달하는데 걸리는 시간    
    - d = 물리적 링크의 길이, s = 전파 속도, propagation delay = d/s   
    - s는 전파이기 때문에 조절할 수 없으며, d가 짧을 수록 propagation delay가 적게 걸린다.   
    
  - __delay 줄이는 방법__   
    - __1. Nodal processing delay(처리 지연)__   
      - 좋은 성능의 라우터를 사용   
    - __2. Queuing delay(큐잉 지연)__   
      - x, 인터넷 사용량이 항상 다르기 때문이다.   
    - __3. Transmission delay(전송 지연)__   
      - 링크 대역폭을 확장   
    - __4. Propagation delay(전파 지연)__   
      - 물리적 링크 변경

  - __큐잉 지연과 패킷 손실__   
    - __queue의 크기는 제한적이기 때문에 꽉 찬 상태에서 계속하여 패킷이 온다면 패킷 손실이 발생한다.__   
      - __TCP에서 패킷 손실이 일어날 경우 어떻게 재전송을 하는가?__   
        1. 이전 라우터가 직접 재전송하는 방법
        2. 처음부터 재전송   
        - __2번의 방법으로 처음부터 재전송을 한다. -> 현재 인터넷 디자인__   
        - __1번의 방법을 사용하지 않는 이유__ - 라우터는 빠른 속도로 전달해야하기 위해 해야할 작업이 많으므로 이 전달을 위한 목적으로만 사용을 하기 때문이다.   
        - dumb core: TCP 사이의 모든 라우터를 지칭 (TCP - 라우터 - 라우터 - 라우터 ... - TCP)

## 쿠키,세션,캐시       
   - __쿠키와 세션을 사용하는 이유__   
      - HTTP 프로토콜의 특징이자 약점을 보완하기 위해서 사용된다.   
      - __Connectionless 프로토콜 (비연결지향)__   
        - 클라이언트가 서버에 요청(Request)을 했을 때, 그 요청에 맞는 응답(Response)을 보낸 후 연결을 끊는 처리방식이다.   
        - HTTP 1.1 버전에서 연결을 유지하고, 재활용 하는 기능이 Default로 추가되었다.(keep-alive 값으로 변경 가능)   
      - __Stateless 프로토콜 (상태정보 유지 안함)__   
        - 클라이언트의 상태 정보를 가지지 않는 서버 처리 방식이다.   
        - 클라이언트와 첫번째 통신에서 데이터를 주고 받았다 해도, 두번째 통신에서 이전 데이터를 유지하지 않는다.   
        
      - __서버와 클라이언트가 통신을 할 때 통신이 연속적으로 이어지지 않고 한 번 통신이 되면 끊어진다. 따라서 서버는 클라이언트가 누구인지 계속 인증을 해줘야 한다. 하지만 그것은 매우 귀찮고 번거로운 일이다. 또한 웹페이지의 로딩을 느리게 만드는 요인이 되기도 한다. 그런 번거로움을 해결하는 방법이 바로 쿠키와 세션이다.__ 
      
   - __쿠키__   
      - HTTP의 일종으로 사용자가 어떠한 웹 사이트를 방문할 경우, 사이트가 사용하고 있는 서버에서 사용자의 컴퓨터에 저장하는 작은 기록 정보 파일     
      - HTTP에서 클라이언트의 상태 정보를 클라이언트의 PC에 저장하였다가 필요시 정보를 참조하거나 재사용할 수 있다.   
      - 사용자의 편의를 위하되 지워지거나 조작되거나 가로채이더라도 큰 문제가 없는 데이터를 저장하는데 사용한다.     
      - __특징__   
         - 이름, 값, 만료일(저장 기간 설정), 경로 정보로 구성   
         - 클라이언트에 총 300개의 쿠키 저장 가능
         - 하나의 도메인 당 20개의 쿠키   
         - 하나의 쿠키는 4KB(=4096byte)까지 저장 가능
    
      - __동작 순서__   
         - 클라이언트가 페이지 요청(웹 사이트 접근)    
         - 서버가 쿠키 생성   
         - 생성한 쿠키에 정보를 담아 HTTP Response 할 때 클라이언트에게 보내준다.   
         - 클라이언트는 자신의 PC에 쿠키를 저장한 후 나중에 서버에 요청할 때 쿠키를 함께 전송한다.   

         - ex) 사이트 다시 방문 시 ID/PASSWORD 자동 입력, 팝업 창 "오늘 이 창을 다시 보지 않기", 비회원의 장바구니   
         
      __User-server state: cookies__   
         - 많은 웹 사이트들은 쿠키를 사용한다.   
         - __4가지 구성요소__   
            - HTTP 응답 메시지의 쿠키 헤더 라인   
            - 다음 HTTP  요청 메시지의 쿠키 헤더 라인    
            - 쿠키 파일은 사용자의 브라우저에 의해 관리되고 사용자의 호스트에 보관된다.   
            - 웹 사이트의 백엔드 데이터베이스   

         - 어떻게 "state"를 유지할까?   
            - protocol endpoints: 여러 트랜잭션에서 송/수신자 상태를 유지   
            - cookies: http 메시지는 state를 나타낸다(carry)   
      
      
   - __세션__   
      - 일정 시간동안 같은 사용자(브라우저)로부터 들어오는 일련의 요구를 하나의 상태로 보고, 그 상태를 일정하게 유지시키는 기술   
      - 사용자가 웹 서버에 접속해 있는 상태를 하나의 단위로 보고 그것을 세션이라고 한다.   
      - 사용자나 다른 누군가에게 노출되어서는 안되는 서비스 제공자가 직접 관리해야할 정보를 저장하는데 사용한다.    
    
      - __특징__   
         - 서버에 웹 컨테이너의 상태를 유지하기 위한 정보를 저장   
         - 서버의 저장되는 쿠키 (= 세션 쿠키)   
         - 브라우저를 닫거나, 서버에서 세션을 삭제했을 때만 삭제가 되므로 쿠키보다 보안이 좋다.   
         - 저장 데이터에 제한이 없다.(서버 용량)   
         - 각 클라이언트에 고유 세션 ID를 부여하여 각 클라이언트 요구에 맞는 서비스 제공한다.   
      
         - __동작 순서__   
            - 클라이언트가 페이지 요청(웹 사이트 접근)   
            - 서버는 클라이언트의 Request-header 필드인 쿠키를 확인하여, 세션 ID를 보냈는지 확인   
            - 세션 ID가 존재하지 않다면, 서버는 세션 ID를 생성하여 클라이언트에게 전달   
            - 서버에서 클라이언트로 준 세션 ID를 쿠키를 사용해 서버에 저장     
            - 쿠키 이름 : JSESSIONID(세션을 유지하기 위해 발급하는 키)   
            - 클라이언트는 재접속 시 JSESSIONID를 이용하여 세션 ID값을 서버에 전달   
            - ex) 화면 전환을 하더라도 로그아웃하기 전까지 로그인 상태 유지   
    
   - __세션을 쓰면 되는데 쿠키도 사용하는 이유?__   
      - 세션이 쿠키에 비해 보안도 높은 편이나 쿠키를 사용하는 이유는 세션은 서버에 저장되고, 서버 자원을 사용하기 때문에 사용자가 많을 경우 소모되는 자원이 상당하다.   
      - 이러한 자원관리 차원에서 쿠키와 세션을 적절한 요소 및 기능에 병행 사용하여, 서버 자원의 낭비를 방지하며 웹사이트의 속도를 높일 수 있다.   
 
   - __쿠키와 세션의 차이점__   
      - __저장 위치__   
         - 쿠키 : 클라이언트 PC   
         - 세션 : 서버   
      
      - __보안__   
         - 쿠키 : 클라이언트 로컬에 저장되기 때문에 변질되거나 request에서 Sniffing(네트워크상에서 자신이 아닌 다른 상대방들의 패킷 교환을 훔쳐보는 행위) 당할 우려가 있어서 보안에 취약   
         - 세션 : 쿠키를 이용해서 sessionid 만 저장하고 그것으로 구분해서 서버에서 처리하기 때문에 서버에 직접 접근하지 않는 이상 세션 내의 데이터를 탈취하는 것은 어려우므로 비교적 보안이 좋다.      
      
      - __라이프 사이클(만료 시점)__   
         - 쿠키 : 브라우저가 종료되도, 만료시점이 지나지 않으면 삭제가 되지 않는다.   
         - 세션 : 만료시점을 정할 수 있지만 만료시점 상관없이 브라우저 종료 시 삭제된다.   
      
      - __속도__   
         - 쿠키 : 쿠키에 정보가 들어있기 때문에 서버에 요청시 속도가 빠르다.   
         - 세션 : 정보가 서버에 있기 때문에 처리가 요구되어 비교적 느린 속도를 낸다.   

   - __캐시__   
      - 캐시는 이미지나 css, js파일 등 용량 큰 파일이나 홈페이지 관리자가 설정한 값이 사용자의 브라우저에 저장이 되는 것으로 페이지 로딩 속도를 개선시켜준다.   
      - 같은 홈페이지를 접속하게 되면 css, js, 이미지 파일을 서버를 거치지 않고 사용자의 PC에서 가져오게 된다.   
      - 이를 통해 자원을 아낄 수 있지만 서버에서 자원을 변경하였는데 사용자에게는 변경된 자원이 보이지 않을 경우가 발생할 수 있다. 즉, 브라우저에 이미 캐시되어 있는 경우 문제가 발생할 수 있다.  
        - 사용자의 브라우저의 캐시를 지워주거나(Ctrl + F5) 서버에서 클라이언트로 응답을 보낼 때 header에 캐시 만료시간을 명시하는 방법 등을 이용하여 해결할 수 있다.   
        
      - __Web caches__      
         - 목표: Origin Server를 거치지 않고 Client request에 대한 response를 해주는 것   
         - 사용자 브라우저를 설정: Proxy server에 요청된 내용들에 대한 캐시를 통한 웹 접근   
         - __Proxy Server__   
            - 클라이언트가 자신을 통해서 다른 네트워크 서비스에 간접적으로 접속할 수 있게 해주는 컴퓨터   
            - __서버와 클라이언트 사이에서 중계기로서 통신을 수행하는 기능을 가리켜 proxy__ 라고 한다.   
         - 브라우저는 모든 HTTP 요청을 캐시로 보낸다   
            - 캐시의 객체: 캐시는 객체를 반환   
            - 저장되어 있는 데이터가 없을 경우 캐시는 원래 서버에서 객체를 요청한 다음 객체를 클라이언트에게 반환   
         - __Origin server를 통해 생긴 데이터(캐시)는 Proxy server에 저장되고 같은 request를 받았을 경우 origin server를 거치지 않고 Proxy server에서 데이터를 response한다.__   
         - 캐시는 웹 페이지를 디스플레이하기 위해 다운로드한 데이터의 콜렉션이다.   
         - __Why Web caching?__    
            - 클라이언트 요청에 대한 응답 시간을 줄임   
            - 기관의 접근 링크에서 트래픽을 줄인다   
            - 캐시가 있는 인터넷 밀도: "poor" 컨텐츠 제공 업체가 효과적으로 컨텐츠를 제공 할 수 있다(P2P 파일 공유도 마찬가지)   
            - bandwidth 사용을 줄인다.   
            - access link 속도를 늘리는 것보다 비용이 적게 들고 빠르다.   
            
         - __웹 캐시 종류__   
            - __브라우저 캐시__   
               - 클라이언트 app이 내부적으로 가지고 있는 캐시    
            - __프록시 캐시__    
               - 실제 서버가 있는 곳이 아닌 네트워크 관리자에 의해 네트워크 상에 설치되는 캐시로 latency(지연)와 트래픽을 줄이는데 이용된다.   
            - __게이트웨이 캐시__   
               - 서버의 관리자에 의해 설치 및 운용되는 캐시    
               - 서버의 앞단에 설치되어 요청에 대한 캐시 및 효율적인 분배를 통해 서버의 응답 성능을 좋게 한다.   
          
          
   - __캐시와 쿠키의 차이__   
      - __쿠키__   
         - 정보를 사용자의 PC 임시 저장소에 저장   
         - 사용자가 임의로 저장   
         - 사용자와 관련된 정보 저장    
            - 비밀번호, 방문 날짜 등   
      - __캐시__   
         - 정보를 웹 서버의 저장소에 저장   
         - 사용자의 의지와 관계없이 무조건 자동 저장   
            - ex) 웹 사이트에서 영상 처음 보면 오래걸리는데, 다시 보면 빨리 로딩됨   

      
        
## HTTP와 HTTPS    
   - __HTTP(Hypertext Transfer Protocol)__   
      - __Hypertext__   
         - 링크를 가진 text   
      - Web의 어플리케이션 계층의 프로토콜   
         - 웹브라우저(Client)와 서버(Server)간의 웹페이지 같은 자원을 주고 받을 때 쓰는 통신 규약   
         - 누군가 네트워크에서 신호를 가로채어 본다면 내용이 노출된다.   
      - __Client/Server 모델__   
         - Client: HTTP를 사용하여 request, receive하는 browser이며 Web의 객체들을 보여준다.   
         - Server: Web Server가 HTTP를 사용하여 request에 응답하는 객체를 보낸다.   
      - __stateless__   
         - 과거 client의 request에 대한 정보를 유지하지 않는다.   
         
      __HTTP request, response 하기 전에 TCP 연결을 먼저 해야한다.__   

      - __non-persistent HTTP__   
         - TCP를 연결을 통해 최대 하나의 객체가 전달된 후 TCP 연결이 끊긴다. 다운로드하는 다수의 객체는 다수의 TCP 연결이 필요하다.   
         - HTTP 1.0이 사용   
         - RTT: 작은 패킷이 client에서 server로 갔다 다시 올 동안의 시간   
         - non-persistent HTTP response time = 2RTT + 파일 전송 시간
         - ex) TCP 연결을 위한 RTT 하나, HTTP request와 반환 받아야할 response를 위한 RTT 하나, file 전송 시간   
         - non-persistent HTTP issues:   
            - object 당 2번의 RTT가 걸림   
            - 각 TCP 연결을 위한 OS 오버헤드   
            - 브라우저가 참조된 object를 가져오기 위해 종종 병렬 TCP 연결을 연다   
      
      - __persistent HTTP__: 하나의 TCP 연결을 통해 다수의 객체가 전달될 수 있다.   
         - HTTP 1.1이 사용   
         - Pipelining 기법: 다수의 객체를 요청할 경우 요청할 수 만큼 연속적으로 request를 하고 연속적으로 response 받는 방식을 사용하여 client와 server간 요청과 응답의 효율성을 높인다.   
      
      __일반 web은 persistent HTTP pipelining 기법을 사용한다__   
    
  - __HTTPS(Hypertext Transfer Protocol + Secure Sockets)__   
    - HTTP는 패킷 캡쳐시 정보가 그대로 노출될 수 있는 문제점을 가지고 있어 이를 보완하기 위해 HTTP에 SSL(Secure Socket Layer)을 추가한 통신 규약   
    - 공개키 암호화를 사용한다.   
    - __공개키 암호화(Public Key Cryptography)__   
      - __모든 사람은 공개키와 해독키를 가진다.__   
      - __수신자와 송신자가 암호키를 공유하지 않는다.__   
      - __공개키는 모두가 알고 있다.(Public)__   
      - __수신자만 해독키를 알고 있다.(Private)__   
      - ex) Diffie-Hellman76(수학적 증명 X), __RSA78__   
      
      - __Public key encryption algorithms__   
         - K+(m) = 메세지 암호화 Public, K-(m) = 메시지 복호화 Private      
         - K-(K+(m)) = m   
         - K+는 K-를 연산하는 것이 불가능하다.   
         - RSA(Rivest, Shamir, Adelson algorithm)   
      - 어떤 키를 먼저 적용시키던간에 결과는 같다.   
         - K-(K+(m)) = m = K+(K-(m))   
         - __수학적 연산이 많아 CPU 이용량이 많다. 그러므로 공개키를 사용해 대칭키를 생성하고 대칭키를 통해 메세지 전체를 암호화하여 전송한다.__    
         
    - __SSL: Secure Sockets Layer__   
      - __광범위하게 사용되는 보안 프로토콜(Widely deployed security protocol)__    
         - 거의 모든 브라우저와 웹 서버에서 지원된다.   
         - HTTPS   
         - SSL로 연간 수백억원이 사용된다.   
      - __특징__   
         - 기밀성(Confidentiality)   
         - 무결성(Integrity)   
         - 인증(Authentication)   
      - __목표__   
         - 암호화(특히 신용카드 번호)   
         - 웹 서버 인증   
         - 선택적인 고객 인증   
         - 새로운 상인이 사업을 하는데 작업의 최소화   
      - __모든 TCP Application에서 이용가능하다.__   
      
   - __SSL과 TCP/IP__   
      - 일반 Application은 APP, TCP, IP 계층을 가진다.   
         - HTTP의 메세지를 TCP Socket에 전달   
      - 보안에 대한 기능 SSL을 추가하면 APP, SSL, TCP, IP 4가지 계층을 가지게 된다.   
         - HTTP의 메세지를 SLL 라이브러리를 사용하여 암호화하여 TCP Socket에 전달 = HTTPS   
         - SSL은 실제 계층은 아니다.   
         - TLS(Transport Layer Security)라고도 불린다.   
         - Application에 API를 제공한다.   
         - C and Java SSL libraries/classes를 쉽게 이용할 수 있다.   
         
## TCP 3-way-handshake와 4-way-handshake   
   - __TCP 3-way-handshake__   
      - TCP 통신을 이용하여 데이터를 전송하기 위해 서버와 클라이언트를 연결하는 과정   
         - 3번의 패킷 교환을 한다.   
      - __동작 과정__   
         1. __Client -> Server : SYN__
            - Client가 TCP header의 SYN에 시퀀스 번호를 임의적으로 설정하고 Server에 전송   
            - ex) SYN = 1   
         2. __Server -> Client : SYN + ACK__   
            - Server는 SYN에 시퀀스 번호를 임의적으로 설정하고, Client의 SYN에 대한 ACK(Client의 SYN + 1)를 Client에 전송   
            - ex) SYN = 100, ACK = 2   
         3. __Client -> Server : ACK__   
            - Client는 Server의 SYN에 대한 ACK(Server의 SYN + 1)을 Server에 전송   
            - ex) ACK = 101   
            
         __마지막 3번째 과정에서 Segment에 데이터를 넣어 같이 전송할 수도 있다.__   
    
   - __TCP 4-way-handshake__   
      - TCP 연결 해제 과정   
      - __동작 과정__   
         1. __Client -> Server : FIN__   
            - Close를 원하는 Client가 FIN 시퀀스 번호를 임의적으로 설정하여 Server에 전송하고 FIN_WAIT_1 상태로 대기한다.   
            - ex) FIN = 1   
            
         2. __Server -> Client : ACK__   
            - Server가 Client가 보낸 FIN을 받을 경우 CLOSE_WAIT 상태로 바꾸고 FIN에 대한 ACK(Client의 FIN + 1)를 Client에 전송하고 보내야 할 데이터가 있을 경우 모두 보낸다.   
            - Client는 Server가 보낸 ACK를 받을 경우 FIN_WAIT_2 상태로 변경된다.   
            - ex) ACK = 2   
            
         3. __Server -> Clinet : FIN__   
            - Server는 데이터가 모두 전송이 되었으면 연결 종료 요청에 합의한다는 의미로 FIN 시퀀스 번호를 임의적으로 설정하여 Client에 전송하고 LASK_ACK 상태로 변경된다.   
            - ex) FIN = 100    
         
         4. __Client -> Server : ACK__   
            - Client는 FIN을 받으면 time wait을 실행하고 FIN에 대한 ACK(Server FIN + 1)를 Server에 전송한다.   
            - ex) ACK = 101   
            - Server가 ACK를 받으면 port를 CLOSED 하고 (Client는 TIME_WAIT에서 일정 시간이 지나면 CLOSED가 된다)   
            
         __3단계에서 Server에 보낸 ACK가 유실될 경우 Server는 ACK을 받지 못하고 time out이 되어 Client에 FIN을 다시 전송할 수 있는데 Client가 연결 해제되어있을 경우 Server는 계속해서 FIN을 보내게 될 수도 있기 때문에 ACK가 확실히 전달되기 위해서 Client는 바로 연결 해제 하지 않고 time wait을 한다.__    
         
         
## 브로드캐스트, 멀티캐스트, 유니캐스트   
   - __브로드캐스트__   
      - __UDP 기반으로 자신의 호스트가 속해 있는 네트워크 서식지에 있는 모든 PC들에게 데이터를 전달__ 하는 통신 방식   
         - 목적지 MAC 주소를 FF-FF-FF-FF-FF-FF로 채워서 보낸다.   
      - 패킷, 프레임을 받는 PC의 MAC 주소가 실제 프레임에 적혀있는 MAC 주소와 일치하지 않더라도 폐기하지 않고 CPU에게 인터럽트를 걸고 우선적으로 받은 패킷을 처리하게 한다.   
   - __멀티캐스트__   
      - __UDP 기반으로 하나 이상의 송신자들이 특정한 그룹에 속해있는 하나 이상의 데이터를 수신하기를 원하는 수신자들에게 데이터를 전송__ 하는 방식   
      - 특정한 그룹을 선택하는 기준은 D클래스의 IP 주소 이용   
      - D클래스는 32bit 주소체계에서 상위 4개의 비트가 "1110" 으로 고정되어 있으며 하위 28bit 가 그룹의 ID 로 사용되는 특별한 주소형식을 가지는데 리를 10진수로 표현하여 보면 224.000.000.000 ~ 239.255.255.255 로 표현할 수 있음.		
   - __유니캐스트__    
      - __TCP 기반으로 1:1로 데이터를 전달하는 통신 방식__   
      - __MAC 기반으로 수신자 IP 주소를 목적지로 하는 일대일 통신 방식__   
      - __구체적으로 데이터를 보내는 PC는 자신의 MAC 주소를 적고 받는 쪽 PC의 MAC 주소도 적어 프레임에 감싸 데이터를 전달한다.__   
      
      
 ## 도메인과 DNS   
   - __도메인__   
      - __식별하기 어려운 IP 주소(예:240.10.20.1)를 example.com 처럼 기억하기 쉽게 만들어주는 네트워크 호스트 이름__ 을 의미   
      - __보통 루트 네임 서버(최상위 DNS서버 이며 IANA 에서 관리한다)에 등록된 최상위 호스트 네임 및 각 최상위 호스트 네임을 관리하는 도메인 레지스트리에서 관리하는 하위 호스트 네임__ 을 뜻한다.      
      - __최상위 호스트 네임은 최상위 도메인 이라고 부르며 해당 레지스트리에 등록된 하위 호스트 네임들은 '.'으로 구분 된 호스트가 얼마나 붙었는지에 따라 2차 도메인, 3차 도메인__ 등으로 불린다.   
      - ex) `krnic.co.kr` 이라는 도메인   
         - kr은 최상위 도메인(또는 1차 도메인)   
         - co는 2차 도메인   
         - krnic은 3차 도메인이라고도 부르지만 보통 등록명으로 불린다.   
         
      - __대한민국의 경우 .kr 이라는 ccTLD(country code Top Level Domain)를 부여받아 KRNIC(실질적으로는 한국인터넷진흥원 인터넷주소센터)에서 관리하고 있으며 KRNIC WHOIS에서 .kr 도메인의 정보를 조회할 수 있다. 여기서 다른 레지스트리(Regisrty)의 정보도 일부 검색할 수 있지만, 검색이 어려울 경우 해당 최상위 도메인(TLD)의 레지스트리(Registry)가 운영하는 공식 WHOIS를 이용하는 것이 좋다.__   
         
   - __DNS__     
      - Domain Name System   
      - __많은 name 서버의 계층 구조로 구현된 분산 데이터 베이스__   
      - __host가 분산 데이터베이스(name servers)로 질의하여 host name에서 IP 주소를 획득하는 어플리케이션 계층 프로토콜__   
      - __host name을 IP 주소로 변환__   
      - host aliasing   
         - 간단한 별칭 호스트 네임을 복잡한 정식 호스트 네임으로 변환   
      - canonical, alias names   
      - canonical names => cname   
      - mail server aliasing   
      - load distribution   
         - 중복 웹 서버(replicated Web server): 많은 IP 주소는 하나의 정식 호스트 네임에 대응된다.   
         - 중복 웹 서버에서는 서버들이 부하 분산   
      - __DNS 메세지 종류__   
         - DNS 질의 메세지(2개 영역만 있음) = Header + 질의   
         - DNS 응답 메세지(5개 영역 있음) = Header + ( 질의 + 응답 + 책임 + 부가정보 )   

      - __질의/응답 동작 형태__   
        - __DNS 클라이언트__   
           - 설정되어진 local name server에게 질의/응답이라는 단 2개의 DNS 메세지로 통신   
        - __DNS 네임서버__   
           - 질의를 위임받은 local name server는 다른 name server와 재귀적/반복적 방법을 통해 원하는 호스트의 IP 주소 등을 얻고 이를 요청한 Client에게 알려준다.   

      - __centralize DNS__      
         - __단점__   
            - single point of failure   
               - 단일 중앙 집중 방식이기 때문에 서버 고장 시 모든 서비스가 동작하지 않는다.      
            - traffic volume   
            - distant centralized database   
               - 거리가 먼 호스트가 접근할 경우 느릴 수도 있음   
            - doesn't scale   
               - scale ability 불가, 즉 규모가 커지지 않는다.      
         - __계층화로 해결할 수 있다.__   
         - __장점__   
            - 단일 중앙 집중 방식이기 때문에 관리가 쉽다.       
            - 최적의 결과를 낼 수 있다.   
      - __Distributed hierarchical database__   
         - __Client가 IP 찾는 과정__   
            1 - Client가 `www.amazon.com`의 IP를 원한다.   
            2 - Client가 __Local DNS Server__ 에 쿼리하여 `www.amazon.com`의 IP 주소를 가지고 있는지 확인(없다면 다음 단계 진행)   
            3 - Client가 __Root server__ 에 쿼리하여 __.com DNS server(TLD)__ 를 찾는다.   
            4 - Client가 __.com DNS server__ 에 쿼리하여 __amazon.com DNS server(Authoritative)__ 를 찾는다.   
            5 - Client는 __amazon.com DNS server__ 에 쿼리하여 __`www.amazon.com`의 IP 주소__ 를 얻는다.      

            - __DNS name resolution example__   
               - 1~4 모든 과정은 각 과정에서 해결이 안됐을 시 진행된다고 가정한다. 해당 과정에서 IP 주소 발견하면 IP 주소 반환한다.   
               - __반복적 질의(iterative query)__   
                  - 질의된 도메인에 대해 응답하거나, 아니면 이 작업을 할 수 있는 다른 DNS 서버에 Client를 연결시켜 주는 작업      
                     - 자신이 관리하지 않는 질의 요청에 대해 응답 가능한 name server 목록 전달   
                     - Client는 다수의 DNS 서버들에게 같은 질의를 반복할 수 있게 된다.   
                  - __Local DNS server가 상위 DNS 서버에게 쿼리를 보내어 IP 주소를 구하는 작업__   

                  1 - 호스트가 local DNS 서버한테 도메인의 IP 주소 질의   
                  2 - 모를 경우 local DNS server가 root DNS server한테 질의   
                  3 - 모를 경우 local DNS server가 TLD DNS server에 질의    
                  4 - 모를 경우 local DNS server가 authoritative DNS 서버에 질의   
                  5 - 모를 경우 에러 메세지 반환   
                  5-1 - 찾을 경우 authoritative response 반환하고 local DNS server는 도메인 IP 주소를 캐싱하고 Client server에 도메인의 IP 주소 전달   
                  - __장점__   
                     - local DNS가 데이터를 많이 가지고 있어 빠르게 받을 수 있다.   
                  - __단점__   
                     - local DNS가 바쁘다.   

               - __재귀적 질의(recursive query)__   
                  - 질의된 도메인에 대해 즉각 응답하거나, 다른 DNS 서버에게 질의한 결과로 응답하거나, 찾고 있는 정보가 없다는 에러 메시지를 보내준다.         
                     - 연결된 name server에 name 확인 부담을 준다   

                  - 최종 응답이 Client에 반환될 때까지 상위 DNS 서버를 쿼리하도록 설정된 DNS 서버로부터 정보를 요청할 때 발생   
                  1 - 호스트가 local DNS 서버한테 도메인의 IP 주소 질의   
                  2 - 모를 경우 local DNS server가 root DNS server한테 질의   
                  3 - 모를 경우 root DNS server가 TLD DNS server에 질의    
                  4 - 모를 경우 TLD DNS server가 authoritative DNS 서버에 질의   
                  5 - 모를 경우 에러 메세지 반환   
                  5-1 - 찾을 경우 authoritative response 반환하고 local DNS server는 도메인 IP 주소를 캐싱하고 Client server에 도메인의 IP 주소 전달   
                  - __단점__   
                     - 상위 DNS 계층 서버에 많은 부담을 준다.   
                     
               - __재귀적 질의 및 반복적 질의 선택은 DNS 해석기가 요청할 때 이를 결정한다.__   
                  - DNS header 내 flag 필드에 표시를 한다.   
                  
         - __Local DNS name server__   
            - DNS 계층에 속하지 않는다.   
            - 각 ISP(가정 ISP, 회사, 대학)는 local DNS 서버(default name server)를 가진다.   
            - 호스트가 DNS 쿼리를 만들면 쿼리가 로컬 DNS 서버로 전송된다.   
               - 최근 name-address 변환 쌍인 local 캐시를 가진다.   
               - host와 DNS 계층 사이에 proxy 역할을 하며 쿼리를 계층으로 전달    

         - __Root name servers__   
            - 번역되지 않는 name을 root name server에 질의   
            - 인터넷의 DNS Root 영역의 name server다.   
            - 루트 영역의 레코드 요청에 직접 답하고, 적절한 최상위 도메인(TLD)에 대한 권한 있는 name server 목록을 반환하여 다른 요청에 응답한다.   
            - Root name server는 사람이 판독 가능한 호스트 name을 인터넷 호스트 간의 통신에 사용되는 IP주소로 번역하는 첫 번째 단계이기 때문에 인터넷 인프라의 중요한 부분이다.   
            
         - __TLD & authoritative servers__   
            - __TLD(Top-Level Domain) server__   
               - com, org, net, edu, aero, jobs, museums 및 모든 top-level country domains 예: uk, fr, ca, jp   
               - Network Solution 회사가 com의 TLD 서버를 관리   
                  ex) Educause가 edu의 TLD 서버 관리   
            - __Authoritative DNS servers__  
               - 조직 자체의 DNS server, 조직의 명명된 호스트에 대한 권한있는 호스트 이름 대 IP 매핑 제공   
               - 조직이나 서비스 제공 업체가 유지할 수 있다.   
               - 권한있는 server는 해당 영역의 권한이다. DNS의 다른 name server에서 쿼리   
               
         - __DNS caching__   
            - __DNS name server가 한 번 요청된 DNS 요청을 일정 시간(TTL)만큼 메모리에 저장하여 뒀다가, 똑같은 요청이 들어오면 신속히 처리할 수 있도록 하는 기능__   
               - 사실상 모든 Name server가 cache를 할 수 있다.   
            - __TTL(Time-to-Live)__
               - 컴퓨터나 네트워크에서 데이터의 유효 기간을 나타내기 위한 방법   
               - Counter나 타임스탬프의 형태로 데이터에 포함되며, 정해진 유효기간이 지나면 데이터는 폐기된다.   
               - 컴퓨터 네트워크에서 TTL은 패킷의 무한 순환을 방지하는 역할을 한다.   
               - 컴퓨터 애플리케이션에서 TTL은 캐시의 성능이나 프라이버시 수준을 향상시키는 데에 사용되기도 한다.   
            - __name server가 IP 주소를 찾을 경우 캐싱한다.__   
               - 일정 시간(TTL) 후 캐시 항목은 사라진다.   
               - 일반적으로 local name servers에 TLD servers이 캐싱된다. 따라서 root name server는 자주 방문하지 않는다   
               - 캐시된 항목은 최신 버전이 아닐 수 있다.   
                  - name host가 IP 주소를 변경하는 경우, 모든 TTL이 만료될 때까지 인터넷 전체에 알려지지 않을 수 있다.   
         - __DNS 응답 종류__   
            - __Authoritive Answer__   
               - 질의된 도메인의 네임서버에서 직접 찾은 결과   
            - __Non-authoritive Answer__    
               - 질의를 위임받은 local name server의 캐시에서 찾은 결과 응답   
         - 다수의 name server가 있을 경우 RTT가 가장 짧은 name server에 DNS 질의 메세지를 보낸다.   


## 정적 라우팅, 동적 라우팅   
   - __라우팅__   
      - __한 네트워크에서 다른 네트워크로 최적의 경로를 선택하여 패킷을 전달하기 위해 네트워크 계층 장치에 의해 수행되는 프로세스__   
   - __정적 라우팅 (Static Routing)__   
      - __관리자가 라우팅 테이블에 경로를 수동으로 추가__   
         - 소규모 네트워크에 적합   
      - __장점__   
         - 라우터 CPU에 라우팅 오버 헤드가 없으므로 더 저렴한 라우터를 사용하여 라우팅을 수행 할 수 있다.   
         - 관리자만 특정 네트워크로의 라우팅 만 허용하므로 보안이 추가된다.   
         - 라우터간에 대역폭 사용이 없다.   
         
      - __단점__   
         - 대규모 네트워크의 경우 관리자가 각 라우터의 라우팅 테이블에서 네트워크에 대한 각 경로를 수동으로 추가하는 것은 리소스가 많이 투입되는 작업   
         - 관리자는 토폴로지에 대해 잘 알고 있어야합니다. 새 관리자가 오면 각 경로를 수동으로 추가해야하므로 토폴로지 경로에 대해 잘 알고 있어야한다.   
         - 네트워크의 변화가 빈번한 경우, 등록해야 할 네트워크의 수가 많을 경우에는 능동적으로 경로 설정을 변경하기 어렵고, 관리자의 네트워크에 대한 설정 및 운영지식을 습득해야 함   
         
   - __동적 라우팅 (Dynamic Routing)__   
      - __라우팅 테이블에서 경로의 현재 상태에 따라 경로를 자동으로 조정__   
         - 중간 ~ 대규모 네트워크에 적합   
         - 하나의 경로가 다운되면 네트워크 대상에 도달하도록 자동 조정   
         - ex) RIP 및 OSPF   
      - __라우터는 경로를 교환하기 위해 동일한 동적 프로토콜을 실행해야한다.__   
      - __라우터가 토폴로지에서 변경 사항을 찾으면 라우터는 이를 다른 모든 라우터에 알린다.__   
      - __장점__   
         - 라우터 간에 서로 Routing 정보를 주고 받아 Routing Table을 라우터가 자동으로 작성하기 때문에 Network 관리자는 초기 설정만 해주면 되므로 구성이 쉽다.      
         - 대상 원격 네트워크에 대한 최상의 경로를 선택하고 원격 네트워크를 검색하는 데 더 효과적입니다.   
         - Network의 변화에 능동적으로 대처할 수 있다.  
      - __단점__   
         - 다른 장비들과 통신하기 위해 더 많은 대역폭을 소비   
         - 정적 라우팅보다 안전하지 않다.   


## TCP와 UDP의 차이점   
   - __TCP__    
      - 소켓 1:1 연결 O   
      - 신뢰성있고 순서대로 데이터 전달   
      - 데이터 손실시 재전송   
      - 데이터의 경계 존재 X   
      - 흐름 제어(슬라이딩 윈도우) - 수신자의 수신 속도에 맞춰 송신자가 데이터 전달   
      - 혼잡 제어 - 네트워크 혼잡 시 송신자의 데이터 전송률을 줄임    
      - 3-way handshaking   
      - 신뢰성있게 데이터를 송신해야 하는 대부분의 프로토콜과 어플리케이션    
         - 파일/메시지 전송 프로토콜 포함   
      - 소형 ~ 초대형 데이터(최대 수 기가 바이트)   
      - ex) 멀티미디어 app, DNS, BOOTP, DHCP, TFTP, SNMP, RIP, NFS(초기버전)   
      
   - __UDP__   
      - 소켓 1:N 방식, 연결 X   
      - no 신뢰성, 순서, 흐름 제어, 혼잡 제어, 재전송   
      - 데이터의 경계 존재 O, 한번의 전송할 수 있는 데이터 크기 제한   
         - 전달 속도가 중요하고, 소량의 데이터를 송신하고 멀티캐스트/브로드캐스트를 사용하는 어플리케이션   
      - 소형 ~ 중형 데이터(최대 수백 바이트)   
      - ex) FTP, Telnet, SMTP, DNS, HTTP, POP, NNTP, IMAP, BGP, IRC, NFS(나중버전)   

### 관련 내용   

   - __TCP__   
      - __point-to-point__   
         - 수신자 1 : 송신자 1   
      - __신뢰성있고, 순차적인 byte stream__    
         - message의 경계가 없다.   
         - GO-BACK-N과 같이 Timer를 하나 사용하지만 하나의 Segment만 재전송한다.   
         - IP의 unreliable service의 상위에서 RDT 서비스를 생성하여 reliable전송 할 수 있게 한다.   
         - Pipelined Segments   
      - __pipelined__   
         - TCP 혼잡, 흐름제어, window 크기 설정   
      - __send & receive buffer__   
         - 수신자와 송신자 모두 window 크기 만큼 고유의 send, receive buffer 2개를 가지고 있다.   
      - __full duplex data__   
         - 같은 연결에서 데이터가 양방향으로 갈 수 있다.   
         - MSS(Maximum Segment Size)   
      - __연결지향형__   
         - 3 way handshaking을 통해 데이터 교환전에 송신자와 수신자 연결   
            - Client가 TCP header의 SYN에 1을 세팅하고 자신의 Seq를 Server에 전송   
            - Server는 SYN에 1, SYN에 대한 ACK 1로 세팅하고, Client의 Seq에 대한 ACK(Client의 Seq + 1), 자신의 Seq를 Client에 전송   
            - Client는 SYN에 대한 ACK 1, Server의 Seq에 대한 ACK(Server의 Seq + 1)을 Server에 전송   
            __마지막 3번째 과정에서 Segment에 데이터를 넣어 같이 전송할 수도 있다.__   
         - 연결 해제 과정   
            - Close를 원하는 Client가 FIN 1을 세팅하여 Server에 전송   
            - Server가 FIN에 대한 ACK를 Client에 전송하고 보내야 할 데이터가 있을 경우 모두 보내고 연결 해제 후 FIN 1을 세팅하여 Client에 전송   
            - Client는 FIN을 받으면 time wait을 실행하고 FIN에 대한 ACK를 Server에 전송   
            __Server에 보낸 ACK가 유실될 경우 Server는 ACK을 받지 못하고 time out이 되어 Client에 FIN을 다시 전송할 수 있는데 Client가 연결 해제되어있을 경우 Server는 계속해서 FIN을 보내게 될 수도 있기 때문에 ACK가 확실히 전달되기 위해서 Client는 바로 연결 해제 하지 않고 time wait을 한다.__   
            
      - __TCP Segment(32bit) 구조__   
         - __출발지 Port번호(16bit), 도착지 Port번호(16bit)__    
            - 2^16-1 대략 6만개 정도의 Port 번호가 존재   
         - __Sequence Number(32bit)__   
            - Seq n: data의 첫번째 byte의 number   
         - __Acknowledgement Number(32bit)__   
            - ACK n: 다음으로 받아야 할 byte의 Sequence Number, 받은 Seq n + 1을 보낸다.   
            - __데이터를 받을 경우 ACK을 바로 보내지 않는다. 일정 시간을 기다린 후 ACK를 보내는 이유(pending)__   
               1. 내가 보내고자 하는 데이터를 같이 보낼수도 있다.   
               2. pipelining 방식으로 데이터가 오기 때문에 일일이 ACK을 하지 않고 __현재까지 수신된 byte들을 단 하나의 ACK로 일괄 확인 응답하는 Cumulative ACK__을 한다.   
            - __Timeout 값을 RTT의 값에 따라 조절할 경우__   
               - RTT는 Segment가 지나가는 경로가 다르기 때문에 항상 값이 다르고 만약 같은 경로로 지나간다해도 Segment의 Queuing delay 때문에 값이 다르다.   
               - 그러므로 Estimated RTT값을 사용하여 빠르게 반응할 수 있게 한다.   
                  - Sample RTT: 실제로 ACK을 받을 때까지 걸린 측정된 시간   
                  - Estimated RTT: 최근 RTT들의 평균   
                  - 이럴 경우 실제 RTT가 김에도 Estimated RTT가 짧아서 데이터 유실할 가능성이 높아진다.   
               - __그러므로 Estimated RTT * 4 * DevRTT(편차)를 사용하여 Timeout을 설정한다.__   
            - __Fast retransmit__   
               - TCP는 같은 ACK를 4번 받을 경우 유실됐다고 생각하고 Timeout 되기 전에 해당 데이터를 재전송하도록 권고한다.   
         - __header 길이, (U,A,P,R,S,F), Receive window__   
         - __checksum(16bit), URG data pointer(16bit)__   
         - __옵션(32bit)__   
         - __어플리케이션 데이터__   
         
   - __UDP__   
      - __UDP는 전송 계층이 할 수 있는 최소 기능인 Multiplexing/Demultiplexing, checksum을 통한 오류 검사만 한다.__   
      - __UDP Segment 헤더는 16bit씩 구성된 4개의 필드를 가진다.__   
         - 출발지 Port 번호   
         - 목적지 Port 번호   
         - UDP 세그먼트 길이   
         - __checksum - Segment안의 비트에 대한 변경사항이 있는지 검사를 통한 오류 검출__   
            - 송신측에서 UDP는 세그먼트 안에 있는 모든 16비트 워드 단위로 더하고 이에 대하여 1의 보수(0->1, 1->0)를 수행하며, 덧셈과정에서 발생하는 오버플로우는 Wrap around(가장 하위 비트에 더해줌) 윤회식 자리올림을 합니다. 이 결과는 UDP 세그먼트의 체크섬 필드에 삽입됩니다.   
            - 수신자에서는 체크섬을 포함한 모든 16비트 워드를 더합니다. 만약 패킷에 어떤 오류도 있지 않다면, 수신자에서의 합은 1111 1111 1111 1111 이 될 것입니다. 만약 비트 중에서 하나라도 0이 있다면 패킷에 오류가 발생했음을 알 수 있습니다.   
            - 링크계층 프로토콜(이더넷 프로토콜 포함)이 오류검사를 제공하는데, 굳이 왜 UDP가 체크섬을 제공하는가?   
               - 모든 링크가 오류검사를 제공한다는 보장이 없기 때문입니다. 즉, 링크 중에서 하나가 오류검사를 제공하지 않는 프로토콜을 사용할 수도 있는 것입니다. 그러므로 주어진 링크 간의 신뢰성과 메모리 오류검사가 완벽히 보장되지 않기 때문에, 종단간의 데이터 전송 서비스가 오류검사를 제공한다면, UDP는 종단간의 트랜스포트 계층에서 오류검사를 제공해야만 합니다.   
               - UDP는 오류검사를 제공하지만, 오류를 회복하기 위한 어떤 일도 하지 않습니다.   

      - __DNS가 UDP를 사용하는 어플리케이션 계층 프로토콜의 예__      
         1. DNS 질의(Query)를 생성할 때, DNS 질의 메시지를 작성하고 UDP에게 메시지를 넘겨줍니다.   
         2. 목적지 쪽에서 동작하는 UDP와 송신 측 UDP는 어떠한 Handshake도 수행하지 않고 메시지에 헤더 필드만 추가한 후 최종 세그먼트를 네트워크 계층에 넘겨준다.   
         3. 네트워크 계층은 UDP 세그먼트를 데이터그램으로 캡슐화하고 네임(name)서버에 데이터그램을 송신합니다.   
         4. 이 때 질의하는 호스트(송신쪽)에서의 DNS 애플리케이션은 질의에 대한 응답을 기다립니다.   
         5. 만약 질의 호스트가 응답을 수신하지 못하면, 질의를 다른 네임서버로 송신하거나 요청한 애플리케이션으로 응답할 수 없다는 것을 통보   

      - __TCP보다 UDP 방식으로 어플리케이션 개발하는 이유__   
         - __애플리케이션 레벨이 데이터 송신에 대해서 정교한 제어를 할 수 있습니다.__   
            UDP 하에서는 데이터를 UDP에게 전달되자 마자 UDP가 데이터를 세그먼트로 만들고, 즉시 그 세그먼트를 네트워크 계층으로 전달합니다. 이에 반해서 TCP는 혼잡제어 메커니즘(Congestion Control)을 가지고 있습니다. Congestion Control 기능은 호스트들 사이에 링크가 과도하게 혼잡해지면 TCP 송신자를 조절합니다. 또한 목적지가 세그먼트의 수신여부를 확인응답(ACK)할 때까지 총 시간이 얼마나 걸리든 상관없이 신뢰적인 전달을 보장하기 위해 세그먼트 재전송을 계속할 것입니다. 그러나 실시간(Real-time) 애플리케이션은 종종 최소전송률만을 요구하고, 지나치게 지연되는 세그먼트 전송을 원하지 않습니다. 또한 조금의 데이터 손실은 허용할 수 있으므로 TCP의 서비스 모델은 이들 애플리케이션의 요구와 맞지 않습니다.   

         - __연결 설정 X__      
            TCP는 3-way handshake를 사용하는 반면에 UDP는 형식적인 예비동작이 없습니다. 그러므로 UDP는 연결을 설정하기 위한 어떤 지연도 없습니다.   
         - __연결 상태 X__   
            TCP는 연결 상태를 유지하기 위한 수신버퍼, 송신버퍼, congestion control 파라미터, sequence number, ACK number 파라미터를 포함   
            UDP는 연결상태를 유지하지 않으므로 이 파라미터 중의 어떤 것도 기록하지 않습니다.   
            그래서 일반적으로 특정 애플리케이션에 할당된 서버는 애플리케이션이 TCP보다 UDP에서 동작할 때 좀 더 많은 클라이언트를 수용할 수 있습니다.   
         - __작은 패킷 헤더 오베헤드__   
            TCP는 20바이트의 헤더 오버헤드   
            UDP는 8바이트의 헤더 오버헤드   
            
## OSI 7 Layer    
   - 네트워크 통신에서 생기는 여러가지 충돌 문제를 완화하기 위하여, 국제표준기구(ISO)에서 표준화된 네트워크 구조를 제시한 기본 모델로 통신망을 통한 상호접속에 필요한 제반 통신절차를 정의하고 이 가운데 비슷한 기능을 제공하는 모듈을 동일계층으로 분할하여 모두 7계층으로 분할한 것   
   - __Application 계층(7-응용 계층)__   
      - __프로토콜__   
         - DHCP, DNS, FTP, HTTP    
         - 서비스 제공   
      - __기능__   
         - __Message = head + data(원하는 요청)__   
         - 사용자가 네트워크에 접근할 수 있도록 해주는 계층   
         - 사용자 인터페이스, 전자 우편, 데이터베이스 관리 등 서비스 제공   
         - ex) HTTP, SSH, SMTP, FTP, Telnet   
         
   - __Presentation 계층(6-표현 계층)__   
      - __프로토콜__   
         - JPEG, MPEG, SMB, AFP   
         - 이해할 수 있는 포맷으로 변환   
      - __기능__   
         - 입력 또는 출력되는 데이터를 하나의 표현 형태로 변환한다.   
         - 필요한 번역을 수행하여 두 장치가 일관되게 전송 데이터를 서로 이해할 수 있게 한다. = 확장자(jpg, gif, mpg...)   
         - 코드 변환, 데이터 암호화, 데이터 압축, 정보 형식 변환, 문맥 관리 기능   
         
   - __Session 계층(5-세션 계층)__   
      - __프로토콜__   
         - SSH, TLS   
      - __기능__   
         - Port 연결   
         - 통신장치 간 상호작용 설정을 유지하며 동기화한다.   
         - 사용자간 포트연결(세션)이 유효한지 확인하고 설정한다.   
         - 체크점(= 동기점) : 오류가 있는 데이터의 회복을 위해 사용하는 것으로 소동기점과 대동기점이 있다.   
         
   - __Transport 계층(4-전송 계층)__   
      - __프로토콜__   
         - TCP, UDP, ARP   
         - 장비 : 게이트웨이   
      - __기능__   
         - __Segment = head(출발지 port, 목적지 port 필드 등) + data(Message)__   
         - 전체 메세지에 대한 end-to-end 간 제어와 에러 관리   
         - 패킷들의 전송이 유효한지 확인하고 실패한 패킷은 다시 보내는 등 신뢰성있는 통신 보장   
         - 전송 연결 설정/해제, 데이터 전송    
         - 주소 설정, 다중화, 오류제어, 흐름제어   
         - ex) TCP, UDP   
         
   - __Network 계층(3-네트워크 계층)__   
      - __프로토콜__   
         - IP, ICMP, IGMP   
         - 장비 : 라우터   
      - __기능__   
         - __Packet = head(IP) + data(Segment)__   
         - 각 Packet이 시작 지점에서 최종 목적지까지 성공적으로 전달된다.   
         - 네트워크 연결 관리, 데이터의 교환 및 중계   
         - 경로 설정, 트래픽 제어, 패킷 전송   
         - ex) IP   
         
   - __Data link 계층(2-데이터링크 계층)__   
      - __프로토콜__   
         - MAC, PPP   
         - 장비 : 브리지, 스위치   
      - __기능__   
         - __Frame = head + data(Packet)__   
         - 3계층(네트워크 계층)에서 정보를 받아 MAC 주소와 제어정보를 시작과 끝에 추가   
         - 오류없이 한 장치에서 다른 장치로 Frame 전달   
         - MAC 주소를 이용하여 정확한 장치로 정보 전달   
         
   - __Physical 계층(1-물리 계층)__   
      - __프로토콜__   
         - Ethernet, RS-232C       
         - 장비 : 허브, 리피터   
      - __기능__   
         - 물리적 매체를 통해 비트흐름을 전송하기 위해 요구되는 기능들을 조정   
         - 네트워크의 두 노드를 물리적으로 연결시켜주는 신호방식을 다룬다.   
         - 케이블, 연결 장치 등과 같은 기본적인 물리적 연결기기의 전기적 명세를 정한다.   

 ## 라우팅과 포워딩     
   - __라우팅__   
      - __포워딩 테이블을 만들어 주는 것__   
      - __송수신 측 간의 전송 경로 중에서 최적 패킷 교환 경로를 설정하는 기능__   
      
   - __포워딩(Forwarding)__   
      - __Packet header에 들어있는 목적지 주소가 라우터 테이블(포워딩 테이블)을 참조하여 올바른 목적지를 찾아 전달하는 것__    

### 관련 내용         

  - __Routing__   
      - __포워딩 테이블을 만들어 주는 것__   
      - __포워딩 테이블에 모든 주소를 넣을 경우 검색하기도 복잡하고 관리하기도 힘들기 때문에 주소 범위를 정하여 관리한다.__   
      - __Hop__   
         - 패킷이 하나의 라우터에서 다른 라우터로 이동하는 한 구간   
      - __포워딩 테이블 종류__   
         - __Longest prefix matching__   
            - 목적지 주소와 매칭되는 가장 긴 prefix 주소에 따라 링크를 연결한다.   

      - __Routing 알고리즘__   
         - __목적지까지 최소 비용 경로를 찾는 알고리즘__   
         - __접근 방식 2가지__   
            - __Link state 알고리즘__   
               - OSPF를 구현할 수 있다.   
               - 각 라우터가 모든 네트워크 정보를 알고 있는 경우(Global)   
                  - 모든 라우터가 자신의 Link state를 broadcast 하여 서로 같은 모든 정보를 알 수 있게 한다.   
                     - 모든 네트워크가 아닌 자신이 속해있는 네트워크에만 broadcast를 한다.   

                  - 메인 서버가 아니라 각 라우터들이 다익스트라 알고리즘을 사용하여 출발지부터 모든 라우터에 대한 최단경로를 찾아내기 위한 자신 고유의 포워딩 테이블을 만들어낸다.   
                     - n개의 노드가 있을 때, 모든 노드를 확인해야하기 때문에 n(n+1)/2번의 비교가 필요하므로 O(n^2)의 시간이 걸린다.   
                     - 더 효율적으로 구현하면 O(nlogn)이 가능하다.   
                     - oscillation possible   
                        - 주어진 트래픽의 양의 따라 새로운 경로를 찾아내는 과정을 반복하면서 링크를 계속 변경한다.   

            - __Distance vector 알고리즘__   
               - RIP를 구현할 수 있다.   
               - 이웃해있는 라우터에 대해서만 정보 교환을 통해 알고 있는 경우(Decentralized)    
                  - __벨만포드 알고리즘을 사용하여 최단 경로를 찾아낸다.__   
                     - 자신의 distance vector estimate를 이웃들에게 보낸다.   

                  - __iterative, asynchronous__   
                     - local iteration이 발생   
                        - local link cost가 변경될 경우   
                        - 이웃으로부터 Distance Vector 업데이트 메세지 받을 경우   
                           - 이웃으로부터 Distance Vector 업데이트 메세지 받으면, B-F equation을 이용하여 자신의 Distance Vector 업데이트   
                  - __distributed__   
                     - 각 노드들은 자신의 Distance Vector가 변경될 때 이웃에게 알린다.      
                  - __link cost changes__   
                     - __x-y : 4, y-z : 1, x-z : 50일 경우__
                     - __link cost가 감소한 경우 업데이트가 빠름__   
                        - x-y : 4 -> 1, y-z : 1, x-z : 50   
                        - 변경된 link cost값을 Distance vector에서 변경해주면서 업데이트 하면 된다.   
                        - t0 : y가 link cost 변화를 탐지하고 자신의 DV를 업데이트한 후, 이웃들에게 이를 알림   
                        - t1 : z가 y로부터 업데이트 정보를 받고, 자신의 테이블을 업데이트. x로의 최소 비용을 업데이트 한 후, 이웃들에게 자신의 DV 보냄   
                        - t2 : y가 z로부터 업데이트된 정보로를 받고, 자신의 테이블을 업데이트. y의 최소 비용 테이블은 변하지 않았으므로 z에게 메시지를 보내지 않음 => 업데이트 끝!   

                     - __link cost가 증가한 경우 업데이트가 느림__   
                        - x-y : 4 -> 60, y-z : 1, x-z : 50   
                        - 어떤 목적지로 가는 도중 중간에 거쳐가는 노드에게도 Distance vector값을 전달할 경우 자기 자신을 다시 거쳐가는 경우를 모르기 때문에 Distance Vector 값이 계속해서 변경될 가능성이 있다.   
                           - z가 x로 갈 때, y를 통한 경로보다 x로 바로 가는 것이 cheap하다는 것을 깨달을 때까지 44번의 반복이 발생 -> 'count to infinity' 문제   
                        - 그러므로 중간에 거쳐가는 노드에는 변경된 값을 넘겨주지 않고 무한대 값을 넘겨준다.   
                        - __poison reverse__를 이용해 문제 해결  
                           - 만약 Z가 X를 얻기 위해 Y를 통해 간다면 z에서 y 거리를 ∞ 주고 업데이트 하도록 함   
                           - 그 이후 다음 단계에서 Distance vector를 검사한다.   
                           - 즉, 업데이트된 곳 반대를 ∞로 설정해주고 업데이트하면 빠르다.   
                           - poisoned reverse가 infinity problem을 대부분 해결하기는 하나, 세 개 이상의 이웃 노드를 포함한 루프인 경우 감지하지 못한다.   
                  - __즉, 이웃으로부터 Distance Vector list를 받거나 링크 cost값이 변경되면 B-F equation을 이용하여 자신의 Distance Vector 계산한 뒤 업데이트가 되었다면 이웃에게 전달한다.__   
            - __Link state , Distance vector 알고리즘 둘 다 하나의 네트워크에 국한되었을때 내부를 위한 라우팅 알고리즘이다.__   

         - __Hierarchical routing 알고리즘__   
            - 6억개의 목적지가 있다면?   
               - 모든 목적지를 라우팅 테이블에 저장할 수 없음   
               - 라우팅 테이블의 변경은 링크를 벅차게 함   

            - __Administrative Autonomy__   
               - internet = network of networks   
               - 각 네트워크 admin은 자신의 네트워크 내에서 라우팅을 컨트롤하기를 원한다.   

            - __Autonomous Systems(ASes)__
               - __AS: LG U+, SKT, KT와 같은 자치권을 가진 시스템으로 각자 고유의 AS 번호를 가진다.__   
                  - AS Numbers(ASNs)   
                     - 16 bit 값   
                     - 64512 ~ 65535는 private   
                     - 현재 11000개 이상이 사용되고 있다.   
               - __AS간 관계가 존재__   
                  - 비용과 관련   
                  - __Cutomers and Providers Relationship__   
                     - Customer는 인터넷 접속을 위해 Provider에게 비용 지불하고 Privider는 Customer에게 서비스 제공      
                     - ex) 학생 등록금 <-> 대학교 AS <-> SKT   
                  - __Peering Relationship__   
                     - 비슷한 계층 AS가 있을 경우에는 서로 비용을 지불하지 않는다.   

               - 같은 AS 내의 라우터는 같은 라우팅 알고리즘을 사용
               - 라우터를 영역으로 통합   
               - __Inter-AS 라우팅 프로토콜__   
                  - AS 사이의 라우팅 알고리즘   
                     - __BGP__   
                        - Routing Gateway Protocol   
                        - AS 정책에 기반   
                  - __ASPATH__   
                     - 자신의 Prefix(AS 번호)를 다른 AS에 광고하면서 목적지까지 전달됨   
                     - 각 AS를 거칠때마다 자신의 AS 번호를 ASPATH 추가시키고 목적지에서는 경로 횟수 또는 비용 등 정책에 따라 경로를 선택할 수 있다. 일반적으로 AS는 자신이 갑이 되는 위치 즉, 자신이 비용을 제일 많이 받을 수 있는 경로를 선택한다.   

               - __Intra-AS 라우팅 프로토콜__   
                  - Interior Gateway Protocols (IGP)라고도 알려짐   
                  - AS 내의 라우팅 알고리즘   
                     - 최단경로가 목적   
                     - __RIP__   
                        - Routing Information Protocol   
                        - Distance vector 알고리즘 : 이웃의 더 좋은 정보를 이용하여 업데이트    
                        - RIP table processing   
                           - RIP 라우팅 테이블은 route-d라 불리는 application 계층 프로세스에 의해 관리된다.   
                           - advertisement는 주기적으로 반복해서 UDP 패킷을 보냄   

                     - __OSPF__   
                        - Open Shortest Path First  => 빠르고 정확   
                        - Link state 알고리즘 : 다익스트라 알고리즘으로 경로 연산   
                        - OSPF advertisement는 이웃 당 하나의 엔트리를 전달   
                        - advertisements는 전체 AS에게 flood   
                           - TCP나 UDP 대신에 IP로 직접 OSPF 메시지 전달   
                        - IS-IS routing protocol: OSPF와 거의 동일   
                        - OSPF "advanced" features (not in RIP)   
                           - security: 모든 OSPF 메시지는 인증됨(authenticated) -> 악의적인 침입을 막기 위해   
                           - 여러개의 같은 cost 경로를 허용 (RIP는 하나의 경로만 허용)   
                              - cost가 같은 경로 여러 개가 있으면 나눠서 보냄 => 속도↑   
                           - 각 link에 대해 서로 다른 TOS에 대한 여러 cost 행렬 (예: best effort ToS에 대해 낮음으로 설정된 위성 링크 비용, 실시간 ToS에 대해 높음)   
                           - 통합된 uni-cast 와 multicast 지원   
                              - Multicast OSPF (MOSPF)는 OSPF 기반의 동일한 토포로지 데이터를 사용한다   
                           - 계층적인 OSPF in large domains   
                        - __Hierarchical OSPF__   
                           - two-level hierarchy: local area, backbone   
                              - link-state advertisements는 영역 안에서만 일어남   
                              - 각 노드에는 자세한 영역 토폴로지가 있다.   
                              - 다른 지역은 net로 가는 방향(최단 경로)만 안다   
                              - __area border routers__   
                                 - 자신의 영역 안의 nets까지의 거리를 요약하여 다른 영역 경계 라우터에게 알린다.   
                                 - __backbone routers__    
                                    - 백본으로 제한된 OSPF 라우팅을 실행   
                                    - __boundary routers__   
                                       - 다른 AS에 연결   

         - __Broadcast, Multicast routing__   
            - 출발지에서 모든 다른 노드에게 패킷 전달   
            - source 중복은 비효율적이다.   

            - __In-network duplication__   
               - __Flooding__   
                  - 노드가 broadcast 패킷을 받으면, 복사본을 모든 이웃에게 보낸다.   
                     - 문제점: cycles & broadcast storm   
                  - 즉, 나를 제외한 나머지한테 보냄   
               - __Controlled flooding__   
                  - 이전에 같은 패킷을 broadcast한 적이 없는 경우에만 패킷을 broadcast한다.   
                     - 노드는 이미 broadcast한 패킷의 트랙을 저장한다.   
                     - 또는 reverse path forwarding (RPF): 출발지와 노드 사이의 최단 경로에 도달한 경우에만 패킷 forward   
                  - 즉, 나와 이전에 보낸 사람 제외하고 보냄 = broadcasting   
               - __Spanning tree__   
                  - 어떤 노드로부터 중복된 패킷을 받지 않는다.   
                  - 노드는 스패닝 트리를 따라서만 카피하고 forward한다.   
 
## ARQ   
   - __ARQ(Automatic Repeat reQuest) protocol__   
      - __에러 검출 부호를 사용하여 데이터의 에러를 자동적으로 발견하여 그 재송을 요구하는 방식__   
      - __Error 검사__
         - checksum bits를 추가   
      - __Feedback__   
         - ACKs(Acknowledgements): 수신자가 Packet을 정확하게 받았다고 송신자에게 알려줌   
         - NAKs(Negative acknowledgements): 수신자가 Packet에 Error가 있다고 송신자에게 알려줌   
      - __Re-transmission__   
         - 송신자가 NAK를 보낸 곳에 Packet을 재전송한다.   
         
   - __ARQ 방식__   
      - __Stop and wait ARQ__   
         - 하나의 Packet 송신할 때마다 수신자로부터 ACK 또는 NAK을 받는데 ACK을 받을 경우 다음 Packet 전송하고 NAK일 경우 현재 Packet 재전송   
            - 송신자가 ACK 또는 NAK을 제대로 받지 못하게 되기 때문에 보냈던 데이터를 다시 수신자에게 보내게 된다. 이러한 경우 수신자는 다시 받은 데이터가 새로운 데이터인지 아니면 중복된 데이터인지 알 수 없기 때문에 __Sequence Number를 사용하여 Packet을 구분__ 하게 할 수 있다.    
               - 송신자는 Sequence Number를 각 Packet에 추가   
               - 수신자는 중복된 Packet 무시   
      - __Continuous ARQ__   
         - 여러 Packet을 연속하여 송신하고 수신자는 error가 있는 Packet을 받은 경우 해당 Packet에 대한 Sequence Number와 NAK을 전송한다.   
            - 송신자는 Error에 해당하는 Packet만 재전송하거나 또는 Error Packet부터 그 뒤에 Packet들까지 다시 연속하여 모두 재전송한다.   
   - __ARQ Pipelining 방식__     
      - __송신자의 이용률을 증가시킬 수 있다.__   
      - __GO-Back-N__    
         - Window size n만큼 feedback받지 않고 전송할 수 있다.   
            - Window size n만큼 전송시 하나의 Timer가 켜지고 제한 시간 내 ACK을 받지 못할 경우 모두 다시 재전송한다.     
            - ex) Timer를 켜고 0,1,2,3,4,5 중 앞 Packet 4개를 보냈는데 0,1이 제대로 전송되어 ACK 0, 1을 받아 4,5를 전송하고 2는 제대로 전송이 안되어 ACK 1을 보내 2를 제대로 받지 못한 것을 알리고 3은 제대로 전송이 됐을 경우에도 3에 대한 값은 무시하고 ACK 1을 보내 2를 제대로 받지 못한 것을 송신자에게 알린다. 이후 4,5도 제대로 전송이 됐을 경우에도 마찬가지로 4,5에 대한 값은 무시하고 ACK 1을 보내 2를 제대로 받지 못한 것을 송신자에게 알린다. 이 과정에서 Time 제한 시간이 다 되면 송신자는 2,3,4,5를 전송한다.   
            - __즉 Packet loss 일어날 시 n만큼 돌아와서 전부 다시 보낸다.__    

      - __Selective Repeat__   
         - Window size n만큼 Feedback받지 않고 전송할 수 있다.    
            - GBN의 전부 재전송하는 비효율을 해결하기 위한 방식   
            - Window size n만큼 전송시 각 Packet의 Timer가 켜지고 각 Packet의 제한 시간 내 ACK을 받지 못할 경우 ACK을 받지 못한 Packet만 재전송한다.   
            - ex) 0,1,2,3 Packet 4개를 보냈는데 0에 대한 ACK를 받을 경우 1,2,3,4로의 범위로 바뀌고 4에 대해서 전송하고 Timer를 켜고 1에 대한 ACK를 받을 경우 2,3,4,5의 범위로 바꾸고 5를 전송하고 Timer를 키는데 2에 대한 ACK을 받지 못하고 2에 대한 Timer 제한 시간이 다 된 경우 2를 다시 재전송한다.    
            - __즉 Packet loss 일어난 Packet만 다시 보낸다.__    
      
         
   - __ARQ를 사용하는 이유__   
      - 네트워크 계층은 Unreliable Channel이기 때문에 Message error(Packet error), Message loss(Packet loss)가 발생할 수 있는데 전송 계층 네트워크 계층 사이에 RDT(Reliable Data Transfer) protocol을 구현함으로써 데이터를 Reliable하게 전송할 수 있게 하기 위해서이다.   
         - RDT 내에 ARQ가 있다.   
         - 전송 계층의 TCP가 Reliable Channel로 데이터를 잘 전송하는 것 같이 보이지만, 실제적으로는 네트워크 계층의 Unreliable Channel 위에 RDT protocol을 구현함으로써 데이터를 Reliable하게 전송할 수 있는 것이다.   

### 관련 내용   
- __네트워크 계층은 Unreliable Channel이다.__   
   - __Unreliable을 통해 발생할 수 있는 것__   
      - Message error(Packet error)   
      - Message loss(Packet loss)   
   - __전송 계층의 TCP가 Reliable Channel로 데이터를 잘 전송하는 것 같이 보이지만, 네트워크 계층의 Unreliable Channel 위에 RDT(Reliable Data Transfer) protocol을 구현함으로써 데이터를 Reliable하게 전송할 수 있는 것이다.__   
   - __stop-and-wait protocol__   
   - __수신자와 송신자를 명시하기 위해 FSM(Finite State Machines)를 사용__   
   - __하위 채널이 완벽히 reliable할 경우(RDT1.0)__   
      - reliable transfer를 위한 어떠한 메카니즘도 필요없다.   
   - __하위 채널이 packet error(손실 X)를 가질 경우 필요한 메카니즘(RDT2.0)__   
      - __ARQ(Automatic Repeat reQuest) protocol__   
         - __Error 검사__
            - checksum bits를 추가   
         - __Feedback__   
            - ACKs(Acknowledgements): 수신자가 Packet을 정확하게 받았다고 송신자에게 알려줌   
            - NAKs(Negative acknowledgements): 수신자가 Packet에 Error가 있다고 송신자에게 알려줌   
         - __Retransmission__   
            - 송신자가 NAK를 보낸 곳에 Packet을 재전송한다.   
   - __ACK 또는 NAK에 Error가 있을 경우 필요한 메카니즘(RDT2.1)__   
      - 송신자가 ACK 또는 NAK을 제대로 받지 못하게 되기 때문에 보냈던 데이터를 다시 수신자에게 보내게 된다. 이러한 경우 수신자는 다시 받은 데이터가 새로운 데이터인지 아니면 중복된 데이터인지 알 수 없기 때문에 __Sequence Number를 사용하여 Packet을 구분__ 하게 할 수 있다.   
      - 송신자는 Sequence Number를 각 Packet에 추가   
      - 송신자는 ACK 또는 NAK을 제대로 받지 못할 경우 현재 Packet 재전송   
      - 수신자는 중복된 Packet 무시   
      - __Packet header에 sequence number를 계속 추가할 경우__   
         - header의 크기가 커지기 때문에 좋지 않다. header에는 최소한의 필요한 필드로만 구성되야하고 구성되는 필드들도 최소의 크기를 가지는 것이 좋다.   
         - sequence number를 최소화 시키기 위해서 0, 1 두개만 사용한다.  
   - __RDT2.1과 같은 기능을 가지지만 NAK을 사용하지 않는 메카니즘(RDT2.2)__   
      - 수신자가 마지막으로 제대로 받은 Packet의 sequence number를 ACK에 추가하여 송신자에게 전달함으로써 송신자는 ACK의 sequence number를 통해 보낸 Packet의 재전송 또는 새로운 Packet 전송을 한다.   
   - __Packet loss와 error 둘 다 있을 경우 필요한 메카니즘(RDT3.0)__   
      - __Timer__   
         - 일정 시간내에 ACK를 받지 못할 경우 Packet 재전송   
         - __Timer 짧은 경우__   
            - loss가 일어날 경우 재전송이 빠르다
            - Packet 전송이 되는 중이거나 ACK 오고 있음에도 다시 전송하여 중복된 Packet을 다시 보내어 네트워크 오버헤드가 커질 수 있다.   
            - Sequence number를 통해 수신자가 같은 Packet일 경우 무시(RDT2.2)해버리지만 송신자는 계속 다시 보낼 수 있기 때문에 네트워크에 문제가 생길 수 있다.   
         - __Timer 길 경우__   
            - 기다리는 시간이 길기 때문에 네트워크의 오버헤드가 적다.    
            - loss가 일어날 경우 반응이 느리다.
   __RDT3.0은 신뢰적인 데이터 전송의 완벽한 기능을 가지지만 Stop-and-wait 방식이기 때문에 고속 네트워크에서 좋은 성능을 보여주지 못한다.__   
      - __ACK을 기다리지 않고 여러 Packet을 보내는 방식을 사용함으로써 문제 해결 가능__   
      - __Pipelining__   
         - __송신자의 이용률을 증가시킬 수 있다.__   
         - __3가지 고려사항__   
            - 여러 Packet을 보내기 때문에 0,1이 아닌 __sequence number 범위 증가 필요__   
            - 여러 Packet을 담을 수 있는 __버퍼 필요__   
            - Pipelining Packet loss와 delayed Packet에 대한 __오류 회복 방법__   
         - __2가지 기본적인 방법__   
            - __GO-Back-N__   
               - Window size n만큼 feedback받지 않고 전송할 수 있다.   
                  - Window size n만큼 전송시 하나의 Timer가 켜지고 제한 시간 내 ACK을 받지 못할 경우 모두 다시 재전송한다.  
                      - Window 안에 있는 Packet들은 수신자가 제대로 Packet을 못받을 경우 재전송해야하기 때문에 송신자 send 버퍼에 반드시 저장되어있어야 한다.   
               - ACK N: N까지 모두 잘 수신했다는 의미   
               - 수신자는 순서대로 항상 정확하게 수신한 Packet의 가장 큰 seqence number를 ACK로 송신자에게 보낸다.   
                  - 잘못된 순서의 Packet을 받을 경우 무시하고 이전에 정확하게 받은 Packet의 가장 큰 seqence number를 ACK로 송신자에게 보낸다.   
                  - ex) Timer를 켜고 0,1,2,3,4,5 중 앞 Packet 4개를 보냈는데 0,1이 제대로 전송되어 ACK 0, 1을 받아 4,5를 전송하고 2는 제대로 전송이 안되고 ACK 2를 받지 못하고 3는 제대로 전송이 됐을 경우 3에 대한 값은 무시하고 ACK 1을 보내 2를 제대로 받지 못한 것을 송신자에게 알린다. 이후 4,5도 제대로 전송이 됐을 경우에도 마찬가지로 4,5에 대한 값은 무시하고 ACK 1을 보내 2를 제대로 받지 못한 것을 송신자에게 알린다. 이 과정에서 Time 제한 시간이 다 되면 송신자는 2,3,4,5를 전송한다.   
                  - __즉 Packet loss 일어날 시 n만큼 돌아와서 전부 다시 보낸다.__    

            - __Selective Repeat__   
               - Window size n만큼 Feedback받지 않고 전송할 수 있다.   
                  - Window size n만큼 전송시 각 Packet의 Timer가 켜지고 각 Packet의 제한 시간 내 ACK을 받지 못할 경우 ACK을 받지 못한 Packet만 재전송한다.   
               - ACK N: N을 잘 수신했다는 의미   
               - 수신자는 정확히 받은 각 Packet에 대해서만 ACK을 송신자에게 보낸다.   
                  - 받은 Packet은 버퍼에 반드시 저장해야한다.   
                     - 손실된 패킷 이후 정확히 받은 다른 패킷들은 수신자의 손실된 패킷 자리를 비워놓고 recv 버퍼에 저장해놓은다.   
                     - 버퍼가 순서대로 꽉 차게 되면 어플리케이션 계층에 넘겨주고 마지막 받은 Packet에 대한 ACK을 송신자에게 보내고 버퍼를 비운다.   
                  - 0,1,2,3 Packet 4개를 보냈는데 0에 대한 ACK를 받을 경우 1,2,3,4로의 범위로 바뀌고 4에 대해서 전송하고 Timer를 켜고 1에 대한 ACK를 받을 경우 2,3,4,5의 범위로 바꾸고 5를 전송하고 Timer를 키는데 2에 대한 ACK을 받지 못하고 2에 대한 Timer 제한 시간이 다 된 경우 2를 다시 재전송한다.   
                  - __즉 Packet loss 일어난 Packet만 다시 보낸다.__   
                  - But, Sequence number가 계속 증가하는데 Sequence number의 값의 크기는 최대한 작게 하는 것이 좋다.   
                     - __그러므로 Window size가 n일 경우 Sequence number의 범위를 2n으로 할 경우 재전송할 Packet에 대한 혼선없이 전송이 가능하다.__   
                  - __문제점은 모든 Packet이 Timer를 가져야하기 때문에 Window size가 클 경우 복잡해지기 때문에 잘 사용하지 않는다.__   
                     - 그래서 __보통 Window를 대표하는 Timer를 하나__ 를 가지고 사용한다.   

## TCP 흐름제어, 혼잡제어   
   - __흐름 제어__   
      - __수신자가 받을 수 있는 만큼만 송신자가 데이터 전송하기 위한 제어__   
      - __수신자의 버퍼가 얼마만큼 비어있는지 송신자에게 알려줘야한다.__   
         - __Sliding Window__   
            - 설정한 윈도우 크기만큼 송신 측에서 확인 응답 없이 세그먼트를 전송할 수 있게 하여 데이터 흐름을 동적으로 조절하는 제어 기법   
            - TCP header의 recv_buff 필드에 저장해서 전송한다.   
            - 송신자는 수신자의 recv_buff가 비어있는만큼 데이터를 전송한다.   
               - 보내는 양이 많으면 보내는 속도가 빠르고 보내는 양이 적으면 보내는 속도가 느리다.   
            - 수신자의 app에서 읽기 버퍼로부터 데이터를 읽지 않을 경우 계속해서 쌓인다.   
               - 빈 공간이 하나도 없게될 경우 TCP header의 recv_buff에 0 byte를 저장해서 전송한다.   
               - 송신자는 기다리지 않고 주기적으로 데이터 없이 또는 1 byte를 넣어 의미없는 Segment를 보내어 수신자의 ACK를 받아 수신자의 읽기 버퍼 상태를 확인한다.   
            - __Window size__   
               - 한번에 전송할 수 있는 최대 프레임 크기(통상, `바이트 갯수` N)를 의미   
               - 호스트들은 실제 데이터를 보내기 전에 먼저 “TCP-3-way handshaking”을 통하여 수신컴퓨터의 receive window size에 자신의 send window size를 맞추게 된다.   
               - 송신측은 순서번호, 수신측은 승인번호(확인응답번호)로 관리   
               - __수신 Window size(rwnd, Receiving Window)__   
                  - 수신 버퍼의 여유 용량   
               - __혼잡 윈도우 크기 (cwnd, Congestion Window)__   
                  - 네트워크 혼잡을 초래하지 않도록 송신율을 제한하는 (송신) 윈도우 크기   
               - __TCP에서는 윈도우 크기를 TCP 최대 세그먼트 크기(MSS) 보다 크게 할 수 없다.__   
               - __실제 윈도우 크기__   
                  - 실제 송신 윈도우 크기 = min ( cwnd, rwnd )   
                  - 즉, cwnd 및 rwnd 중 작은 값을 취함   
               - __흐름 제어__   
                  - 수신측이 주도적으로 rwnd 값 결정   
                  - 수신측은 ACK(확인응답)을 보내면서 현재의 수신 윈도우 크기를 함께 보내게 된다.   
               - __혼잡 제어__   
                  - 네트워크 혼잡 상황에 따라 cwnd 값 결정   
            
   - __혼잡 제어__   
      - __송신자와 수신자 사이의 네트워크가 수용할 수 있는 만큼만 데이터 전송하기 위한 제어__   
      __네트워크가 막힐 경우 TCP 데이터가 제대로 전달되지 않아 재전송을 하는 특성 때문에 네트워크를 더 막히게 하여 TCP는 무너지게 된다.__   
      - 어떻게 Public 네트워크 막히지 않게 할 수 있을까?   
         - 네트워크가 혼잡할 경우 보내는 데이터의 양을 줄이고 한가할 경우 보내는 데이터의 양을 늘린다.   
         - 네트워크의 상태를 확인하는 방법   
            - __Network-assisted__   
               - 네트워크의 라우터들이 상태를 알려준다.   
               __라우터들은 데이터를 전송하느라 바쁘기 때문에 이 방법은 이상적인 방법이다.__   
            - __End-end__   
               - __현재 인터넷의 구현 방식__   
               - 호스트가 서로 TCP Segment를 전송하면서 유추하는 방법   
               - TCP Segment의 ACK를 통해 유추하기 때문에 정확하지 않다.
      - __Slow start__   
         - 1개, 2개, 4개 .... 2n개 즉, 수신자로부터 ACK을 받을 경우 보내는 Segment를 2배씩 증가시킨다.   
      - __Additive increase__   
         - threshold(한계점)을 넘을 경우 2배씩이 아니라 1씩 증가시켜서 보낸다.   
      - __Multipicative decrease__   
         - Packet loss가 일어날경우 window size를 2배 줄이고 Slow start를 다시 처음부터 시작한다.   
            1. __TCP Tahoe__   
               - 80년대의 TCP로 첫번째 TCP 버전   
               - threshold를 Window size/2로 설정   
            2. __TCP Reno__   
               - TCP 두번째 버전으로 __현재 인터넷에서 사용__   
               - Packet loss이 일어나는 상황   
                  1. timeout   
                     - threshold를 Window size/2로 설정하고 Window size를 threshold로 설정 후 1씩 증가하며 다시 시작   
                  2. Fast Retransmit(같은 ACK를 4번 받을 경우)   
                     - threshold를 Window size/2로 설정하고 Window size를 1 MSS로 설정 후 시작. 즉, Slow start부터 다시 시작   
                  - __timeout 상황이 더 위험한 상황이다.__      
         - __네트워크는 공유 자원이기 때문에 해당 네트워크에 접근하는 모든 호스트는 window size를 2배 줄여야 하고 threshold 이후에는 항상 1씩 증가한다.__   
      
      - __TCP Fairness__   
         - 같은 네트워크를 사용하는 아주 많은 TCP 세션들이 존재한다. (같은 라우터를 사용)   
         - K개의 TCP 세션이 대역폭 R의 같은 라우터를 사용한다면, 각각의 세션은 R/K의 평균 전송률을 가진다.   
            - K개의 TCP가 Additive increase를 증가시켜 전송량을 늘리고 R을 초과할 경우 Multipicative decrease하고 다시 Additive increase를 하는 과정을 반복함으로써 각각의 TCP 세션이 공평한 전송률은 가지게 된다.   
         - TCP끼리의 경쟁이기 때문에 TCP를 많이 연 사람이 더 높은 전송률을 가지게 된다.    

      - __MSS(Maximum Segment Size)__   
         - 500 Byte   
         - Window size가 1 MSS로 설정되어있다. 그러므로 처음에는 하나의 Segment만 전송할 수 있고 이에 대한 ACK를 받을 시 2배씩 증가한다.   
      - __전송률 = Window size / RTT__     
         - Window size가 더 변동이 심하기 때문에 전송률은 Window size(CongWin)에 좌우된다.   
         
      __즉 송신자가 보내는 속도는 수신자의 버퍼 상태와 네트워크 상태에 의해 결정되는데 더 좋지 않은 쪽에 맞춰서 데이터를 전송해야 한다.__      

## TCP 타이머 종류   
   - __재전송 타이머(Retransmission timer)__   
      - 손실된 세그먼트를 재전송하기 위해서, 한 세그먼트를 보내고 그 세그먼트의 ACK을 기다리는 시간에 대한 타이머   
      - 매 세그먼트 전송 시 마다 가동   
         - 보통 3~6초, 권고 1초   
      - 시간 내에 ACK을 받지 못할 경우 재전송   
      - RTT(Round Trip Time) 통해 RTO(Retransmission Timeout) 도출(RTO = RTTs + 4*RTTd)   
         - RTO 값은 상황에 따라 유동적    
         - ICMP 통해 RTT 값 측정 가능   
      
   - __영속 타이머(Persistent timer)__   
      - 송신자가 Packet을 보냈는데 수신자의 receive buffer가 Full인 상태일 경우 수신 TCP의 Window size = 0으로 하여 ACK를 보내 송신자가 수신자의 receive buffer 상태를 확인하는데 사용하는 타이머    
      - 즉, 수신 TCP Window size가 0임을 알려올 때 필요한 타이머   
      
      - __Pesistent timer를 사용하지 않으면 발생할 수 있는 문제점__   
         - TCP에서 ACK의 ACK은 전송하지 않기 때문에, ACK이 손실되었을 경우, 즉 수신 TCP에서 새로운 Window size를 알리는 ACK이 손실되었을 경우 문제가 발생   
         - 수신 TCP는 송신 TCP가 세그먼트를 전송하기를 기다리고, 송신 TCP는 수정된 Window size를 알리는 ACK을 기다리게 된다.   
         - 즉, 교착상태(deadlock)에 빠지게 된다.   
      - __송신 TCP가 윈도우 사이즈 0인 ACK을 수신하면, persistence timer가 시작된다.   
         - 이 타이머가 만료되면, 송신 TCP는 Windows Probe Packet이라는 특수한 세그먼트(1byte) 전송한다.   
            - probe는 순서번호를 가지고 있지만, ACK되지 않고, 수신 TCP 순서 번호 계산에서도 무시된다.   
            - 오직 수신 TCP으로부터 ACK 응답(rwnd)을 받기위해서 사용한다.       
         - __Probe__   
            - 메시지의 전달 가능성 확인 등, 네트워크의 상태에 관하여 무엇인가를 알아내기 위한 목적으로 이루어지는 행동, 또는 사용되는 객체   
            - ex) Ping, 메시지의 도착지가 실제로 존재하는지를 알기 위해, 단순히 내용이 없는 메시지를 보내는 것을 들 수 있다.  
      
   - __연결유지 타이머(Keep-alive timer)__   
      - TCP 사이에 오랜 기간 휴지(idel) 상태에 있는것을 방지하기 위해 사용되는 타이머   
      - Server와 Client 연결간에 데이터가 오고가지 않을 경우, 서버에서 연결이 유효한지 체크하는 타이머   
         - Server는 Client로부터 세그먼트 받을때마다 타이머 초기화한다. (보통 7200초(2시간))   
         - 시간 내에 데이터를 받지 못할 경우 Server는 Client에게 Probe Packet을 75초 간격 9번 전송한다.   
         - Server가 probe의 ACK을 받지 못할 경우 Client와 연결 종료   
      
   - __시간대기 타이머(Time-waited timer)__   
      - 연결 종료 과정에서 2MSL(maximum segment lifetime)만큼 TIME-WAIT상태에 들어가서 대기했다가 연결을 종료하기 위해서 사용하는 타이머 
         - 보통 4분   
         - TCP 세그먼트 존재 시간 = 보통 2분   
      - 이전 연결 종료 전의 어떤 패킷이 늦게, 중복지연 도착하는 것을 방지   
      - __과정__   
         - Client application에서 Client TCP로 Passive close(close())를 수행할 것을 알림   
         - Client TCP에서 FIN을 Server TCP로 전송하고 FIN-WAIT-1 상태로 전이   
         - Server TCP에 전송했던 FIN의 ACK을 받으면 FIN-WAIT-2 상태로 전이   
         - Server TCP로 부터 FIN이 전송되면 이 FIN에 대한 ACK을 전송하고 TIME-WAIT상태로 전이하고 타이머 작동       
         - 2MSL(maximum segment lifetime) 후에 타이머가 끝나고 연결은 종료   
      - __타이머 사용하는 이유__   
         - Server가 보낸 FIN에 대한 ACK이 손실(lose)되면 서버는 계속 FIN을 보내게 되는데 Client가 TIME-WAIT상태 없이 바로 종료를 하면, 이 FIN을 받지 못할 것이고, 연결은 정상적으로 종료 될 수 없다.   
            - 그러므로 Client가 TIME-WAIT상태에서 FIN을 받으면 다시 ACK을 전송하고 이 타이머를 재가동 한다.   
            
         - Client가 TIME-WAIT없이 바로 종료 되면, 방금 close된 socket과 동일한 socket주소를 가지는 연결이 생길 수 있는데 Server는 이전 연결과 새로운 연결을 구분하기 힘들기 때문에 세그먼트가 꼬일 수 있다.   
            - 그러므로 2MSL이라는 충분한 시간을 두어 새로운 client가 이전 연결과 같은 소켓주소를 못쓰게 한다.   
            
## GET, POST       
   - __GET__   
      - 어떠한 정보를 가져와서 조회하기 위해서 사용되는 방식    
         - 서버의 어떤 값이나 내용, 상태 등을 바꾸지 않는 경우에 사용된다.   
      - URL에 변수(데이터)를 포함시켜 요청한다.   
         - URL 뒤에 ? 마크를 통해 URL의 끝을 알리고, Key-Value의 쌍으로 인수 앞에 &을 붙여 구분하며, 글자수는 255자로 제한된다.   
      - URL에 포함되기 때문에 HTTP 패킷의 Header(헤더)에 데이터가 포함되어 서버에 요청된다.   
         - Body는 보통 빈 상태로 전송된다.(HTTP 패킷의 Body)    
         - Header안에 Body의 데이터를 설명하는 Content-type 헤더 필드도 들어가지 않는다.    
      - URL에 데이터가 노출되어 보안에 취약하다.   
         - ex) `www.google.com/login?id=seongbeen&pw=123456` 과 같이 ?으로 URL의 끝을 알리고 id(Key)의 값(Value)을 seongbeen, pw(Key)의 값(Value)을 123456으로 전송하여 개인정보 노출    
      - 전송하는 길이에 제한이 있다.   
         - URL의 길이가 정해져 있기 때문이다.       
      - 캐싱할 수 있다.   
         - 데이터를 노출시키는 경우는 개인정보가 포함되지 않는 상황에서 캐싱을 하여 속도를 높이거나 즐겨찾기를 편리하기 위해 사용되는 경우가 많다.   
      - 요청 후 멱등성을 가지며, 조작 대상의 자원의 상태를 변화시키지 않아 안정적이다.   
         - 멱등성(idempotence): 연산을 여러 번 적용하더라도 결과가 달라지지 않는 성질   
      
   - __POST__   
      - 값이나 상태를 추가 또는 수정하기 위해서 사용하는 방식   
         - ex) DB에 저장/수정 시에 DB의 값이 변경되게 하는 경우에 사용된다.   
      - URL에 데이터를 노출하지 않고 HTTP 패킷의 Body에 넣어 요청한다.
      - 데이터를 Body(바디)에 포함시킨다.   
         - Header안에 Body의 데이터를 설명하는 Content-type 헤더 필드도 들어가고 어떤 데이터 타입인지 명시해야 한다.    
      - URL에 데이터가 노출되지 않아서 기본 보안은 되어있다.   
      - 전송하는 길이에 제한이 없다.   
         - 데이터를 Body에 포함시켜 길이에 제한은 없지만 Time out이 있어 클라이언트에서 페이지를 요청하고 기다리는 시간이 존재한다.   
      - 캐싱할 수 없다.   
         - URL에 데이터가 노출되지 않으므로 즐겨찾기나 캐싱이 불가능하지만 쿼리스트링(문자열)데이터 뿐만 아니라, 라디오 버튼, 텍스트 박스와 같은 객체들의 값도 전송이 가능하다.   
      - 내부의 구분자가 각 파라미터를 구별하여 서버가 해석하므로 속도가 GET에 비해 느리다.   
      - PUT이나 DELETE 메소드와 같은 역할 수행이 가능하다.   
         
   - __GET과 POST의 차이__   
      - 보안, 전달 형식, 전달할 수 있는 데이터의 양   
      
   - __POST 방식이 GET 방식보다 보안 측면에서 더 좋은가?__   
      - POST든 GET이든 보내는 데이터는 전부 클라이언트 측에서 볼 수 있다. 단지 GET 방식은 URL에 데이터가 표시되어 별다른 노력없이 볼 수 있어서지, 두 방식 전부 보안을 생각한다면 암호화 해야 한다.   
      
   - __GET 방식이 POST 방식보다 속도가 빠른가?__    
      - 빠르다. 이유는 GET 방식의 요청은 캐싱 때문에 빠른 것이다.   

## 네트워크 주소 체계   
   - __IP 주소(IPv4)__    
      - __고유한 32bit 숫자__   
      - __가지고 있는 문제점__   
         - 주소 공간 부족   
         - 보안   
      - 8bit 4부분으로 나누어져서 각 부분이 10진수로 변환되어 255.255.255.255 처럼 마침표(.)로 구분하여 표현된다.
      - __호스트의 들어있는 네트워크 인터페이스를 가르키는 주소__   
      - __네트워크 인터페이스 카드를 여러 개 가질 경우 여러 개의 IP를 가질 수 있다.__    
         - ex) 라우터   
            - 인터페이스를 여러 개 가지고 있기 때문에 여러 개의 IP를 가진다.   
      - __IP를 무작위로 배정하지 않는 이유__   
         - 라우터의 forwarding 테이블이 너무 커져서 검색이 어려워질 수 있기 때문이다.   
      - __현재 IP주소를 배정하는 방법__   
         - __계층화__   
            - Network(= Prefix = Subnet) 24 bit, Host 8 bit 로 나눠진다.   
               - Network을 나타낼때는 /24로 표현한다.   
               - ex) IP 주소: 12.34.158.5 일경우 Network: 12.34.158.0/24   
            - IP주소는 항상 24 bit Subnet Mask와 같이 다닌다.   
               - Subnet mask를 통해 어디까지가 Network이고 Host인지 구별할 수 있다.   
                  - ex) IP 주소: 12.34.158.5 / Subnet mask: 255.255.255.0 를 AND 연산을 할 경우 Network 12.34.158.0만 알아낼 수 있다.   
               - Subnet으로부터 같은 Network에 속한 호스트들을 알아낼 수 있다.   
               - 같은 Network에 속한 호스트들은 같은 prefix를 가지기 때문에 라우터의 Forwarding 테이블이 단순해져 확장성(Scalability)이 증가된다.   
               - 라우터를 업데이트 할 필요없이(Forwarding table entry 추가 필요 x) 같은 Prefix를 가진 새로운 호스트를 쉽게 추가할 수 있다.   
                  - 라우터의 Forwarding table entry는 목적지 IP 주소가 매칭되는 prefix에 매핑하여 forwarding하기 때문이다.   

      - __과거 Class에 따른 IP 주소 배정__   
         - __Class A__   
            - Network을 나타낼때는 __/8__ 로 표현한다.   
               - Network는 8bit, Host는 24bit   
               - 전 세계의 2^8 = 128개의 기관만이 Network주소를 가질 수 있었고 각 기관이 Host 24bit 만큼 모두 사용하지 못하여 공간이 낭비되었다.   
         - __Class B__   
            - Network을 나타낼때는 __/16__ 로 표현한다.   
               - Network는 16bit, Host는 16bit   
               - Class A과 비슷하다.   
         - __Class C__   
            - Network을 나타낼때는 __/24__ 로 표현한다.   
               - Network는 24bit, Host는 8bit   
               - 전 세계의 2^24개의 기관이 Network주소를 가질 수 있으나 Host가 8bit이기 때문에 할당할 수 있는 Host의 수가 너무 적었다.      
      - __Subnets__   
            - __IP 주소의 같은 Subnet(Prefix)를 가진 장치 인터페이스 집합__   
            - __라우터를 거치지 않고 물리적으로 서로 접근이 가능한 Host들의 집합__    
            - __IP 주소__   
               - Subnet part - 높은 순위 bits    
               - Host part - 낮은 순위 bits   
            - __라우터는 여러 개의 인터페이스를 가지므로 여러 개의 IP를 가진다.__   
               - 각 IP 주소의 prefix가 다 다르다.   
                  - 라우터는 여러 개의 Subnet에 속하기 때문에(교집합) 다른 곳으로 가려면 라우터를 거쳐서 가야한다.   


### 관련 내용   
   - __Classless Inter-Domain Routing(CIDR)__   
      - __Class가 가진 문제점을 해결하기 위해 나온 방법__   
      - __Network를 8 bit 단위로 정하지 않고 32bit내 원하는 만큼 유연하게 __/n__ 로 정할 수 있다.__   
         - 하나의 기관이 Class C에서 1000개의 Host를 할당하기 위해서는 Network /24를 4개 받아야 한다. Host 8bit * 4개 = 1020개   
            - 여러 prefix를 가지기 때문에 라우터의 Forwarding table의 크기가 커진다.   
         - 하나의 기관이 CIDR에서 1000개의 Host를 할당하기 위해서는 Network /22를 1개 받으면 된다. Host 10bit * 1개 = 1024개   
            - 하나의 prefix만 가지기 때문에 라우터의 Forwarding table의 크기가 줄어든다.   

   - __Longest Prefix Match Forwarding__   
      - __목적지 기반 forwarding__   
         - Packet은 목적지 IP 주소를 가진다.   
         - __라우터는 가장 길게 매칭되는 prefix를 찾는다.__     
         - Cute algorithm problem: very fast lookup   

   - __IPv4의 문제점을 해결하기 위해 나온 방법__   
      - __IPv6__   
         - 1996년에 __IPv4의 주소 공간 부족 문제점을 해결__ 하기 위해서 나왔다.        
         - __128 bit__   
         - __IPv6 datagram 구조__    
            - ver, priority, flow lable   
            - payload length, next header, hop limit   
            - source address   
            - destination address   
            - data   

            __IPv4 -> IPv6__    
               - __Tunneling__   
                  - IPv4와 IPv6를 같이 사용하는 방식   
                  - IPv6 -> IPv4에게 전송할 때 IPv4 datagram 구조에 맞춰서 전송한다.   
                     - 즉 IPv6안에 IPv4가 들어있다.   

            __IPv6를 사용하는 것이 더 좋은 방법이기 때문에 전세계에 존재하는 라우터를 IPv4에서 IPv6로 변경하는게 좋지만 IPv4의 환경이 이미 전체적으로 자리잡고 있고 라우터는 각자 소유하고 있기 때문에 모든 라우터를 변경하는 것은 현실적으로 어렵다.__   



   - __개인이 인터넷을 사용하기 위해서 가지고 있어야 할 필수적인 정보__   
      - __IP, Subnet mask, Router, DNS__   
         - ex) __IP__ : 192.168.1.47, __Subnet mask__: 255.255.255.0, __Router__: 192.168.1.1, __DNS__: 192.168.1.1   

   - __ICMP(Internet Control Message Protocol)__   
      - __네트워크에서 일어나는 정보에 대해서 출발지에게 알려주기 위한 네트워크 자체가 만들어내는 프로토콜__   
         - __Packet이 목적지에 도착하지 못했을 경우__    
            - TTL이 0이 되었을 경우   
            - 목적지의 Port가 닫혀있을 경우   
            - IP header가 잘못됐을 경우   
         - __Traceroute__   
            - 목적지까지 거쳐가는 라우터의 정보를 알려준다.   
               - TTL 1, 2, 3, 4 등 점차 늘려가면서 각 라우터 순서를 알아낸다.   


## Packet switching, Circuit switching    
   - __데이터 전송 방식__   
      - __Circuit switching__   
         - 출발지에서 목적지까지의 길을 미리 지정하고 전송   
         - 데이터 전송 시작시점부터 종료시점까지 회선을 독점하기 때문에, 중간에 다른 데이터가 선점된 경로로 전달될 수 없다.   
         ex) 무선 전화   
         
      - __Packet switching__   
         - 출발지에서 목적지까지의 길이 변경되면서 전송   
         - Router를 거쳐 경로를 바꿔주어 더 빨리 데이터를 전송한다.   
            - 각 노드에서 라우팅 알고리즘을 사용하여 최단경로로 갈 수 있는 라우터를 선택한다.   
         - 공용선을 이용하는데 링크(회선)이 바쁘다면 다른 회선을 선택한다.   
         - 사용자가 보내는 데이터를 패킷 단위로 받아서 올바른 방향으로 전송, 모든 비트가 받아져야지만 다음 방향으로 전송   
            - 저장 후 전달(Store and Forward transmission) 방식이다.   
         ex) 인터넷, caravan analogy   

      - __Circuit switching VS Packet switching__   
         - 1mb/s link가 있고 각 유저가 active일 때 100kb/s를 보낸다고 가정할 경우   

         - Circuit switching 사용 시 최대 10명의 유저까지 지원   
         - Packet switching 사용 시 받는 대로 보내기 때문에 제한이 없어 일반적으로 많이 사용하지만 10명 이상이 몰릴 경우 문제가 생길 수 있다.   


## NAT   
   - __IPv4 with NAT__   
      - 2015년에 NAT(Network Address Tranlation)을 사용하여 기존의 IPv4 주소 공간 문제를 해결하였다.   
      - __NAT__   
         - IP 주소 재사용을 가능하게 한다.   
         - __local 네트워크는 오로지 한 개의 IP 주소만 가진다.__   
            - 바깥 세상과으로부터 독립   
               - 밖에 알리지 않고 local 네트워크에 있는 디바이스의 주소를 바꿀 수 있다.   
               - local 네트워크의 주소를 바꾸지 않고 ISP를 바꿀 수 있다.   
               - local net에 있는 디바이스는 바깥세상에 의해 명시적으로 addressable, visible하지 않는다.   
            - 하나의 네트워크 망에서 여러 개의 디바이스들이 존재할 때, 그들 각각에게 실제 IP를 하나씩 주게 되면 IP를 너무 많이 필요로 한다.   
               - IPv4 주소 공간이 부족하다.   

         - __NAT 라우터가 하는 기능__   
            - __Outgoing datagrams__   
               - 내부에서 외부로 나가는 datagram의 (출발지 IP 주소, Port 번호)를 (NAT IP 주소, 새로운 Port 번호)로 대체한다.   
            - __모든 (출발지 IP 주소, 포트 번호)->(NAT IP 주소, 새로운 포트 번호) 변환 쌍을 NAT translation table에 기록한다.__   
            - __Incoming datagrams__      
               - 외부에서 내부로 들어오는 datagram의 목적지 필드에 있는 (NAT IP 주소, 새로운 Port 번호)를 NAT translation table 내의 상응하는 (출발지 IP 주소, Port 번호)로 바꾼다.   

         - __외부에서 보기에는 하나의 IP 주소만 있는 것처럼 보인다.__   
         - __NAT 라우터는 ISP의 DHCP 서버로부터 IP 주소를 얻고, NAT-DHCP-라우터로 제어되는 홈 네트워크의 주소 공간에서 DHCP 서버를 실행하여 컴퓨터에게 주소를 제공__   
         - __16bit의 port-number field__   
            - 하나의 LAN 주소로 동시에 2^16 = 약 60,000개의 연결이 가능   
            - Flow   
               - [(WAN side addr) 138.76.29.7, 5001]이랑 [(LAN side addr) 10.0.0.1, 3345]를 바인딩한 것
            - 약 60,000개의 flow를 생성할 수 있다   

         - __NAT 문제점__   
            - __end-to-end 법칙을 위반__   
               - host가 중간 노드에서 Packet(IP 주소, Port 번호) 수정 없이 직접 통신해야 한다는 원칙 위반   
               - 라우터는 네트워크 계층 장치이므로 네트워크 IP datagram header만 봐야하는데 header의 IP 주소도 변경하고 data 부분의 TCP header의 Port 번호(전송 계층)도 변경한다.     
                  - 라우터는 네트워크, 데이터 링크, 물리적 계층까지만 처리해야 한다.   
            - __라우터는 Port를 확인하면 안된다__   
               - Port 번호의 용도는 수신자 호스트에서 프로세스를 찾을 때 사용되어야 하는데 라우터에서 호스트를 찾을 때 사용된다.   

         - __NAT를 사용하는 네트워크에서는 Server를 운영할 수 없다.__   
            - __Client는 오직 외부 IP 주소만 알 수 있다.__   
               - 그러므로 Client가 LAN의 로컬 주소(내부 IP 주소)를 목적지 주소로 사용할 수 없다.   
            - __Server는 Client의 요청을 기다리는 역할이므로 NAT 테이블이 만들어지지 않는다.__   
               - NAT 내부에서 외부로 나갈 때만, 주소 변환 쌍이 NAT 테이블에 업데이트 된다.   
               - NAT 내부에서 외부로 datagram이 나갈 때 생성된 NAT 테이블을 활용하여 외부 datagram이 내부로 들어온다.   
            - __내부 사설 IP는 사설 인터넷 내에서만 유효하기 때문에 외부에서 접근 불가__   
               - __해결책__   
                  - __특정 Server를 위해 관리자가 NAT 테이블에 Server 정보를 매핑해두는 방식__   
                     - 특정 Port로 들어오는 연결 요청을 Server로 전달(forward) 하도록 NAT을 정적으로 구성   
                  - __Universal Plug and Play(UPnP) Internet Gateway Device(IGD) Protocol__   
                     - UPnP 프로토콜을 사용하여 자동으로 NAT Port 매핑 테이블을 생성한다.   
                     - 호스트가 가까운 NAT을 발견하고 동적으로 Port 매핑을 자동 설정하는 프로토콜 사용   
                  - __Skype를 사용하여 전달(relaying)__   
                     - NAT의 Client와 릴레이로 연결을 설정   
                     - 외부 Client는 릴레이에 연결   
                     - 릴레이가 두 연결 사이에 패킷을 중계   
                     - Skype에서 사용   
                     
                     
