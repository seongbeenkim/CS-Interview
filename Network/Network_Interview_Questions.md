# :bookmark_tabs: Network Interview Questions   
   
* [REST API](#rest-api란-무엇인가)    
* [라우터에서 발생하는 Packet delay](#라우터에서-패킷-교환간-발생하는-delay)   
* [쿠키,세션,캐시](#쿠키세션캐시)   
* [HTTP와 HTTPS](#http와-https)   
* [브로드캐스트, 멀티캐스트, 유니캐스트](#브로드캐스트-멀티캐스트-유니캐스트)   
* [도메인과 DNS](#도메인과-dns)      
* [](#)   
* [](#)   
* [](#)   
* [](#)   
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
         - http는 텍스트 교환이다. html 페이지도 텍스트다. 바이너리 데이터로 되어있는 것도 아니고 단순 텍스트를 주고 받기 때문에 누군가 네트워크에서 신호를 가로채어 본다면 내용이 노출된다.   
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




