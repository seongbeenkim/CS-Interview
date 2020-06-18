# :bookmark_tabs: Network

* [1. 컴퓨터 네트워크 기본](#1-컴퓨터-네트워크-기본)   
   - [네트워크 구성 요소](#네트워크-구성-요소)   
   - [데이터 전송](#데이터-전송)   
   - [라우터에서 Packet delay](#라우터에서-packet-delay)   
   - [delay 줄이는 방법](#delay-줄이는-방법)   
   
* [2. 어플리케이션 계층](#2-어플리케이션-계층)   
   - [Web and HTTP](#web-and-http)   
   - [Web caches (Proxy server)](#web-caches)   
   - [FTP](#ftp)   
   - [DNS](#dns)   
   - [TCP 과정](#tcp-과정)   
   - [UDP 과정](#udp-과정)   
   
* [3. 전송 계층](#3-전송-계층)   
   - [Multiplexing&Demultiplexing](#multiplexingdemultiplexing)   
   - [RDT, ARQ, Pipelining](#네트워크-계층은-unreliable-channel이다)   
   - [TCP](#tcp)   
   
* [4. 네트워크 계층](#4-네트워크-계층)   
   - [라우터](#라우터)   
   - [IPv4 datagram 구조](#ipv4-datagram-구조)   
   - [IP Address(IPv4)](#ip-addressipv4)   
   - [Dynamic Host Configuration Protocol(DHCP)](#dynamic-host-configuration-protocoldhcp)   
   
* [5. 링크 계층](#5-링크-계층)   
   - [MAC](#broadcast-medium)   
   - [Ethernet](#ethernet)   
   
* [6. 무선과 이동 네트워크](#6-무선과-이동-네트워크)   
   - [Wireless](#wireless)   
   - [IEEE 802.11 Wireless LAN](#ieee-80211-wireless-lan)   
   - [Cellular Internet access](#cellular-internet-access)   
   - [Mobility](#mobility)   
   
* [7. 멀티미디어 네트워크](#7-멀티미디어-네트워크)   
   - [Multimedia](#multimedia)      
   
* [8. 네트워크 보안](#8-네트워크-보안)   
   - [Network security](#network-security)     
   - [Cryptography](#cryptography)   
   - [Securing TCP connections: SSL](#securing-tcp-connections-ssl)   
   - [Firewalls](#firewalls)   
   
## 1. 컴퓨터 네트워크 기본   
### 네트워크 구성 요소   
__라우터__ : 메세지를 전달받아서 데이터를 전달, Network, Data link, Physical 계층으로 구성   
__네트워크 엣지__ : 어플리케이션, 호스트 (노트북, 데스크탑)    
__네트워크 코어__ : 라우터, 네트워크   
__네트워크 접근, 물리적 장치__ : 통신 링크 (무선랜,wifi 등)   
   
#### Network edge       
__end system(hosts)__: 어플리케이션 실행   
ex) web, email   
__client/server model__: client host는 요청하고 항상 server로부터 서비스를 제공받는다.   
ex) Web browser/server, email client/server    

### 애플리케이션 구조

__client-server 구조__   
   - __server__:   
      - 모든 서버는 호스트이다   
      - 모든 호스트가 서버인 것은 아님   
      - 네트워크에 연결이 확립된 모든 장치는 호스트의 자격이 있는 반면, 다른 장치(클라이언트)로부터의 연결을 수락하는 호스트만 서버가 될 수 있다.   
      - 영구적인 IP 주소   
      - 스케일링을 위한 데이터 센터   
   - __client__:  
      - 서버와 통신  
      - 간헐적으로 연결되었을 수 있다   
      - 동적 IP 주소가 있을 수 있다. (껐다 키면 IP가 바뀔 수 있다)   
      - 서로 직접적으로 통신하지 않는다   

__peer-to-peer (P2P)__   
   - no always-on server  (상시 서버 없음)   
   - 임의의 end systems이 직접적으로 통신   
   - peer가 다른 peer에게 서비스를 요청하고 및 제공함 (동등한 관계)   
      - self scalability: 새로운 동료(peers) 새로운 service demand 및 새로운 service capacity를 가져온다.   
   - peers가 간헐적으로 연결되고 IP 주소를 변경한다   
      - 복잡한 관리   
   
__1. Connection-oriented service   
TCP(Transmission Control Protocol)__   
- 소켓 1:1 연결   
- 신뢰성있고 순서대로 데이터 전달   
- 데이터의 경계 존재 X   
- 흐름 제어 - 수신자의 수신 속도에 맞춰 송신자가 데이터 전달   
- 혼잡 제어 - 네트워크 혼잡 시 송신자의 데이터 전송률을 줄임    
- 3-way handshaking   
   
__2. Connectionless service   
UDP(User Datagram Protocol)__   
- 소켓 1:N 방식, 연결 X   
- no 신뢰성, 순서, 흐름 제어, 혼잡 제어   
- 데이터의 경계 존재 O, 한번의 전송할 수 있는 데이터 크기 제한   
   
- 대부분 TCP를 사용하며 처리해야 할 작업이 많기 때문에 비용이 더 비싸지만 UDP보다 속도가 느리다.   
   
__Packet__: 인터넷에서의 데이터 전송 단위, 비트들의 집합   
   
__Protocol__: 컴퓨터와 컴퓨터 사이, 또는 한 장치와 다른 장치 사이에서 데이터를 원활히 주고받기 위하여 약속한 통신 규약   
__Protocol Family__: 프로토콜 체계   
   - PF_INET: IPv4 인터넷 프로토콜 체계    
   - PF_INET6: IPv6 인터넷 프로토콜 체계   
   - PF_LOCAL: 로컬 통신을 위한 UNIX 프로토콜 체계   
   - PF_PACKET: Low Level 소켓을 위한 프로토콜 체계   
   - PF_IPX: IPX 노벨 프로토콜 체계   
   
### 데이터 전송   
__데이터 전송 방식__   
__1. Circuit switching__   
- 출발지에서 목적지까지의 길을 미리 지정하고 전송   
ex) 무선 전화   

__2. Packet switching__   
- 사용자가 보내는 데이터를 패킷 단위로 받아서 올바른 방향으로 전송, 모든 비트가 받아져야지만 다음 방향으로 전송   
ex) 인터넷, caravan analogy   
   
__Circuit switching VS Packet switching__   
- 1mb/s link가 있고 각 유저가 active일 때 100kb/s를 보낸다고 가정할 경우   
   
Circuit switching 사용 시 최대 10명의 유저까지 지원   
Packet switching 사용 시 받는 대로 보내기 때문에 제한이 없어 일반적으로 많이 사용하지만 10명 이상이 몰릴 경우 문제가 생길 수 있다.   

### 라우터에서 Packet delay   
__1. nodal processing delay(처리 지연)__: 라우터에서 패킷을 받았을 때 검사(최종 목적지가 어디인지 다음 라우터는 무엇인지)하고 output link를 결정한다.   
__2. queuing delay(큐잉 지연)__: output link에 전송될 때까지 큐에서 대기하는 시간, 라우터의 혼잡 레벨에 따라 다르다.   
__3. transmission delay(전송 지연)__: 전송 시 걸리는 delay, output link로 첫번째 비트가 나가는 순간부터 마지막 비트가 나가는 순간까지 걸리는 시간   
- R = 링크 대역폭(bps), L = 패킷 크기(bits), L/R = 비트를 링크로 보내는데 걸리는 시간   
- R이 클수록 transmission delay가 적게 걸린다.   

__4. propagation delay(전파 지연)__: 데이터가 output link로 전달된 후 다음 라우터까지 도달하는데 걸리는 시간    
- d = 물리적 링크의 길이, s = propagation 속도, propagation delay = d/s   
- s는 전파이기 때문에 조절할 수 없으며, d가 짧을 수록 propagation delay가 적게 걸린다.   
   
### delay 줄이는 방법   
   - __1. Nodal processing delay(처리 지연)__   
      - 좋은 성능의 라우터를 사용   
   - __2. Queuing delay(큐잉 지연)__   
      - x, 인터넷 사용량이 항상 다르기 때문이다.   
   - __3. Transmission delay(전송 지연)__   
      - 링크 대역폭을 확장   
   - __4. Propagation delay(전파 지연)__   
      - 물리적 링크 변경

   - __queue의 크기는 제한적이기 때문에 꽉 찬 상태에서 계속하여 패킷이 온다면 패킷 손실이 발생한다.__   
      - __TCP에서 패킷 손실이 일어날 경우 어떻게 재전송을 하는가?__   
         1. 이전 라우터가 직접 재전송하는 방법
         2. 처음부터 재전송   
            - __2번의 방법으로 처음부터 재전송을 한다. -> 현재 인터넷 디자인__   
            - __1번의 방법을 사용하지 않는 이유__ - 라우터는 빠른 속도로 전달해야하기 위해 해야할 작업이 많으므로 이 전달을 위한 목적으로만 사용을 하기 때문이다.   
            - dumb core: TCP 사이의 모든 라우터를 지칭 (TCP - 라우터 - 라우터 - 라우터 ... - TCP)
   
__peer-peer model__: minimal use of dedicated servers   
ex) skype, bittorrent, kazaa   
   
## 2. 어플리케이션 계층   
우리가 사용하는 프로세스(프로그램)가 존재하는 계층  
프로세스는 app 개발자에 의해 통제되며 Socket을 통해 transport 계층과 연결된다.   
어플리케이션 계층을 제외한 transport, network, link, physical 계층은 OS에 의해 통제되며 physical 계층끼리 인터넷을 통해 통신한다.   

- __Processes communicating__   
   - __process__: 호스트 내에서 실행 중인 프로그램   
      - 같은 호스트 내에서, 두 개의 프로세스들이 (OS에 의해 정의된) inter-process communication를 사용하여 통신한다.   
      - 서로 다른 호스트의 프로세스가 메시지를 교환하여 통신함   
   - __clients, servers__   
      - client process: 통신을 시작하는 프로세스   
      - server process: 연락을 기다리는 프로세스   
   - __aside__:   
      - P2P 구조를 사용하는 애플리케이션은 client processes & server processes를 가진다.   
- __Server__   
   - 인터넷에 상시 접속된 host   
   - 영구적인 IP 주소   
   - Scailing을 위한 data 중심지   
- __Client__   
   - Server와 통신   
   - 간간이 연결
   - 동적 IP 주소   
   - 직접적으로 통신하지 않음   
- __Socket__   
   - 어플리케이션 계층과 네트워크 계층 사이에 인터페이스   
       - 어플리케이션은 소켓을 생성   
       - 소켓 type을 지정   
         - TCP 또는 UDP   
            reliable vs best effort   
            connection-oriented vs connecttionless   
   - 프로세스는 소켓으로부터 메시지를 받고, 보낸다.   
   - Server와 Client가 통신할 수 있게 이어주는 것   
   - IP 주소(어떤 컴퓨터인지 나타냄), Port 번호(해당 컴퓨터의 어느 프로세스인지 나타냄)를 가짐   
   
__web의 Port 번호를 80로 통일하지 않으면 DNS에서 IP뿐만 아니라 Port번호까지 처리해야한다__   
   
보통 상위 계층은 하위 계층의 서비스가 필요하다.   
- __어플리케이션 계층이 요구하는 전송 계층의 서비스__   
   - __Data integrity(데이터 무결성)__   
      - 100% 안정된 데이터 전송 필요     
      ex) 파일 전송, web 거래   
      but, 일부 audio와 같은 app은 데이터 일부 손실 괜찮음   
   - __Timing(시간)__   
      - 효율적이기 위해 낮은 delay 필요   
      ex) 시간 내에 목적지 도착, interactive game   
   - __Throughput(처리량)__   
      - 효율적이기 위해 최소 처리량 필요   
      ex) multimedia   
      elastic app 같은 경우 얻는 만큼의 처리량을 활용   
   - __Security(보안)__   
      - 암호화, 데이터 무결성 등   
      
   __but, 전송 계층은 오직 Data integrity 서비스(from TCP)만 어플리케이션 게층에 제공__   
   
__대표적인 어플리케이션 : 전송 프로토콜__   
- __e-mail__ - SMTP(RFC 2821) : TCP   
- __remote terminal access__ - Telnet(RFC 854) : TCP   
- __web__ - HTTP(RFC 2616) : TCP   
- __file transfer__ - FTP(RFC 959) : TCP
- __streaming multimedia__ - HTTP(e.g., Youtube), RTP(RFC 1889) : TCP or UDP   
- __internet telephony__ - SIP, RTP, proprietary(e.g., Skype) : TCP or UDP   

### Web and HTTP   

__Web__   
   - web page는 객체들로 구성   
   - 객체는 HTML 파일, JPEG 이미지, Java applet, audio file 등이 있다.   
   - web page는 다수의 참조된 객체를 포함하는 기반 HTML 파일로 구성   
   - 각 객체는 URL에 의해 자체 주소를 가진다.   
      
__HTTP(Hypertext Transfer Protocol)__   
   - __Hypertext__: 링크를 가진 text   
   - Web의 어플리케이션 계층의 프로토콜   
   - __Client/Server 모델__   
      - Client: HTTP를 사용하여 request, receive하는 browser이며 Web의 객체들을 보여준다.   
      - Server: Web Server가 HTTP를 사용하여 request에 응답하는 객체를 보낸다.   
   - __stateless__: 과거 client의 request에 대한 정보를 유지하지 않는다.   
         
   __HTTP request, response 하기 전에 TCP 연결을 먼저 해야한다.__   
   
   - __non-persistent HTTP__: TCP를 연결을 통해 최대 하나의 객체가 전달된 후 TCP 연결이 끊긴다. 다운로드하는 다수의 객체는 다수의 TCP 연결이 필요하다.   
      - HTTP 1.0이 사용   
      - RTT: 작은 패킷이 client에서 server로 갔다 다시 올 동안의 시간   
      - non-persistent HTTP response time = 2RTT + 파일 전송 시간
      ex) TCP 연결을 위한 RTT 하나, HTTP request와 반환 받아야할 response를 위한 RTT 하나, file 전송 시간   
      - non-persistent HTTP issues:   
         - object 당 2번의 RTT가 걸림   
         - 각 TCP 연결을 위한 OS 오버헤드   
         - 브라우저가 참조된 object를 가져오기 위해 종종 병렬 TCP 연결을 연다   
      
   - __persistent HTTP__: 하나의 TCP 연결을 통해 다수의 객체가 전달될 수 있다.   
      - HTTP 1.1이 사용   
      - Pipelining 기법: 다수의 객체를 요청할 경우 요청할 수 만큼 연속적으로 request를 하고 연속적으로 response 받는 방식을 사용하여 client와 server간 요청과 응답의 효율성을 높인다.   
      
   __일반 web은 persistent HTTP pipelining 기법을 사용한다__   
   
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

### Web caches   
   - 목표: Origin Server를 거치지 않고 Client request에 대한 response를 해주는 것   
      - 사용자 브라우저를 설정: Proxy server에 요청된 내용들에 대한 캐시를 통한 웹 접근   
      - __Proxy Server__: 클라이언트가 자신을 통해서 다른 네트워크 서비스에 간접적으로 접속할 수 있게 해주는 컴퓨터   
      - __서버와 클라이언트 사이에서 중계기로서 통신을 수행하는 기능을 가리켜 proxy__ 라고 한다.   
      - 브라우저는 모든 HTTP 요청을 캐시로 보낸다   
         - 캐시의 객체: 캐시는 객체를 반환   
         - 저장되어 있는 데이터가 없을 경우 캐시는 원래 서버에서 객체를 요청한 다음 객체를 클라이언트에게 반환   
      - Origin server를 통해 생긴 데이터(캐시)는 Proxy server에 저장되고 같은 request를 받았을 경우 origin server를 거치지 않고 Proxy server에서 데이터를 response한다.   
      - 캐시는 웹 페이지를 디스플레이하기 위해 다운로드한 데이터의 콜렉션이다.   
      - __Why Web caching?__    
         - 클라이언트 요청에 대한 응답 시간을 줄임   
         - 기관의 접근 링크에서 트래픽을 줄인다   
         - 캐시가 있는 인터넷 밀도: "poor" 컨텐츠 제공 업체가 효과적으로 컨텐츠를 제공 할 수 있다(P2P 파일 공유도 마찬가지)   
         - bandwidth 사용을 줄인다.   
         - access link 속도를 늘리는 것보다 비용이 적게 들고 빠르다.   
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

### FTP   
   - File Transfer Protocol
   - TCP/IP 프로토콜을 가지고 서버와 클라이언트 사이의 __파일 전송__을 하기 위한 프로토콜이다.   
   - client/server model   
      - client: 전송을 시작하는 쪽   
      - server: 원격 호스트   
   - FTP: RFC 959   
   - FTP server: Port 21   
      - FTP server는 "state"를 유지한다   
         - ex) 현재 디렉토리, 이전 인증   
   - HTTP와 달리 연결의 종류는 2가지가 있다.   
      - Control Connection: 먼저 제어 Port인 Server 21번 포트로 사용자 인증, 명령을 위한 연결이 만들어지고, 연결을 통해 Client에서 지시하는 명령어가 전달된다.   
      - 데이터 전송용 연결: 실제의 파일 전송은 필요할 때 새로운 연결이 만들어 진다.   
   - 제어 연결(control conection)을 통해 승인 된 클라이언트   
   - 클라이언트가 원격 디렉토리를 탐색하고 제어 연결을 통해 명령을 보냄   
   - Server가 파일 전송 명령을 받으면, Server는 Client와의 두번째 TCP 데이터 연결을 연다.   
   - 하나의 파일을 전송 한 후, 서버는 데이터 연결을 닫는다.   
   - 다른 파일을 전송하려면, 서버는 또 다른 TCP 데이터 연결을 열어야한다.   
   
### DNS   
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
      - DNS 응답 메세지(5개 영역 있음)    = Header + ( 질의 + 응답 + 책임 + 부가정보 )   

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
         2 - Client가 __Root server__ 에 쿼리하여 __.com DNS server__ 를 찾는다.   
         3 - Client가 __.com DNS server__ 에 쿼리하여 __amazon.com DNS server__ 를 찾는다.   
         4 - Client는 __amazon.com DNS server__ 에 쿼리하여 __`www.amazon.com`의 IP 주소__ 를 얻는다.      
      
         - __DNS name resolution example__   
            - 1~4 모든 과정은 각 과정에서 해결이 안됐을 시 진행된다고 가정한다. 해당 과정에서 IP 주소 발견하면 IP 주소 반환한다.   
            - __반복적 질의(iterative query)__   
               - 질의된 도메인에 대해 응답하거나, 아니면 이 작업을 할 수 있는 다른 DNS 서버에 Client를 연결시켜 주는 작업      
                  - 자신이 관리하지 않는 질의 요청에 대해 응답 가능한 name server 목록 전달   
                  - Client는 다수의 DNS 서버들에게 같은 질의를 반복할 수 있게 된다.   
               - local DNS server가 상위 DNS 서버에게 쿼리를 보내어 IP 주소를 구하는 작업   
               
               1 - 호스트가 local DNS 서버한테 도메인의 IP 주소 질의   
               2 - 모를 경우 local DNS server가 root DNS server한테 질의   
               3 - 모를 경우 local DNS server가 TLD DNS server에 질의    
               4 - 모를 경우 local DNS server가 authoritative DNS 서버에 질의   
               5 - 모를 경우 에러 메세지 반환   
               5-1 - 찾을 경우 authoritative response 반환하고 local DNS server는 도메인 IP 주소를 캐싱하고 Client server에 도메인의 IP 주소 전달   
               - __장점__   
                  - local DNS가 데이터를 많이 가지고 있어 빠르게 받을 수 있다.   local DNS가 바쁘다.   
               - __단점__   
                  - local DNS가 바쁘다.   
                  
            - __재귀적 질의(recursive query)__   
               - 질의된 도메인에 대해 즉각 응답하거나, 다른 DNS 서버에게 질의한 결과로 응답하거나, 찾고 있는 정보가 없다는 에러 메시지를 보내준다.         
                  - 연결할 수 있는 name server에 name 확인 부담을 준다   
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
            - 최근 name-address 변환 쌍인 local 캐시를 가짐   
            - proxy 역할을 하며 쿼리를 계층으로 전달   
            
      - __Root name servers__   
         - 번역되지 않는 name을 root name server에 질의   
         - 인터넷의 DNS Root 영역의 name server다.   
         - 루트 영역의 레코드 요청에 직접 답하고, 적절한 최상위 도메인(TLD)에 대한 권한 있는 name server 목록을 반환하여 다른 요청에 응답한다.   
         - Root name server는 사람이 판독 가능한 호스트 name을 인터넷 호스트 간의 통신에 사용되는 IP주소로 번역하는 첫 번째 단계이기 때문에 인터넷 인프라의 중요한 부분이다.   
      - __TLD & authoritative servers__   
         - __TLD(Top-Level Domain) server__   
            - com, org, net, edu, aero, jobs, museums 및 모든 top-level country domains 예: uk, fr, ca, jp   
            - Network Solution 사가 com의 TLD 서버를 관리   
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
      __ 다수의 name server가 있을 경우 RTT가 가장 짧은 name server에 DNS 질의 메세지를 보낸다.   



   
       
### TCP 과정   
__TCP Server__
   1. __socket()__ - socket 생성   
      - domain: PF_INEF for IPv4, type: Sock_STREAM(TCP), Sock_DGRAM(UDP), protocol: 0 or IPPROTO_TCP   
      __첫 번째, 두 번째 인자로 전달된 정보를 통해서 소켓의 프로토콜이 결정되기 때문에 0을 전달해도 된다.__   
   2. __bind()__ - IP 주소와 port와 socket 연결   
      - sockfd: socket fd, *myaddr: INADDR_ANY(IP 주소, Port번호 포함), addrlen: sizeof(struct sockaddr_in) 주소 구조 길이   
   3. __listen()__ - socket을 listen용으로 사용, non-blocking   
      - sockfd: socket fd, backlog: listen하기 위해 연결 큐에 최대 담을 수 있는 수   
   4. __accept()__ - Client가 연결 요청 받을 때까지 block   
      - sockfd: socket fd, *cliaddr: Client의 IP주소, Port번호, addrlen: sizeof(struct sockaddr_in) 주소 구조 길이   
   5. __read(),write()__ - Client와 연결 후 사용   
      - write() - blocking, data가 보내진 후에만 return   
         - sockfd: socket fd, *buf: data 버퍼, nbytes: write할 byte의 수   
      - read() - blocking, data가 받아진 후에만 return    
         - sockfd: socket fd, *buf: data 버퍼, nbytes: read할 byte의 수   
   6. __close()__ - socket 종료, Port를 해방하고 TCP 연결 종료   
   
__TCP Client__   
   1. __socket()__ - socket 생성   
      - domain: PF_INEF for IPv4, type: Sock_STREAM(TCP), Sock_DGRAM(UDP), protocol: 0 or IPPROTO_UDP   
      __첫 번째, 두 번째 인자로 전달된 정보를 통해서 소켓의 프로토콜이 결정되기 때문에 0을 전달해도 된다.__   
   2. __connect()__ - TCP 3-way handshaking 과정을 통해 Server와 연결, blocking      
      - sockfd: socket fd, *servaddr: Server의 IP주소, Port번호, addrlen: sizeof(struct sockaddr_in) 주소 구조 길이   
   3. __write(),read()__ - Server와 연결 후 사용   
   4. __close()__ - socket 종료, Port를 해방하고 TCP 연결 종료   
   __Client가 bind()를 사용하지 않는 이유는 자신의 IP와 Port번호를 미리 알 필요가 없기 때문이다.   
   따라서 대부분 Client는 시스템이 자동적으로 배정하는 Port 번호를 사용한다.   
   (원한다면 사용할 수 있으나 Client 프로그램이 하나의 컴퓨터에서 두 개 이상 실행될 시 포트 번호 중복 에러가 발생할 가능성이 있다.)__   
   
### UDP 과정   
__UDP Server__
   1. __socket()__ - socket 생성   
      - domain: PF_INEF for IPv4, type: Sock_STREAM(TCP), Sock_DGRAM(UDP), protocol: 0   
   2. __bind()__ - IP 주소와 port와 socket 연결   
      - sockfd: socket fd, *myaddr: INADDR_ANY(IP 주소, Port번호 포함), addrlen: sizeof(struct sockaddr_in) 주소 구조 길이   
   3. __sendto,recvfrom()__ - 데이터 송수신   
      - sendto()   
         - sockfd: socket fd, *buf: 전송할 data 버퍼의 주소 값, nbytes: write할 byte의 수, flags: 옵션 지정에 사용되는 매개변수, 지정할 옵션이 없다면 0 전달, *to: 목적지 주소정보를 담고 있는 sockaddr 구조체 변수의 주소 값, addrlen: to로 전달된 주소 값의 구조체 변수 크기   
         __UDP 소켓은 연결의 개념이 있지 않으므로 데이터를 전송할 때마다 목적지에 대한 정보를 전달해야 한다.__   
      - recvfrom()   
         - sockfd: socket fd, *buf: 전송할 data 버퍼의 주소 값 전달, nbytes: read할 byte의 수, flags: 옵션 지정에 사용되는 매개변수, 지정할 옵션이 없다면 0 전달, *from: 발신지 주소정보를 담고 있는 sockaddr 구조체 변수의 주소 값, addrlen: from로 전달된 주소 값의 구조체 변수 크기   
         __UDP 소켓은 연결의 개념이 있지 않으므로, 데이터의 전송지가 둘 이상이 될 수 있다. 따라서 데이터 수신 후 전송지가 어디인지 확인할 필요가 있다.__   
         
__UDP Client__   
   1. __socket()__ - socket 생성   
   2. __sendto,recvfrom()__ - 데이터 송수신   
      - sockfd: socket fd, backlog: listen하기 위해 연결 큐에 최대 담을 수 있는 수  
   3. __close()__ - socket 종료   
   __UDP는 데이터의 경계가 존재하기 때문에 한번의 recvfrom 함수 호출을 통해서 하나의 메시지를 완전히 읽어 들인다.   
   sendto 함수호출 시 IP와 PORT 번호가 자동으로 할당되고 recvfrom을 통해 발신자의 IP와 Port번호를 알 수 있기 때문에 일반적으로 UDP의 프로그램에서는 주소정보를 할당하는 별도의 과정이 불필요하다.__   

__unconnected UDP 소켓의 sendto 함수 호출과정__   
1. UDP 소켓에 목적지의 IP와 Port번호 등록   
2. 데이터 전송   
3. UPD 소켓에 등록된 목적지 정보 삭제   
__1~3단계를 매번 거친다.__   

__connected UDP 소켓의 sendto 함수 호출과정__   
1. Connect   
2. 데이터 전송    
__connected UDP 소켓은 TCP와 같이 상대 소켓과의 연결을 의미하지는 않는다. 그러나 소켓에 목적지에 대한 정보는 등록이 된다.   
그리고 connected UDP 소켓을 대상으로는 read, write 함수의 호출이 가능하다. 이를 통해 성능을 향상 시킬 수 있다.__   

__NAT은 UDP의 특징을 이용하여 작동중인 네트워크의 내부 호스트에게 데이터를 전달하는 방법으로 UDP 홀펀칭을 사용한다.__   
[https://nenunena.tistory.com/61]
1. 외부에 있는 중계자에게 NAT 네트워크 내부 호스트A가 UDP 패킷을 전송한다.   
2. 패킷을 받은 중계자는 NAT를 통해 바뀐 공인 IP와 포트 번호를 알게 된다.   
3. 다른 호스트B가 중계자에게 접속하여(TCP or UDP) 중계자가 가지고 있는 NAT내부 호스트A의 바뀐 IP와 포트번호를 전달 받는다.   
4. 호스트B는 알아낸 IP와 포트번호로 UDP 패킷을 전송하면 공유기가 NAT 매핑정보를 보고 호스트A의 내부 IP와 포트번호로 패킷을 전달한다.   
   
   
## 3. 전송 계층   
### Multiplexing&Demultiplexing   
__각 계층의 전송 단위__
   - 어플리케이션 계층 - __Message__ = head + data(원하는 요청)   
   - 전송 계층 - __Segment(32bit)__ = head(출발지 port, 목적지 port 필드 등) + data(Message)   
   - 네트워크 계층 - __Packet__ = head(IP) + data(Segment)   
   - 데이터 링크 계층 - __Frame__ = head + data(Packet)   
   물리적 계층 - Router = head + data(Frame)를 통해서 상대방의 물리적 계층을 전송   
   
__Multiplexing__   
   - 출발지 호스트에서 소켓으로부터 데이터를 전달받아 데이터를 모으고, 이를 세그먼트 단위로 묶어 생성하기 위해서 세그먼트 앞에 헤더를 붙여 캡슐화하고 세그먼트를 네트워크 계층으로 전달하는 작업   
   - TCP or UDP Multiplexing   
      - 전송 계층은 어플리케이션 데이터, 출발지 Port번호, 도착지 Port번호, 그리고 기타값을 포함하는 전송 계층 세그먼트를 생성하고 네트워크 계층으로 전달한다.   
      네트워크 계층은 세그먼트를 IP 데이터그램으로 캡슐화하고 ‘최선형’ 전달 서비스로 세그먼트를 수신 호스트로 전달   
         
__Demultiplexing__   
   - 목적지 호스트에서 전송 계층은 네트워크 계층으로부터 세그먼트를 수신하고 세그먼트의 데이터를 헤더 정보를 사용해서 여러 소켓 중 올바른 소켓(어플리케이션 계층에 있는 프로세스)으로 전달하는 작업   
      1. TCP Demultiplexing   
         - 수신 호스트는 세그먼트 안의 목적지 IP/Port번호을 검사하고, 해당 세그먼트를 IP/Port번호로 식별되는 소켓에 전달   
         __출발지 IP/Port번호, 목적지 IP/Port번호 4개의 요소로 소켓 결정하므로 동일한 출발지 IP/Port번호, 목적지 IP/Port번호를 가지면 여러 개의 세그먼트라할지라도 같은 목적지 소켓을 통해 동일한 프로세스로 전달된다.__  
      2. UDP Demultiplexing   
         - 수신 호스트는 세그먼트 안의 목적지 Port번호을 검사하고, 해당 세그먼트를 Port번호로 식별되는 소켓에 전달   
         __목적지 IP/Port번호 2개의 요소로 소켓 결정하므로 동일한 목적지 IP/Port번호를 가지면 여러 개의 세그먼트라할지라도 같은 목적지 소켓을 통해 동일한 프로세스로 전달된다.__   

__Web은 Port 번호 80을 가지고 있기 때문에 Client가 서버로 세그먼트를 보내면 모든 세그먼트는 목적지 Port번호 80을 가지고 있다.   
     그러므로 출발지 IP/Port번호도 검사하는데 세그먼트가 많을수록 연결 소켓도 많이 생성하며, 각각의 연결에 따라서 새로운 프로세스를 많이 생성하는데 오늘날은 연결소켓과 스레드를 생성하는 방식을 통해 HTTP 요청과 HTTP 응답을 한다.__   

__UDP__   
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
   
### 네트워크 계층은 Unreliable Channel이다.   
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
         - 송신자가 ACK 또는 NAK을 제대로 받지 못하게 되기 때문에 보냈던 데이터를 다시 수신자에게 보내게 된다. 이러한 경우 수신자는 다시 받은 데이터가 새로운 데이터인지 아니면 중복된 데이터인지 알 수 없기 때문에 __Sequence Number를 사용하여 Packet을 구분__하게 할 수 있다.   
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
                     - 0,1,2,3 Packet 4개를 보냈는데 0에 대한 ACK를 받을 경우 1,2,3,4로의 범위로 바뀌고 4에 대해서 전송하고 Timer를 켜고 1에 대한 ACK를 받을 경우 2,3,4,5의 범위로 바꾸고 5를 전송하고 Timer를 키는데 2에 대한 ACK을 받지 못하고 2에 대한 Timer 제한 시간이 다 된 경우 2~5를 다시 재전송한다.   
                     - __즉 Packet loss 일어날 시 n만큼 돌아와서 전부 다시 보낸다.__    
                        
               - __Selective Repeat__   
                  - Window size n만큼 Feedback받지 않고 전송할 수 있다.   
                     - Window size n만큼 전송시 각 Packet의 Timer가 켜지고 각 Packet의 제한 시간 내 ACK을 받지 못할 경우 ACK을 받지 못한 Packet만 재전송한다.   
                  - ACK N: N을 잘 수신했다는 의미   
                  - 수신자는 정확히 받은 각 Packet에 대해서만 ACK을 송신자에게 보낸다.   
                     - 받은 Packet은 버퍼에 반드시 저장해야한다.   
                        - 손실된 패킷 이후 정확히 받은 다른 패킷들은 수신자의 손실된 패킷 자리를 비워놓고 recv 버퍼에 저장해놓은다.   
                        - 버퍼가 순서대로 꽉 차게 되면 어플리케이션 계층에 넘겨주고 마지막 받은 Packet에 대한 ACK을 송신자에게 보내고 버퍼를 비운다.   
                     - 0,1,2,3 Packet 4개를 보냈는데 0에 대한 ACK를 받을 경우 1,2,3,4로의 범위로 바뀌고 4에 대해서 전송하고 Timer를 켜고 1에 대한 ACK를 받을 경우 2,3,4,5의 범위로 바꾸고 5를 전송하고 Timer를 키는데 2에 대한 ACK을 받지 못하고 2에 대한 Timer 제한 시간이 다 된 경우 2~5를 다시 재전송한다.   
                     - __즉 Packet loss 일어난 Packet만 다시 보낸다.__   
                     - But, Sequence number가 계속 증가하는데 Sequence number의 값의 크기는 최대한 작게 하는 것이 좋다.   
                        - __그러므로 Window size가 n일 경우 Sequence number의 범위를 2n으로 할 경우 재전송할 Packet에 대한 혼선없이 전송이 가능하다.__   
                     - __문제점은 모든 Packet이 Timer를 가져야하기 때문에 Window size가 클 경우 복잡해지기 때문에 잘 사용하지 않는다.__   
                        - 그래서 __보통 Window를 대표하는 Timer를 하나__를 가지고 사용한다.   

### TCP   
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
   - __흐름 제어__   
      - 수신자가 받을 수 있는 만큼만 송신자가 데이터 전송하기 위한 제어   
      - 수신자의 버퍼가 얼마만큼 비어있는지 송신자에게 알려줘야한다.   
         - TCP header의 recv_buff 필드에 저장해서 전송한다.   
         - 송신자는 수신자의 recv_buff가 비어있는만큼 데이터를 전송한다.   
            - 보내는 양이 많으면 보내는 속도가 빠르고 보내는 양이 적으면 보내는 속도가 느리다.   
         - 수신자의 app에서 읽기 버퍼로부터 데이터를 읽지 않을 경우 계속해서 쌓인다.   
            - 빈 공간이 하나도 없게될 경우 TCP header의 recv_buff에 0 byte를 저장해서 전송한다.   
            - 송신자는 기다리지 않고 주기적으로 데이터 없이 또는 1 byte를 넣어 의미없는 Segment를 보내어 수신자의 ACK를 받아 수신자의 읽기 버퍼 상태를 확인한다.   
   - __혼잡 제어__   
      - 송신자와 수신자 사이의 네트워크가 수용할 수 있는 만큼만 데이터 전송하기 위한 제어   
      __네트워크가 막힐 경우 TCP 데이터가 제대로 전달되지 않아 재전송을 하는 특성 때문에 네트워크를 더 막히게 하여 TCP는 무너지게 된다.__   
      - 어떻게 Public 네트워크 막히지 않게 할 수 있을까?   
         - 네트워크가 혼잡할 경우 보내는 데이터의 양을 줄이고 한가할 경우 보내는 데이터의 양을 늘린다.   
         - 네트워크의 상태를 확인하는 방법   
            - __Network-assisted__   
               - 네트워크의 라우터들이 상태를 알려준다.   
               __라우터들은 데이터를 전송하느라 바쁘기 때문에 이 방법은 이상적인 방법이다.__   
            - __End-end__   
               - 현재 인터넷의 구현 방식   
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
               - TCP 두번째 버전으로 현재 인터넷에서 사용   
               - Packet loss이 일어나는 상황   
                  1. timeout   
                     - threshold를 Window size/2로 설정하고 Window size를 threshold로 설정 후 1씩 증가하며 다시 시작   
                  2. 같은 ACK를 4번 받을 경우   
                     - threshold를 Window size/2로 설정하고 Window size를 1 MSS로 설정 후 시작. 즉, Slow start부터 다시 시작   
                  timeout 상황이 더 위험한 상황이다.   
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
      
## 4. 네트워크 계층   
   - 송신자에서 Segment를 datagram로 변환   
   - 수신자에서 Segment를 전송 계층에 전달   
   
### 라우터   
   - 라우터는 Packet을 목적지까지 전송하기 위해서 Network, Link, Physical 계층을 가지고 있다.   
   - __Forwarding__   
      - __Packet header에 들어있는 목적지 주소가 라우터 테이블(포워딩 테이블)을 참조하여 올바른 목적지를 찾아 전달하는 것__   
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

   - __Name Server, DHCP Server, NAT, Firewall__   
      - Gateway IP 주소를 가지고 있을 뿐만 아니라 NAT 기능, Name Server, DHCP Server, 방화벽도 가지고 있다.   

   - __Interconnected ASes__   
      - 네트워크끼리의 연결을 위한 라우팅 알고리즘   
      - 포워딩 테이블은 intra-AS와 inter-AS 라우팅 알고리즘에 의해 설정된다.   
         - intra-AS은 내부 목적지 엔트리를 설정하고   
         - inter-AS & intra-AS는 외부 목적지 엔트리를 설정한다.   


### IPv4 datagram 구조   
   - 32bit   
   - __ver__: IP Protocol version number, __head len__: header 길이(bytes), __type of service__: 데이터 타입, __length(16bit)__: datagram 길이(bytes)   
   - __16-bit 식별자__, __flags__: 뒤에 분리된 datagram의 존재 여부, __fragment offset__ => __for reassembly or fragmentation__   
      - 네트워크 링크는 MTU(Max Transfer Size)를 가진다.   
         - WIFT, LAN 등 네트워크 링크의 타입이 다르고 MUT도 다르다.   
         - IP datagram에 비해 링크의 MTU가 작을 경우 IP datagram를 보낼 수 없다.   
         - 그러므로 IP datagram를 MTU size에 맞춰 다수의 IP datagram로 분리하여 보낸다.   
         - IP header의 bits가 순서를 확인하기 위해 사용되며 최종 목적지에서 분리된 IP datagram이 하나로 다시 조합된다.   
         - ex) 4000 bytes datagram, MTU = 1500 bytes일 경우   
            - header = 20 bytes, data = 3980 bytes   
            - 최초 datagram: length = 4000, ID = x, flags = 0, fragment offset = 0      

            - 분리1 datagram: length = 1500, ID = x, flags = 1, fragment offset = 0   
               - header = 20, data = 0~1480   
            - 분리2 datagram: length = 1500, ID = x, flags = 1, fragment offset = 185    
               - header = 20, data = 1480 ~ 2960,   1480/8 = 185 = offset
               - /8을 하여 필드의 3 bits를 줄여준다.   
            - 분리3 datagram: length = 1040, ID = x, flags = 0, fragment offset = 370   
               - header = 20, data = 2960 ~ 4440,   2960/8 = 370 = offset   
         - 중간의 분리된 IP datagram이 손실된다면 reassembly가 안되어 제대로 전송이 안되기 때문에 TCP Timer에 의해 재전송이된다.    

   - __TTL(8bit)__: 데이터 무환순환 방지를 위해 데이터 최대 한정 시간 설정. 라우터를 거칠때마다 1씩 감소하고 0이 될 경우 데이터 버린다., __upper layer(8bit)__: 상위 계층(전송 계층) 명시를 의미 즉, TCP인지 UDP인지를 명시하고 수신자에서 사용, __header checkum(16bit)__   
   - __32 bit 출발지 IP 주소__   
   - __32 bit 목적지 IP 주소__   
   - __Options(32bit)__   
   - __data(32bit)__: 변수 길이, TCP or UDP Segment   
   - __IP header는 총 20 bytes, TCP header도 총 20 bytes이므로 어플리케이션 Message에 20 + 20 = 40 bytes overhead가 붙여진것이라 볼 수 있다.__   
      - 인터넷에서 40 bytes인 패킷들이 돌아다니는데 대부분 이러한 패킷은 TCP ACK 패킷이다.   

### IP Address(IPv4)   
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
            
   - Classless Inter-Domain Routing(CIDR)   
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

   - __Subnets__   
      - __IP 주소의 같은 Subnet(Prefix)를 가진 장치 인터페이스 집합__   
      - __라우터를 거치지 않고 물리적으로 서로 접근이 가능한 Host들의 집합__    
      - __IP 주소__   
         - Subnet part - 높은 순위 bits    
         - Host part - 낮은 순위 bits   
      - __라우터는 여러 개의 인터페이스를 가지므로 여러 개의 IP를 가진다.__   
         - 각 IP 주소의 prefix가 다 다르다.   
            - 라우터는 여러 개의 Subnet에 속하기 때문에(교집합) 다른 곳으로 가려면 라우터를 거쳐서 가야한다.   

   - __IPv4의 문제점을 해결하기 위해 나온 방법__   
      - __IPv6__   
         - 1996년에 __IPv4의 주소 공간 부족 문제점을 해결__하기 위해서 나왔다.        
         - __128 bit__   
         - __IPv6 datagram 구조__    
            - ver, priority, flow lable   
            - payload length, next header, hop limit   
            - source address   
            - destination address   
            - data   

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


### Dynamic Host Configuration Protocol(DHCP)   
   - __정적 IP VS 동적 IP__   
      - __정적 IP__   
         - 고정된 IP   
         - 장점: 정체성이 있다, 항상 들어올 수 있다.   
         - 단점: 비용이 비싸다, 사용성이 떨어진다, 얼마 쓰지도 않으면서 계속 할당받고 있어야 한다.   
      - __동적 IP__   
         - IP가 시간에 따라 바뀐다.      
         - 장점: 주소를 유연하게 사용 가능 ,비용 절약   
         - 단점: 고정적으로 데이터 받는 서비스 사용 불가   

   - __IP 주소를 어떻게 얻을 수 있나?__   
      - 한 기관은 ISP로부터 주소 블록을 획득하여, 개별 IP 주소를 기관 내부의 호스트와 라우터 인터페이스에 할당한다.   
      - 라우터 인터페이스 주소에 대해서, 시스템 관리자는 라우터 안에 IP 주소를 할당한다.   
      - 호스트에 IP를 할당하는 것은 일반적으로 동적 호스트 구성 프로토콜(Dynamic Host Configuration Protocol, DHCP)을 더 많이 사용한다.   
         - 네트워크 관리자는 해당 호스트가 네트워크에 접속하고자 할 때마다 동일한 ip 주소를 받도록 하거나, 다른 임시 ip 주소를 할당하도록 DHCP를 설정한다.   
         - DHCP는 호스트 IP 주소의 할당뿐만 아니라, 서브넷 마스크, 첫 번째 홉 라우터 주소나 로컬 DNS 서버 주소 같은 추가 정보를 얻게 해 준다.   
         - 네트워크에서 자동으로 호스트와 연결해 주는 DHCP 능력 때문에 "plug and play"라고도 한다   

   - __DHCP 기본 동작 과정__   
      - __DHCP discover__   
         - device가 부팅되면 동일 서브넷에 위치한 DHCP Server를 찾기 위해 DHCP discover 메시지를 이더넷 망에 broadcast한다. (IP dest:FF-FF-FF-FF-FF)   
            - __UDP Packet을 전송하는데 UDP Packet은 IP datagram으로 캡슐화된다.__   
            - __브로드캐스팅__   
               - 목적지 주소를 255.255.255.255로 설정하여 서브넷에 있는 모든 멤버에게 메세지를 전달하는 것이다.   
            - 브로드캐스팅을 통해 전달된 메세지에 다른 호스트는 반응하지 않고 DHCP 서버만 반응을 하는 이유는 Port 번호 때문이다.   
               - DHCP Server만 해당 Port를 열고 listening을 하고 있기 때문이다.   
            - Client가 출발지 IP 주소 0.0.0.0으로 보내면 DHCP가 채워서 IP 할당해서 보냄   
      - __DHCP offer__   
         - DHCP discover 메시지를 수신한 DHCP Server는 DHCP offer 메시지를 broadcast로 모든 Client에게 전송한다. 여기에는 장치(Client)에게 임대해 줄 IP 주소와 자신의 IP 주소가 포함되어 있다.   
            - broadcast 또는 unicast로 모든 Client에게 전송하는 이유는 Client의 IP가 아직 할당되지 않아 어떤 Client인지 모르기 때문이다. 마찬가지로 Port 번호를 통해 Client를 발견한다.   
      - __DHCP request__   
         - Client는 DHCP offer 메시지로부터 받은 네트워크 정보들을 사용하겠다고 요청   
            - Client는 아직까지 받은 IP를 사용하지 않은 상태이기 때문에 DHCP discover과 똑같이 broadcast를 하여 전송한다.   
      - __DHCP ACK__   
         - DHCP Server는 'DHCP ACK' 메시지를 통해 Client의 IP 주소, Subnet mask, Default gateway IP 주소(라우터 IP 주소), DNS 서버 IP 주소, Lease Time(IP 주소 임대 시간)을 전달한다.    
            - DHCP offer와 똑같이 broadcast를 하여 전송한다.   
      - __여러 DHCP가 있을 경우__    
         - 마음에 드는 IP 중 하나를 선택한다.    

      __즉, Client가 DHCP를 통해 네트워크 정보를 얻은 뒤, 접속을 원하는 사이트를 입력시 DNS를 통해 Name Server에서 목적지 호스트 IP를 얻어오고, NAT를 통해 IP, Port 번호를 변환 뒤 라우터를 통해서 목적지 호스트 IP로 데이터를 전송한다. + 방화벽(Firewall)__    

   - __우리는 일반적으로 비용을 지불하여 ISP회사(ex) KT, SK, LGU+)로부터 IP 주소 하나를 할당받는다__   
      - 공유기를 사용하여 다수의 사람이 다수의 다른 IP를 할당할 수 있게 하는데 공유기는 NAT, DHCP, Name Server, 방화벽 기능을 가지고 있다. 공유기는 라우터 IP를 가지며 공유기로 연결된 다수의 사람들은 같은 prefix를 가지는 Client가 된다.  외부에서는 ISP회사 준 하나의 IP로만 인식되지만 공유기를 거쳐 NAT으로 변환된 다수의 내부 IP들이 존재한다.     


## 5. 링크 계층   
   - __네트워크 Terminology__   
      - __Node__   
         - 호스트와 라우터   
      - __Link__   
         - 인접한 노드들 사이의 통신 경로   
         - wired links   
         - wireless links   
         - LANs   
      - __2계층 패킷__   
         - Frame, encapulates datagram   
      - data-link layer는 Datagram을 링크를 통해서 하나의 노드에서 물리적으로 인접한 노드로 전송하는 역할   
   
   - __링크 계층이 구현되어 있는 장소__   
      - 모든 호스트의 Adaptor = (Network Interface Card(NIC))에 구현되어 있다.   
         - Ethernet card, 802.11 card 등 Ethernet chipset    
         - 링크, 물리적 계층을 구현한다.   
      - 호스트의 시스템 버스에 연결되어 있다.   
      - 하드웨어, 소프트웨어, 펌웨어의 조합   
      
   - __Link layer services__   
      - __Framing, link access__   
         - Datagram에 header와 trailer를 더해 Frame으로 캡슐화   
         - 공유되는 매개체라면 채널 접근   
         - "MAC" address는 출발지를 나타내기 위해 Frame header에 사용한다.   
      - __인접한 노드들 사이에서 reliable delivery__   
         - __wireless links__   
            - 에러율 높음   
            - 무선은 에러가 많아서 체크를 많이 해야 한다. (에러 확인하면 데이터를 다시 보내는데, 이러한 작업이 많아 유선보다 느리다.)   
         - __wire link(유선,광케이블)__   
            - 에러율 적음   
            
      - __Flow control__   
         - 인접한 송신 노드와 수신 노드 사이의 페이싱(pacing)   
      - __Error detection__   
         - signal attenuation, noise로 에러 유발됨   
         - 수신자는 에러 탐지   
            - 수신자가 재전송 받기 위해 시그널을 보내거나 Frame을 버림   
            
      - __Error correction__   
         - 수신자가 식별하고 bit error 수정   
      - __Half-duplx and Full duplex__   
         - __Half-duplex__   
            - 단방향   
            - 두 end points가 서로 소통할 때, 둘 다 보내고 받을 수는 있지만, 둘 다 동시에 보낼 수는 없음   
         - __Full-duplex__   
            - 양방향   
            - 보내고 받는게 동시에 가능   
               - 무선은 한번에 하지 못한다.   
               
   - __Adaptors communicationg__   
      - __Sending side__   
         - Datagram을 Frame으로 캡슐화   
         - Error checking bits, rdt, flow control 등 추가   
      - __Receiving side__  
         - Errors, rdt, flow control 등 검사   
         - Datagram을 추출하고 상위 계층에 전달   
         
### Broadcast medium   
   - 여러 명이 broadcast시 충돌하지 않게 해준다   
   - __Medium Access Control(MAC)__   
      - MAC protocol 이라고 불린다.   
      - __3 가지 방식__   
         - __Channel partitioning__   
            - Channel을 여러 조각(time slots, frequenct, code)으로 나눈 뒤 해당 조각에 할당된 만큼만 전송할 수 있다.   
            - __TDMA__   
               - Time Division Multiple Access   
               - 노드에 각 Time slot을 할당한다.   
               - 고정된 길이의 slot만큼 즉, 할당된 시간 동안만 전송을 할 수 있다.   
               - 사용되지 않는 slot이 있을 경우 slot이 낭비되어 비효율적이다.   
                  - 전송할 패킷을 가진 노드가 하나일지라도 타임슬롯 R/N 대역폭으로 한정된다.   
            - __FDMA__   
               - Frequency Division Multiple Access   
               - Channel 범위를 frequency 대역으로 나눈다.   
               - 각 조각은 고정된 frequency 대역을 할당 받는다.    
               - 사용되지 않는 frequency 대역은 낭비되어 비효율적이다.   
         - __Random Access Protocol__    
            - 노드가 보낼 Packet이 있을 때 전송   
               - 완전한 채널 데이터 전송률 R로 전송한다.   
               - 노드 사이에 우선순위가 없다.   
            - 두 개 이상의 노드가 보낼 시 충돌한다.   
            - __CSMA__   
               - Carrier Sense Multiple Access   
               - 전송하기 전에 누가 보내고 있는지 확인한다.   
                  - __Channel이 idle일 경우__   
                     - 전체 Frame을 전송한다.   
                  - __Channel이 바쁠 경우__   
                     - 전송을 미룬다.   
               - __CSMA collisions__   
                  - 여러 노드가 Channel이 idle임을 확인하고 전송할 경우 충돌할 수 있다.   
                  - __Channel propagation delay__   
                     - 신호가 채널의 한쪽 끝에서 다른 쪽 끝으로 전파되는 데 걸리는 시간   
                     - 이러한 전파 지연이 길수록 네트워크의 다른 노드에서 이미 시작된 전송을 캐리어 감지 노드가 감지할 수 없는 경우가 더 증가하여 충돌한다.   
                  - __Collision__    
                     - 전체 Pactet 전송 시간이 낭비된다.   
                     - 거리, 전파 지연은 충돌 확률 결정에 역할을 한다.   
                  - __CSMA는 충돌을 피할 수 없다.__

                  - __CSMA/CD (collision detection)__   
                     - 일반적으로 많이 Ethernet에서 많이 사용한다.   
                        - Wifi는 CSMA/CA를 사용한다.   
                     - Carrier sending, deferral as in CSMA   
                     - 짧은 시간 안에 충돌 탐지   
                     - 충돌하면 전송을 중단시켜 채널 낭비를 줄인다.   
                        - CSMA는 전송 뒤 충돌해도 그대로 전송한다.   
                     - __충돌 탐지__   
                        - 유선 LAN에서는 쉬움   
                           - 시그널 길이 측정, 전송 비교, 시그널 받기   
                        - 무선 LAN에서는 어려움   
                           - 수신 신호 세기가 로컬 전송 세기에 압도되서 신호를 못 받음 (탐지 불가)   

                  - __Ethernet CSMA/CD 알고리즘__    
                     1. NIC는 네트워크 계층으로부터 Datagram 받고, Frame을 생성한다.   
                     2. 만약 NIC가 Channel이 idel인 것을 감지하면, 프레임 전송을 시작한다. 만약 NIC가 Channel이 바쁘다고 느끼면, Channel이 idle이 될 때까지 기다렸다 전송한다.   
                     3. 만약 NIC가 다른 전송 탐지없이 전체 프레임을 전송했다면, NIC는 프레임 전송 완료   
                     4. 만약 NIC가 전송하는 동안 다른 전송을 탐지했다면, 중단하고 jam 신그널을 보낸다.   
                     5. 중단 후, NIC는 binary (exponential) backoff에 진입: (exponential random backoff)   
                        - m번째 충돌 이후, NIC는 {0, 1, 2, ..., 2^M -1}에서 랜덤하게 K를 고른다. 그리고 NIC는 K*512bit 시간을 기다리고 Step b로 돌아간다.   
                        - __즉, 1번 충돌하면 {0,1} 중 하나를 골라 * 512bit 시간만큼 기다린 후 전송한다. 2번 충돌하면 {0,1,2,3} 중 하나를 골라 * 512bit 시간만큼 기다린 후 전송한다.__   
                        - 충돌이 많으면 backoff 간격이 커진다. 즉, 노드가 많을 때 backoff 간격이 커진다.   
                     - __이러한 방식을 사용하는 이유__   
                        - 총 몇 개의 노드가 전송하는 상황인지 모르기 때문에 충돌이 날 시 조금씩 범위를 늘려가면서 모든 노드가 각자 다른 숫자를 골라 충돌이 나지 않도록 하기 위해서이다.   
                        
                     - __링크 계층에서 홉 사이의 Collision detect 됐을 때만 재전송하기 때문에 TCP 재전송과 다른 개념이다.__   
                        - TCP는 중간 과정 상관없이 최종 목적지에서 오류 확인에 따라 재전송하기 때문에 비용이 많이 든다.     

                  - __Taking turns Mac Protocol__   
                     - Channel partitioning Mac Protocol   
                        - high load에서 효율적이고 공정하게 채널 공유한다.      
                        - low load에서 비효율적이다.   
                           - 채널 접근 지연, 오직 한 개의 노드만 활동할 때에도 1/N bandwidth 할당한다.     

                     - Random Access Mac protocol   
                        - low load에서 효율적이다.   
                           - 하나의 노드가 완전히 Channel을 이용할 수 있다.   
                        - high load시 collision overhead 발생   

                     - __Polling__    
                        - __Master / Slave 구조__   
                           - Master 노드는 전송할 차례가 된 Slave 노드를 초대한다.   
                           - Master에서 오류 발생 시 모두 멈춘다.   
                        - __문제__   
                           - Polling overhead   
                           - latency   
                           - single point of failure(Master)   

                     - __Token passing__   
                        - Token을 하나의 노드로부터 다음 노드로 전달하면서 token을 받은 노드만 전송할 수 있다.   
                        - token message
                        - __문제__   
                           - token overhead   
                           - latency   
                           - single point of failure(token)   
                           
### Ethernet 
   - Ethernet은 네트워크에 연결된 각 기기들이 MAC 주소를 가지고 이 주소를 이용해 상호간에 데이터를 주고 받을 수 있도록 만들어졌다. 각 기기를 상호 연결시키는 데에는 허브, 네트워크 스위치 등의 장치를 이용한다.   
   - __유선 LAN 기술__   
   - token LANs과 ATM보다 간단하고 저렴   
   - __Bus__   
      - 90년대 중반에 인기   
      - 서로 충돌할 수 있다.   
   - __Star__   
      - Switch를 중앙에 놓고 관리   
      - 개별적으로 Ethernet protocol을 작동하여 충돌하지 않는다.   
      - mac learning, switch learning   

   - __Ethernet frame structure__   
      - 송신 Adapter는 Ethernet frame에서 IP Datagram을 캡슐화한다.   
      - __preamble__   
         - 상위 7byte: 비트 동기화를 위해서 10101010로 된 비트열을 전달한다.   
         - 하위 1byte: 프레임 시작을 알리는 10101011을 전달한다.   
         - 수신자, 송신자 클락 속도 동기화하는데 사용된다.   
      - __type__   
         - 어떤 상위 계층 프로토콜인지 지정   
      - __CRC__   
         - cyclic redundancy check at receiver   
         - crc붙여서 에러 탐지하거나 복구함   

   - __unreliable, connectionless__   
      - __connectionless__   
         - 송수신자 사이에서 handshaking 없음   
      - __unreliable__   
         - 수신한 NIC은 ack이나 nack을 송신자한테 보내지 않음   
            - CRC 붙여서 에러 탐지되면 버리고, 탐지 안되면 어쩔 수 없는 것   
            - 나머지는 TCP한테 맡김   
         - __Ethernet's MAC protocol__   
            - No slot CSMA/CD with binary backoff   
         - __Collision이 발생했는데 탐지하지 못할 경우를 대비해 Minimum Frame Size인 64bytes를 전송하게 하여 Collision을 감지할 수 있게 한다.__   
            
   - __MAC addresses__   
      - 32 bit IP 주소   
         - 네트워크 계층 주소를 위한 인터페이스    
         - 네트워크 계층으로 Forwarding을 위해 사용된다.   
      - __48 bit MAX(or LAN or physical or Ethernet) address__   
         - 앞 24 bit는 제조회사, 뒤 24 bit는 고유 번호   
         - 같은 네트워크에서 물리적으로 연결된 인터페이스의 IP 주소를 감지하기 위해 사용   
         - __이름 : 호스트 네임, 주소 : IP 주소, 주민번호 : MAC 주소__   
         
   - __LAN addresses__   
      - LAN에 있는 각 Adapter는 고유의 LAN 주소를 가진다.   
      - __MAC 주소는 IEEE에서 할당 -> unique를 보장__   
      - 제조업자는 MAC 주소 공간의 일부분을 구매   
      - MAC flat address -> portability   
         - MAC addr은 처음부터 끝까지 모든 digit으로 무언가를 구분하지 않음. 계층이 없음 -> flat하다   
         - flat한 게 IP와의 차이 (IP 주소는 계층적이니까)   
         
   - __ARP__   
      - Address resolution protocol   
      - __ARP Table__   
         - Cache 테이블   
         - plug-and-play   
            - Wifi 등에 연결하는 순간 자동으로 동기화되며 그 때부터 <IP, MAC 주소> 업데이트   
            - 노드는 net 관리자에게 간섭받지 않고 자신의 ARP 테이블을 만든다.   
         - LAN에 있는 IP 노드(호스트, 라우터)들 각각은 ARP 테이블을 가진다.   
            - 이를 통해 다음 홉의 라우터의 MAC 주소를 찾을 수 있다.   
         - IP 주소 : MAC 주소 매핑해준다. + TTL   
         - TTL (Time To Live): 일정 시간 지나면 매핑된 주소 사라짐   
         
      - __ARP protocol: same LAN__   
         - __A가 B에게 Datagram을 보내고 싶은데 A는 B의 MAC 주소를 모를 경우 (A의 ARP Table에 매핑되는 B의 MAC 주소 존재 X)__   
            - A는 B의 IP 주소가 담긴 ARP query 패킷을 broadcast한다.   
               - 목적지 MAC 주소를 FF-FF-FF-FF-FF-FF로 채워서 보낸다.(broadcasting)   
               - LAN에 있는 모든 노드는 ARP query를 받는다.   
                  - IP 주소가 다르다면 무시   
            - B는 ARP query 패킷을 확인하고, A에게 자신의(B) MAC 주소를 넣어서 응답해준다.      
               - ARP query 패킷에 출발지 주소가 적혀있으므로 Unicast를 하여 A의 MAC 주소로 Frame을 보냄   
            - A는 IP 주소 : MAC 주소 쌍을 TTL이 만료될 때까지 자신의 ARP Table에 저장한다.     
         
      - __Addressing: routing to another LAN__   
         - __Walkthrough__   
            - R을 통해 A로부터 B로 Datagram을 보낸다.   
            - IP (Datagram) 과 MAC 계층 (Frame)
            - A가 B의 IP 주소를 안다고 가정한다.   
            - A가 첫 번째 hop 라우터 R의 IP 주소를 안다고 가정한다.   
            - A가 R의 MAC 주소를 안다고 가정한다.   
            
            - __과정__   
               - A는 IP datagram (source: A의 IP, destination: B의 IP)을 생성한다.   
               - A는 R의 MAC 주소를 목적지로 지정한 링크 계층 Frame을 생성하고 IP datagram을 포함시킨다.   
                  - 목적지 주소를 라우터의 MAC 주소로 지정하면, 나가고 싶다는 신호로 간주된다. 즉, 현재 네트워크 밖으로 datagram을 전송한다는 의미   
               - A는 R로 Frame을 전송한다.   
               - R은 A가 보낸 Frame을 받아 datagram이 제거된 IP 계층으로 올라가 IP를 확인한다.   
                  - 1 계층 : Physical, 2 계층 : Ethernet (MAC), 3 계층: IP   
               - R은 datagram (source: A의 IP, destination: B의 IP)을 Forwarding Table에 forward한다.   
                  - 라우터는 패킷 안에 적힌 목적지 IP 주소를 보고 해당 호스트로 보내기 위해 라우팅 테이블을 보고 다음 라우터로 포워딩을 한다.   
                  - 이 때 사용하는 것이 Longest prefix matching 알고리즘   
               - R은 B의 MAC 주소를 목적지로 지정한 링크 계층 Frame을 생성하고 IP datagram을 포함시킨다.   
               - R은 B로 Frame을 전송한다.   
               - __4번째 부터 과정을 목적지에 도착할 때까지 반복한다.__       
               
               
      - __Switch__   
         - 호스트는 스위치와 직접 연결(direct connection)   
            - 호스트는 스위치의 존재를 모른다.   
         - Switch buffer packets   
         - Enthernet protocol은 각 들어오는 링크에서 사용된다.   
            - No Collisions   
            - Full duplex   
            - 각 링크는 자신 고유의 충돌 범위를 가진다.   
         - A->A', B->'B 같이 여러 곳에 동시에 전송을 할 수 있다.   
            - A->B', B->B' 같이 한 곳에 동시에 보낼 경우에도 Switch를 통해 충돌 없이 전송할 수 있다.   
            
         - __Switch forwarding table__   
            - 각 Switch는 고유 Forwarding table을 가진다.   
            - __Self-learning 과정__   
               - A에서 A'로 보낼 경우 Switch에 전달된다.   
               - Switch는 A에 대한 정보를 Switch forwarding table에 업데이트한다.   
                  - ex) A, Port 번호: 0, TTL: 60초   
               - Switch는 A'가 어디있는지 모르기 때문에 flood를 한다.   
                  - 즉, 다른 모든 라우터 또는 스위치에 A`를 보낸다.   
               - A'는 flood를 통해 받은 정보를 확인하고 Switch에 응답한다.   
               - Switch는 A'에 대한 정보를 Switch forwarding table에 업데이트한다.   
                  - ex) A', Port 번호: 4, TTL: 60초  
               - __Switch forwarding table에 목적지에 대한 정보가 들어있다면 정보를 통해 데이터를 바로 전송하고 없을 경우 self-learning을 통해 table을 업데이트하면서 데이터를 보낸다.  
                  
         - __Interconnecting switches__   
            - Switch끼리 연결 될 수 있다.   
            - 하나의 Swtich가 모든 라우터 연결할 수 없을 때 Switch를 늘려 서로 연결시킨다.   
            - self_learning을 통해서 Switch forwarding table를 갱신한다.   
               
         - __Router vs Switch__      
            - __공통점__   
               - __Store and Forward__   
                  - __Router__  
                     - 네트워크 계층 장치   
                  - __Swtich__   
                     - 링크 계층 장치   
                  
               - __Forwarding table__   
                  - __Router__   
                     - IP 주소와 라우팅 알고리즘을 사용하여 테이블 연산(다음 hop mapping)   
                  - __Switch__   
                     - flooding, learning을 사용하여 Forwarding 테이블 학습(MAC 주소 갱신)   
                  - 3계층 장비라 3 계층의 주소를 다 봐야한다.   
                  
            - __차이점__   
               - __Router__  
                     - IP 주소, NAT, Name server, Firewall 등 여러 기능을 가진다.   
                     - 연결할 노드의 수가 너무 많아 Router + Router 연결하여 확장할 경우   
                        - 각 Router에 대한 Subnet이 생기기 때문에 데이터를 보낼 때 여러 단계를 거쳐서 전송한다.   
               - __Swtich__   
                     - 어떠한 기능도 하지 않고 여러 노드를 연결시켜주기만 한다.   
                     - 연결할 노드의 수가 너무 많아 Router + Switch 연결하여 확장할 경우   
                        - 같은 Subnet에 있기 때문에 Switch가 존재하지 않고 하나의 Router에 연결된 것처럼 데이터를 전송한다.    
               - __그러므로 확장할 경우 Router보다는 Switch를 하는게 덜 복잡하다.__   
                   
               - __learning 차이점__   
                  - __Router__   
                     - 인접한 노드의 정보를 이용하여 다익스트라 or 벨만포드 알고리즘을 사용하여 업데이트   
                     - 최단 경로 찾기   

                  - __Switch__   
                     - 플러그를 꽂자마자(통신이 시작하자마자) 있는걸 가지고 계속 업데이트   
                     - 인접한 노드 찾기   
                     
      - __Data center networks__   
         - 수많은 고객들에게 서비스하기 위해 Switch 계층화를 하여 만든 data center   
         - 여러 Switch를 계층화시켜 load balancing, avoiding processing, networking, data bottlenecks   
         - __Load balancer__   
            - 어플리케이션 계층 라우팅   
            - 부하가 적은 곳을 찾아 연결시킨다.   
               - 외부 고객 요청을 받는다.   
               - data center내에 작업을 직접 연결한다.   
               - (고객에게 data center 내부 데이터를 숨기면서) 외부 고객에 결과를 반환한다.   
                
## 6. 무선과 이동 네트워크   
### Wireless   
   - __Elements of a wireless network__   
      - __Wireless hosts__   
         - 스마트폰, 노트북   
         - Mobile or non-mobile   
         - wireless는 항상 mobility를 의미하지는 않는다.   
      - __Base station(기지국)__   
         - 유선 네트워크와 연결되어 있다.   
         - 계전기   
            - 유선 네트워크와 영역 내의 무선 호스트 사이에 Packets 전달하는 역할을 한다.   
            - ex) cell towers, 802.11 AP(Access Points)   
      - __Wireless link__    
         - mobile을 기지국에 연결하기 위해 사용한다.   
         - backbone link로서 사용될 수도 있다.   
         - 다중 접속 프로토콜이 링크 접근을 조정한다.   
         - 다양한 데이터 전송률, 전송 거리를 가진다.   
      - __Infrastructure mode__   
         - 기지국은 mobile(이동하는 호스트)을 유선 네트워크에 연결시킨다.   
         - handoff   
            - mobile(이동하는 호스트)이 유선 네트워크로 연결을 제공하는 기지국을 변경한다.   
      - __Ad hoc mode__   
         - 기지국이 없다.   
         - 노드는 링크 범위 내의 다른 노드들에게만 전송할 수 있다.   
         - 노드는 스스로 네트워크를 구성할 수 있다.   
            - 노드들간의 라우팅   
      - __Wireless network taxonomy__   
         - __Infrastucture__   
            - __Single hop__   
               - 호스트는 더 큰 인터넷에 연결하는 기지국(Wifi, cellular, WiMAX)에 연결한다.   
            - __Multiple hop__   
               - 호스트는 더 큰 인터넷(Mesh network)로 연결하기 위해서 다수의 무선 노드를 통해 전달해야 한다.    
         - __No Infrastucture__   
            - __Single hop__   
               - 기지국이 없고 더 큰 인터넷(Bluetooth, ad hoc nets)에 대한 연결도 없다.   
            - __Multiple hop__   
               - 기지국이 없고 더 큰 인터넷에 대한 연결도 없다. 여러 개의 무선 노드들의 중계를 거쳐 서로 통신한다. MANET(mobile ad hoc network), VANET(vehicular ad hoc network)   

            
      - __Wireless Link 특징(1)__   
         - __신호 세기의 감소(decreased signal strength)__   
            - 물체를 통과하면서 무선 신호가 약화되고 거리가 멀어질수록 신호가 약해진다.(경로 손실)   
         - __다른 출발지들로부터의 간섭(interference from other sources)__   
            - 동일 표준 무선 주파수(e.g., 2.4 GHz)가 다른 장치(e.g., 휴대폰, 모터)와 공유되고 방해받는다.   
         - __다중경로 전파(multipath propagation)__   
            - 무선 신호가 물체와의 반사, 굴절, 산란 등으로 서로 다른 경로를 거쳐서 전파하여 약간 다른 시간에 도달된다.   
            - 다중경로 전파는 수신 측에서 신호를 제대로 수신하지 못하게 한다.      
            
      - __Wireless Link 특징(2)__   
         - __SNR(Signal-to-noise ratio)__   
            - SNR이 클수록 noise로부터 signal을 추출하기 더 쉽다.   
            - 측정된 수신 신호의 세기와 잡음의 상대적인 비율   
         - __SNR VS BER__   
            - __물리 계층이 주어졌을 경우__   
               - 파워 증가 -> SNR 증가 -> BER(Bit-error-rate) 감소   
               - 동일한 변조 기법 내에서는 SNR 값이 높을 수록 BER은 낮아진다.   
            - __SNR이 주어졌을 경우__   
               - 동일한 SNR 내에서는 높은 전송률의 변조 기법이 높은 BER값을 가진다.   
               - BER 조건을 충족하고 가장 높은 처리량을 주는 물리 계층을 선택한다.   
                  - SNR은 mobility를 변경할 수도 있다.   
                     - 동적으로 물리 계층을 적용시킨다.(modulation, technique, rate)   
                     
      - __Hidden terminal problem__   
         - A-B는 서로 들을 수 있고 C-B는 서로 들을 수 있다.   
         - A는 B로 전송할 때 C는 A 수신 못하는데 이 때 C가 B로 전송을 원할 경우, B가 수신하고 있지 않다고 판단하기 때문에 송신한다.      
            - C의 CS(Carrier Sensing) 실패      
         - B에서 A,C 수신 충돌, A와 C는 충돌 검출 못한다.    
            - A와 C의 CD(Collision Detection) 실패
            - 그러므로 CSMA 사용하지 못한다.   
         - A는 C에 대해 숨겨짐, C도 A에 대해 숨겨진다.   
         
      - __CDMA(Code Division Multiple Access)__   
         - 코드 분할 다중 접속   
         - 무선 LAN 및 셀룰러 등에서 사용되는 공유 매체 접속 프로토콜   
         - 고유한 코드를 각 유저에 할당한다.   
         - __CDMA 동작 원리__   
            ① 각 사용자들에게 유일한 코드를 할당   
            ② 모든 사용자들이 동일 주파수를 공유   
            ③ 각 사용자는 데이터를 인코드하기 위한 자신의 칩핑 시퀀스(chipping sequence, 코드)를 가짐   
            ④ 인코드된 신호 = (original 데이터) * (칩핑 시퀀스)   
            ⑤ 수신자는 인코드된 신호와 칩핑 시퀀스를 내적(inner-product)하여 디코딩   
            ⑥ 코드들이 직교성(orthogonal)을 가지면 여러 사용자가 최소의 간섭으로 동시에 송수신   

### IEEE 802.11 Wireless LAN   
   - WI-FI(Wirelss Fidelity)
      - HI-FI에 무선기술을 접목한 것으로, 고성능 무선통신을 가능하게 하는 무선랜 기술   
   - 802.11b(2.4-5 GHz unlicensed spectrum, 11Mbps), 802.11a(5-6 GHz range, 54Mbps), 802.11g(2.4-5 GHz range, 54Mbps), 802.11n(2.4-5 GHz range, 200Mbps)     
   - 다수 접속을 위해 모두 CSMA/CA을 사용한다.   
   - Infrastructure 방식과 Ad hoc 방식이 있다.      
   - __구성 방식__    
      - 기지국과 통신하는 무선 호스트   
         - 기지국 = AP(Acess point)   
      - BBS(Basic Service Set)   
         - 셀(Cell)이라고도 부른다.   
         - Infrastructure 모드(하나 이상의 무선호스트, 하나의 기지국)   
         - Ad hoc 모드(무선 호스트로만 구성)   
   - __802.11: Channels, association__   
      - __802.11b/g 채널__   
         - 2.4GHz ~ 2.485GHz의 주파수 범위에서 11개의 서로 다른 주파수의 채널로 분할(85MHz 대역폭)   
         - AP 관리자가 채널 선택   
         - 간섭(interference) 가능성 : 채널이 인접 AP에서 선택한 것과 동일할 수 있다.   
            - 간섭이 일어날 경우 CSMA/CA의 RTS, CTS를 통해 경쟁하여 채널을 획득한다.   
      - __호스트는 단 하나의 AP와 결합되어야 한다.__   
         - 채널을 스캐닝(scanning)하고, AP의 이름(SSID)과 MAC 주소가 포함된 비콘 프레임(beacon frame)을 듣는다.   
         - 결합할 AP를 선택   
         - 인증(authentication)을 수행할 수도 있다.   
         - AP 서브넷의 IP 주소를 획득하기 위해 DHCP 수행한다.       
         
   - __802.11 수동적/능동적 스캐닝(passive/active scanning)__   
      - __Passive 스캐닝__   
         - AP들로부터 비콘 프레임(자신의 AP정보가 들어있다.)이 전송된다.   
         - H1에서 선택된 AP로 결합 요청 프레임 전송   
            - AP의 신호 세기를 확인한다.    
         - 선택된 AP에서 H1으로 결합 응답 프레임를 전송   
      - __Active 스캐닝__   
         - H1에서 탐사 요청 프레임(probe request frame)을 broadcast(초당 10번 정도)   
         - AP로부터 탐사 응답 프레임(probe response frame)이 전송   
         - H1에서 선택된 AP로 결합 요청 프레임 전송   
         - 선택된 AP에서 H1으로 결합 응답 프레임 전송    
         
   - __IEEE 802.11 MAC Protocol: CSMA/CA(Coliision Avoidance)__   
      - 일반 CSMA는 전송 전에 채널을 감지(sense)한다.   
         - CSMA/CA는 충돌 감지(collision detection)하지 않는다.   
         - 무선은 약한 수신 신호로 인해 전송시 충돌 감지 어렵다.   
         - 무선은 숨은 터미널 문제, 신호 감소(fading) 등으로 모든 충돌 감지 어렵다.   
      - __유선의 CSMA/CD와 달리 ACK을 보내는데 TCP의 end to end ACK이 아니라 Link 한 Hop 사이의 ACK를 의미한다.__   
      - __802.11 sender__   
         ① 채널이 사용되지 않음을 감지하면 DIFS(Distributed Inter-Freme Space)라는 짧은 시간 동안 기다린 후 전체 프레임 전송   
         ② 채널이 사용중이면 임의의 백오프 시간을 선택하고, 채널이 사용되지 않는 동안만 감소하고 만료되면 프레임 전체를 전송한 후 ACK를 기다림. ACK를 수신하지 못하면 백오프 인터벌을 증가시킨 후 단계 2를 반복   
      - __802.11 receiver__   
         - 프레임을 수신하면 SIFS(Short Inter-Frame Space)라는 짧은 시간 동안 기다린 후 ACK 프레임을 전송   
            -(ACK needed due to hidden terminal problem)   
            
   - __Avoiding collisions__   
      - __유선보다 무선에서 충돌 시 비용이 더 크기 때문에 충돌을 미리 피하기 위한 대책이 필요하다.__   
         - 유선은 충돌을 감지하여 전송을 멈춰 데이터가 날라가는 양이 적지만 무선은 충돌을 감지하지 못해 모든 데이터를 전송하고 ACK을 받을 때까지 기다리기 때문이다.   
      - __아이디어__   
         - 큰 데이터 프레임의 충돌을 피하기 위해서 데이터 임의의 데이터 프레임 접근을 하기 보다는 채널을 예약한다.    
         - 즉, 작은 예약 패킷을 사용하여 데이터 프레임 충돌을 완벽하게 피한다.   
      - 송신자는 CSMA를 사용하여 작은 RTS(Request to Send) 패킷을 broadcast   
         - RTS에 얼마만큼의 데이터를 얼마만큼의 시간동안 전송할건지에 대한 정보가 담겨져있다.   
         - RTS도 충돌 가능성이 있지만 크기가 작아서 효율적이다.    
         - __RTS 충돌이 날 경우 임의의 시간을 backoff 한 후 재전송한다.__    
            - 먼저 보낸 노드의 RTS가 AP에 도착하고 AP가 이에 대한 CTS를 broadcast하여 먼저 보낸 노드에서 CTS를 받고 데이터를 전송을 할 때 다른 쪽의 노드에서는 아직 CTS를 받지 못하여 RTS를 다시 보내게 될 경우도 있다.   
            - 이럴 경우 다시 충돌이 나기 때문에 또 RTS를 처음부터 서로 다시 보내는 과정으로 돌아간다.   
            - 이러한 경우가 임의의 한계 수치만큼 반복됐을 경우 해당 데이터프레임에 대한 전송을 중지하고 다음 데이터프레임에 대한 전송을 한다.   
            - 하지만 포기한 데이터 프레임은 평생 재전송이 안되는 것이 아니라 TCP에 의해 재전송이 된다.   
      - 수신을 받은 AP는 RTS에 응답하여 CTS(Clear to Send) 패킷을 broadcast   
      - 모든 노드는 CTS를 수신한다.   
         - 송신 측은 데이터 프레임을 전송   
            - 데이터 전송이 끝나면 AP는 ACK 전송   
         - 다른 노드는 전송을 미룬다.   
         
   - __802.11 frame: addressing__   
      - Address 1 : 프레임을 수신하는 무선호스트나 AP의 MAC 주소   
         - AP가 주변에 beacon frame을 전송하여 호스트는 AP의 이름과 MAC 주소를 알 수 있다.   
      - Address 2 : 프레임을 전송하는 무선호스트나 AP의 MAC 주소   
         - DHCP request를 통해 자신의 IP 주소, Subnet mask, GWR IP, Local Name server IP를 알 수 있다.   
         - 목적지를 broadcast를 하여 DHCP request를 전송한다.   
         - DHCP 서버로부터 응답이 돌아올 경우 응답 프레임의 출발지 주소는 DHCP 서버의 주소이고 수신지는 broadcast   
         - DHCP 서버가 보낸 broadcast에서 자신에 대한 응답인것을 확인하는 방법은 DHCP transaction number로 구분한다.   
      - Address 3 : AP에 연결된 라우터 인터페이스의 MAC 주소   
         - ARP query를 통해 Address 3를 broadcast하여 라우터의 MAC 주소를 알 수 있다.   
      - Address 4 : 애드 혹 네트워크에서만 사용(중요 X, 거의 사용하지 않는다.)   
      - duration : 예약된 전송 시간 기간 (RTS/CTS)   
      - seg control : 프레임의 시퀀스 넘버 (for RDT)   
      - Type : 프레임 타입(RTS, CTS, ACK, data)   
      - __호스트 -> AP 보낼 시 header에 Address 1,2,3을 넣어 보내지만(Wifi Frame 802.11)__   
      - __AP -> 라우터 보낼 시 header에 출발지 Address(Address 2), 목적지 Address(Address 3)만 넣어 보낸다.(Ethernet Frame 802.3)__   
         - __호스트 -> AP는 왜 3개의 주소, AP -> 라우터는 왜 2개의 주소를 보내는가?__    
            - AP는 링크 계층 디바이스이기 때문에 네트워크 계층이 존재하지 않는다. 그러므로 라우터가 유선상에서 봤을 경우에는 AP는 스위치이고 호스트가 무선상에서 봤을 때는 링크 계층 디바이스로 인식한다. 호스트가 header에 출발지 주소를 자신의 주소, 목적지 주소를 AP의 주소로 하고 AP에 프레임을 전달할 경우 해당 프레임의 IP 패킷은 호스트의 IP, 목적지의 IP(ex) 구글)가 들어있다. 이 경우 AP는 이러한 내용을 이해하고 처리하는 능력이 없어 어디로 전달해야할 지 모르기 때문에 라우터로 해당 프레임을 전송하지 못한다. 그래서 호스트가 출발지 주소를 자신의 주소, 목적지 주소를 라우터의 주소로 하여 프레임을 전달할 경우 AP를 거치지 않아 라우터에 전달되지 않기 때문에 해당 프레임에 대한 ACK를 받을수가 없어 제대로 된 전송이 불가하다. 그러므로 호스트에서 Address 1(AP MAC 주소),2(호스트 MAC 주소),3(AP에 연결된 라우터 MAC 주소)을 모두 넣어주어 프레임을 전달하면 AP는 해당 프레임을 받아 어느 라우터로 전송해야 할지 알기 때문에 해당 라우터로 전송을 할 수 있게 된다.    
      - 여러 개의 호스트에서 보낸 데이터는 하나의 AP를 통해 전송 될 수 있다 = Multiplexing   
      
   - __802.11: mobility within same subnet__   
      - 호스트는 이동하더라도 같은 IP subnet내에 있으면 IP가 유지된다.   
      - __Switch__   
         - Switch는 어떻게 어느 AP가 호스트와 결합되어있는지 알 수 있을까?      
            - __self learning__   
               - 스위치는 호스트로부터의 프레임을 관찰하고 호스트에 도달할 수 있는 스위치 포트를 기억한다.   
      - __휴대폰이 이동하더라도 IP, Port 번호가 변경되지 않는 이유(연결이 끊기지 않는 이유)는 기지국의 coverage가 굉장히 크기 때문이다.__   
               
   - __802.11: advanced capabilities__   
      - __전송율 적응(rate adaptation)__   
         - 모바일 사용자가 이동함에 따라 SNR이 변한다.   
         - 기지국, 모바일 사용자는 동적으로 전송률을 변경한다.(물리 계층 변조 기법 변경)   
            - 노드가 기지국에서 멀어지면 SNR이 작아지고, BER은 커진다.   
            - BER이 너무 커지면 더 낮은 BER을 갖도록 낮은 전송률로 변경한다.      
            
      - __전력 제어(power management)__   
         - 유선보다 무선에서 소모되는 전력이 10배가 더 든다.   
         - 노드는 명시적으로 휴면 상태(sleep state)와 동작 상태(wake state)를 번갈아 가며 변경한다.   
         - __node-to-AP__      
            - 노드는 AP에게 자신이 다음 비콘 프레임이 올때까지 수면 모드로 진입할 것임을 알린다.   
               - 802.11 프레임의 전력 제어 비트를 1로 세팅   
               - AP는 일반적으로 100ms마다 비콘 신호를 전송   
            - AP는 해당 노드에 프레임을 전송하지 않아야 한다는 것을 알게 된다.   
            - 노드는 다음 비콘 프레임이 오기전에 깨어난다.(250us 소요)   
         - __beacon frame__   
            - AP에서 mobile로 보내지기를 기다리는 프레임을 수신해야 하는 mobile의 리스트를 포함한다.    
            - 노드는 AP에서 mobile로 프레임이 전송되어 지면 깨어있고 그렇지 않다면 다음 비콘 프레임이 올 때까지 휴면 상태가 된다.   
               - 목록에 해당하지 않으면 노드는 다시 수면 상태로 진입. 전송 프레임이 없는 노드의 99%는 수면 상태 유지(100ms vs 250us)   
               
   - __802.15: personal area network__   
      - 10m 반경 이내 저전력 통신   
      - ISM(Industrial, Scientific, Medical) 비허가 무선대역   
      - 케이블 대체(마우스, 키보드, 헤드폰)
      - Ad hoc : No Infrastructure   
      - master/slaves   
         - slaves는 master에게 전송할 권한을 요청한다.    
         - master 요청을 승인한다.      
      - 802.15 Bluetooth의 발전   
         - 2.4-2.5 GHz 무선 대역   
         - 802.15.1 : 블루투스(Bluetooth), 1Mbps 전송률   
         - 802.15.2 : Wi-Fi 등과 공존   
         - 802.15.3 : WPAN-HR(High Rate), 55Mbps 전송률   
         - 802.15.4 : WPAN-LR(Low Rate), Zigbee, 6LoWPAN, 250 Kbps 전송   
         
### Cellular Internet access   
   - __Components of cellular network architecture__   
      - __cell__   
         - 지리적 영역을 포함한다.   
         - base station(BS)는 802.11 AP와 유사하다.   
         - mobile users는 BS를 통해 네트워크와 연결된다.   
         - air-interface는 mobile users와 BS 간의 물리 및 링크 계층 프로토콜   
      - __MSC(Mobile Switching Center)__   
         - cell들을 유선 tel. net과 같은 광역 네트워크에 연결한다.   
         - call 설정을 관리한다.      
         - mobility를 다룬다.    
         
   - __Cellular networks: the first hop__   
      - mobile-to-BS radio spectrum를 공유하는 두 가지 기술   
         - combined FDMA/TDMA   
            - 범위를 주파수 채널로 나누고 각 채널을 time slot으로 나눈다.    
         - CDMA(Code Division Multiple Access)   
            - 모든 것을 섞어서 보낸다.   
            - 각 사용자에게 다른 코드를 주어 자신에게만 해당하는 데이터만 증폭시키게 해준다.   
            - 3G부터 사용한다.   
            
### Mobility   
   - __What is mobility?__   
      - 네트워크 관점에서 본 이동성(mobility)의 범위   
      - 이동성 없음    
         - 이동 사용자가 같은 AP를 통해 접속   
      - 중간   
         - 이동 사용자가 DHCP를 사용하여 네트워크와 연결하고 끊음   
      - 이동성이 큼   
         - 이동 사용자가 연결을 유지한 채 다수의 AP를 통과 like 휴대폰    
         
   - __Mobility: vocabulary__   
      - __home network__       
         - mobile의 영구적인 "home" 네트워크   
         - e.g., 128.119.40/24    
      - __home agent__   
         - mobile가 멀리 떨어져 있을 때 이동성 관리 기능을 수행하는 개체   
      - __permanent address__   
         - mobile에 도달하기 위해 항상 사용될 수 있는 홈 네트워크 주소   
         - e.g., 128.119.40.186    
      - __visited network__   
         - mobile이 현재 위치해 있는 네트워크   
         - e.g., 79.129.13/24   
      - __care-of-address__   
         - visited network의 주소   
         - e.g., 79.129.2   
      - __foreign agent__   
         - mobile의 이동성 관리 기능을 수행하는 visited network 안에 있는 개체   
         - home agent와 통신을 하여 mobile이 현재 어디 있는지 알려준다.   
      - __correspondent__   
         - mobile과 통신하기를 원하는 상대방   
         
   - __Mobility:approaches__   
      - __let routing handle it__   
         - router가 routing table 교환을 통해 mobile의 영구 주소를 알린다.   
            - routing tables은 각 mobile이 위치한 곳을 나타낸다.   
            - end-systems 변경 없다.   
         - __수백만개 이상의 이동노드들로 확장 불가능하다__   
         
      - __let end-systems handle it__   
         - __Indirect routing__   
            - correspondent는 home agent를 통하여 mobile과 통신한다.   
         - __Direct routing__   
            - correspondent는 mobile의 foreign address를 얻어 mobile에 직접 통신한다.   
            
   - __Mobility: registration__   
      - mobile가 방문한 네트워크 진입 시 foreign agent와 접촉한다.   
      - foreign agent는 home agent와 접촉하여 mobile이 자신의 네트워크 있다는 것을 알려준다.   

   - __Mobility via indirect routing__   
      - __과정__   
         - 상대방이 mobile의 영구 주소를 사용하여 패킷들의 목적지 주소를 지정하여 보낸다.   
         - home agent는 패킷을 가로채어 foreign agent로 전달한다.   
         - foreign agent는 받은 패킷을 mobile로 전달한다.   
         - mobile은 상대방에게 직접 응답한다.   
      - __mobile는 2개의 주소를 이용__   
         - __permanent address__   
            - 상대방에 의해 사용된다. (mobile의 위치는 상대방에게 투명하다.)   
         - __care-of-address__   
            - home agent가 mobile로 데이터그램을 전송할 때 사용된다.   
      - __foreign agent의 기능은 mobile에 의해 수행될 수 있다.__   
      - __triangle routing__   
         - 상대방 - 홈 - 네트워크 - 모바일   
         - 상대방과 mobile이 같은 네트워크 영역 내에 있으면 비효율적   
      - __장점__   
         - permanent address만 알면 전송이 가능하다.   
      - __단점__   
         - 여러 번 거쳐 전송이 된다.   
      
      - __Indirect routing: moving between networks __   
         - mobile이 다른 네트워크로 이동한다고 가정   
            - 새로운 foreign agent 등록   
            - 새로운 foreign agent를 home agent에 등록   
            - home agent는 mobile에 대한 care-of-address 업데이트   
            - 새로운 care-of-address를 사용하여 mobile로 패킷 계속 전달   
         - __이동성과 foreign networks 변경이 투명함(transparent)과 동시에 연결이 계속 유지될 수 있다.__   

   - __Mobility via direct routing__   
      - __과정__   
         - 상대방이 home agent에 mobile의 foreign address 요청하여 주소를 얻는다.   
         - 상대방은 foreign agent에 패킷을 보낸다.   
         - foreign agent는 패킷을 mobile에 전달한다.   
         - mobile은 직접 상대방에게 응답한다.   
      - __triangle routing 문제를 극복한다.__   
      - __이동 및 네트워크 변경이 상대방에게 불투명(non-transparent)__   
         - 상대방은 반드시 home agent로부터 care-of-address를 받아야한다.   
      - __장점__   
         - 전송이 빠르다.   
      - __단점__   
         - 상대방이 알아야 할 정보와 해야할 일들이 많아진다.   
         
      - __Accommodating mobility with direct routing__   
         - __anchor foreign agent__   
            - 첫번째로 방문한 네트워크의 foreign agent   
            - 데이터는 항상 anchor foregin agent에 먼저 routing한다.   
            - mobile이 다른 네트워크로 이동한다고 가정   
               - 오래된 foreign agent로 전송된 데이터를 새로운 foreign agent로 할당해야 한다.   
               - 다시 또 이동 시 연속적으로 적용한다.(chaining)   
               
## 7. 멀티미디어 네트워크   
### Multimedia   
   - __Multimedia: audio__   
      - 상수 속도로 샘플된 아날로그 오디오 신호   
         - 전화기 : 8,000 samples/sec   
         - CD music : 44,100 samples/sec   
      - 각 sample은 양자화된다.   
         - 2^8 = 256 양자화된 값   
         - 각 양자화된 값은 bits로 표현된다.   
         - 8bits for 256개의 값   
      - 수신자가 bits를 아날로그 신호로 다시 변환할 때 quality reduction 발생한다.  
      - __아날로그 신호를 최대한 잘 표현하기 위해서는 더 많은 비트를 사용하면 된다.__   
         - sample rate가 크면 클수록 더 잘 표현할 수 있다.   
         - CD: 1.411 Mbps   
         - MP3: 96, 128, 160 Kbps   
         - Internet telephony: 5.3 Kbps and up   
   
   - __Multimedia: video__   
      - 상수 속도로 보여지는 이미지의 연속   
         - ex) 24 images/sec   
      - __digital image__   
         - pixels의 배열   
            - 각 pixel은 bits로 표현된다.   
      - __coding__    
         - coding rate가 크면 클수록 더 잘 표현할 수 있다.   
         - image를 표현하기 위해 사용된 bits를 줄이기 위해 이미지 사이와 안에서 중복을 사용한다.   
            - __spatial coding(이미지 내)__   
               - 같은 색의 값 N 개를 전송하지 않고, 컬러의 값(Color), 반복된 값의 수(N) 2개의 값만 전송한다.   
            - __temporal coding(하나의 이미지로부터 다음 이미지)__   
               - frame i, frame i+1이 있을 경우   
                  - i+1에 frame 전부 보내지 않고 i와 다른 부분만 전송한다.   
      - __CBR(Constant Bit Rate)__   
         - 고정된 video encoding rate   
      - __VBR(Variable Bit Rate)__   
         - spatial, temporal coding 변화의 양과 같은 video encoding rate가 변화한다.   
      - MPEG 1(CD-ROM) : 1.5 Mbps   
      - MPEG 2(DVD) : 3-6 Mbps   
      - MPEG 4(Internet) : 1 Mbps   
               
   - __Multimedia networking: 3 application types__   
      - __streaming, stored__   
         - __streaming__   
            - 전체 파일을 다운로드하기 전에 재생을 시작할 수 있다.  
         - __stored(at server)__   
            - audio/video가 render 되는 것보다 빠르게 전송할 수 있다.   
            - ex) Netflix, Youtube, Hulu   
      - __conversational__   
         - IP를 통한 voice/video   
         - 사람간 대화의 지연 허용을 제한한다.      
         - ex) Skype   
      - __streaming live__   
         - audio, video   
         - ex) live sporting event   
         
   - __Streaming stored video__   
      - video가 녹화되어 서버에 저장된다.   
         - ex) 30 frames/sec   
      - video가 서버로부터 고객에게 전송된다.   
         - 이 과정에서 서버가 video의 뒷 부분을 전송하는 동안 고객은 video의 앞 부분을 재생할 수 있다.(streaming)   
         - Jitter가 발생 할 수 있다. 즉, network delay가 발생할 수 있다.   
      - 고객은 video를 받고 실행한다.   
         - ex) 30 frames/sec
         
   - __Streaming stored video: revised__    
      - __고객은 첫번째 프레임부터 받는 순간부터 바로 재생을 하려고 할 경우 frame이 제대로 도착하지 않아 재생하지 못하는 상황이 발생할 수 있다.__   
      - __그러므로 고객은 항상 일정시간 기다린 후에 재생을 하여 이러한 상황이 일어나는 것을 방지할 수 있다. = buffering__    
      
   - __Client-side buffering, playout__   
      - __playout buffering__   
         - x: fill rate   
            - x의 값은 변화한다.   
         - r: playout rate   
            - r의 값은 상수 값이다. 즉, 변화하지 않는다.   
         - x < r일 경우 buffer는 결국 텅비게 되어 buffer가 다시 채워질때까지 video의 재생은 멈추게 된다.   
         - x > r일 경우 buffer는 텅비지 않는다. 초기 playout delay는 x의 변동성을 받아들이기 위해 충분히 크게 제공된다.   
            - __initial playout delay tradeoff__   
               - buffer starvation은 더 큰 delay를 가질수록 적게 일어나지만 더 큰 delay를 가질수록 사용자는 더 오래 기다려야 한다.   
          
   - __Streaming multimedia: HTTP__   
      - HTTP GET을 통해 멀티미디어 파일을 얻는다.   
      - TCP에서 가능한 최대 전송률로 전송한다.   
      - fill rate는 TCP 혼잡 제어, 재전송(순차적 전송)때문에 변동한다.   
      - playout delay가 클수록 TCP 전송률은 매끄럽다.   
      - HTTP/TCP는 방화벽을 통해 더 쉽게 전달한다.   
      
   - __Streaming multimedia: DASH(Dynamic Adaptive Streaming over HTTP)__   
      - __Server__   
         - video 파일을 여러 chunk(일반적으로 256kb)으로 나눈다.   
         - 각 chunk은 다른 비율로 저장되고 인코딩된다.   
         - __manifest file__   
            - 다른 chunk들을 위한 URL을 제공한다.   
         - 제일 작은 사이즈부터 전송을 시작하고 제대로 전송이 된다면 다음 chunk는 더 큰 사이즈로 전송을 하는 과정을 반복한다.   
         - 제대로 전송이 되지 않았다면 다음 chunk는 사이즈를 줄여 전송을 한다.   
         - 이렇게 함으로써 네트워크 상황이 좋지 않더라도 client는 영상이 끊기지 않고 낮은 화질에서라도 계속 시청이 가능할 수 있게 된다.   
            
      - __Client__   
         - 주기적으로 Server-to-Client 대역을 측정한다.   
         - manifest를 참고하여 한번에 하나의 chunk만 요청한다.   
            - 현재 주어진 대역에서 지속가능한 최대 coding rate를 선택한다.   
            - 이용가능한 대역에 따라 다른 지점에서 다른 coding rate를 선택할 수도 있다.   
            
      - __Youtube, Netflix와 같은 서비스가 사용한다.__    
      
      - __여러 사용자가 하나의 서버에 같은 데이터를 요청할 경우 해결 방법__   
         - __Multicast__   
            - 1:1일 경우 Unicast를 사용하지만 1:n일 경우 라우터에 데이터를 복사한 뒤 그 데이터를 n에게 전송하는 Multicast를 사용한다.   
            - 라우터내에 구현이 힘들어 보통 사용이 되지 않는다.   
            
         - __CDN(Content Distribution Network)__   
            - CDN Storage를 여러 곳에 분산 시킨다.   
            - 사용자가 서버에 접속하여 데이터를 요청할 때 서버는 manifest file을 넘겨주고 사용자는 manifest file을 통해 최소 Hop 수를 가진 CDN Storage로부터 데이터를 제공받는다.   
            - Infrastructure 기반이기 때문에 관련 회사와 계약을 맺어 CDN을 만든다.   
            - __같은 URL을 사용하는데 어떻게 자신 근처의 CDN으로부터 데이터를 제공받을 수 있는가?__   
               - Youtube와 CDN 회사 Global CDN이 있다고 가정   
                  - youtube.com/video1 을 요청할 경우 Youtube 서버는 globalcdn.com/video1을 고객에게 응답해준다.   
                  - globalcdn.com/video1 에서 globalcdn.com에 접근하기 위해서는 IP를 알아야 하는데 이 때 DNS(UDP 사용 = IP 패킷이 들어있다.)을 통해 IP를 알아올 수 있다.   
                     - globalcdn의 authoritative DNS가 요청한 사람의 IP 주소를 통해 가까운 CDN 서버의 IP를 알려준다.   
                     - ex) 한국에서 접근하면 한국 전용 IP를 알려주고, 캐나다에서 접근하면 캐나다 전용 IP를 알려준다.   
                     
               - __하지만 고객의 IP는 NAT 등으로 인해 실제 IP가 아닐수도 있다. 어떻게 위치를 판별할 수 있는가?__    
                  - 고객은 항상 SK, LG U+, KT 같은 네트워크를 거쳐 전달되기 때문에 실질적으로는 SK, LG U+, KT가 DNS query를 보내어 위치를 알 수 있게 된다.   
               - __최소 Hop 수를 가진 CDN에 접근하는게 제일 좋기 때문에 보통 SK, LG U+, KT 같은 네트워크 내의 CDN을 위치시키거나 근처에 위치시킨다.   
                  
## 8. 네트워크 보안   
### Network security   
   - __What is network security?__   
      - __기밀성(Confidentiality)__   
         - 오직 송신자와 수신자만 내용을 알아야한다.   
         - 송신자는 메세지를 암호화한다.   
         - 수신자는 메세지를 해독한다.   
         -__암호화가 되어있지 않을 경우 발생할 수 있는 일__   
            - __도청(eavesdrop)__   
               - 메세지를 엿볼 수 있다.   
            - __사칭(impersonation)__   
               - 패킷의 출발지 주소를 위조할 수 있다.   
            - __탈취(hijacking)__   
               - 수신자나 송신자를 제거하고 자신을 넣음으로써 진행중인 연결을 탈취할 수 있다.   
            - __서비스 거부(denial of service)__   
               - 서비스가 사용되는 것을 막을 수 있다.   
      - __인증(Authentication)__   
         - 송신자와 수신자 모두 서로의 신분을 확인하기를 원한다.   
      - __메세지 무결성(Message integrity)__   
         - 송신자가 보낸 메세지가 변형되지 않고 그대로 수신자가 받아야한다.   
      - __접근과 이용가능성(Access and availability)__   
         - 서비스를 제공하는 사람은 24시간 내내 접근이 가능해야하고 사용자에게 서비스를 제공할 수 있어야 한다.   
            
   - __접근 불가능 사이트__   
      - 정부가 차단하는 사이트의 목록이 있다.   
         - 해외로 나가는 위치의 라우터에 app 계층의 기능(filtering)을 넣고 사이트 목록을 넘겨주어 검사하고 유저에게 Warning.html 파일을 보낸다.   
            - 정부가 패킷을 그냥 Drop시킬경우 사용자의 웹브라우저에는 connection error(Not 404)가 뜨고 사용자는 네트워크의 문제로 인식할 수 있다.   
            - 이럴경우 정부가 사용자에게 전달하고자 하는 의미를 전달할 수 없기 때문에 Warning.html 파일을 전달한다.   
            
      - __해결 방법__   
         - __Proxy__   
            - 해외 Proxy 서버를 통해 우회 접근을 한다.   
            - 기술적으로는 정부가 해외 Proxy 서버에 대한 막을 수 있지만 막지 않고 있다.    
         - __TOR__   
            - 분산형 네트워크 기반의 익명 인터넷 통신 시스템   
            - 익명 인터넷 통신을 위해 오가는 데이터 소스를 추적하기 어렵게 발신자에서 수신자로 가는 도중 랜덤 서버를 통과하도록 해 트래픽을 3회에 걸쳐 전송하는 방식이다.   
            - 목적지와 데이터 등 모두 숨길 수 있다.   
        
      - __어떻게 사용자에게 경고장을 보낼 수 있을까?__   
         - HTTP Request의 Host 확인을 통해 보낼 수 있다.   
         ① 사용자가 서버에 접속을 원할 경우 해당 서버의 IP를 알아오기 위해 DNS를 사용한다.   
         ② 사용자가 서버에 TCP 연결을 요청   
            - TCP Segment에 자신의 IP, 목적지 IP를 담아 전송   
         ③ 서버와의 TCP 연결이 된 후 사용자는 HTTP Request를 요청한다.   
         ④ 해외로 나가는 위치의 라우터(정부)는 HTTP Request의 Host를 확인한다.   
         ⑤ Host가 정부에서 차단하는 사이트일 경우 HTTP Response에 Warning.html을 담아 사용자에게 전송한다.   
            - 출발지 주소는 정부가 아닌 Host, 목적지 주소는 사용자로 설정하여 전송한다.   
         ⑥ 사용자는 웹 브라우저에 Warning.html을 띄운다.   
         ⑦ TCP로 연결된 서버는 Time out되어 연결을 종료한다.   
      
   
### Cryptography   
   - __간단한 암호화 구조(Simple encryption scheme)__   
      - __substitution cipher__   
         - 하나의 것을 다른 것으로 대체한다.   
         - __monoalphabetic cipher__   
            - 하나의 글자를 다른 글자로 대체한다.   
            - plaintext: abcdefghijklmnopqrstuvwxyz   
            - ciphertext: mnbvcxzasdfghjklpoiuytrewq   
            - __Key__   
               - 26개의 글자가 26개의 글자로 mapping된다.   

      - __Polyalphabetic encryption__   
         - n개의 monoalphabetic ciphers   
            - M1,M2,…,Mn   
         - __순환 패턴(Cycling pattern)__   
            - ex) n=4, M1,M3,M4,M3,M2; M1,M3,M4,M3,M2;   
         - 새로운 각 plaintext 기호에 대해서 cyclic pattern내의 다음의 monoalphabetic pattern을 사용한다.   
            - ex) dog: d from M1, o from M3, g from M4   
         - __Key__   
            - n개의 ciphers, cyclic pattern   

      - __Breaking an encryption scheme__   
         - __암호문만 공격(Cipher-text only attack)__   
            - 해커가 분석할 수 있는 ciphertext를 가지고 있는 경우   
            - __2가지 방법__   
               - __모든 키 확인(Search through all keys)__   
                  - 영문 모를 말로부터 plaintext를 식별할 수 있어야만 한다.   
               - __통계적 분석(Statistical analysis)__   

         - __알려진 Plaintext 공격(Known-plaintext attack)__   
            - 해커가 일부 암호문과 부합하는 plaintext을 가지고 있는 경우   
            - ex) monoalphabetic cipher에서 해커는 a,l,i,c,e,b,o에 대한 암호를 알아낼 수 있다.   
         - __선택된 Plaintext 공격(Chosen-plaintext attack)__   
            - 해커가 선택된 plaintext에 대한 암호를 얻을 수 있는 경우   

   - __암호화 종류__   
      - __대칭키 암호화(Symmetric key cryptography)__   
         - __수신자와 송신자가 암호키를 공유한다.__   
         - 첫 만남에 같은 키를 공유하기 위한 방법이 까다롭다. 특히 전에 서로 만나지 않았을 때   

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
  
   - __Authentication__   
      - __ap4.0__   
         - __Playback attack__   
            - 네트워크를 통해 유효한 데이터 전송을 가로 챈 후 반복하는 사이버 공격   
            - 원래 데이터 (일반적으로 권한이 부여 된 사용자에게서 가로챔)의 유효성으로 인해 네트워크의 보안 프로토콜은 해커의 공격을 유효한 데이터인 전송인 것처럼 여기게 된다.   
            - 원본 메시지를 가로채어 그대로 재전송하므로 해커는 데이터를 해독하지 않아도 되는 리플레이 공격을 사용합니다.   
         - playback attack(유효한 데이터 전송을 가로 챈 후 복사하여 재전송하는 것)을 피하기 위해 사용된다.   
         - nonce R(오직 한번만 사용되는 숫자)와 공개키 암호화를 사용한다.   
      - __과정__   
         - A가 B에게 자신이 A라고 전송   
         - B는 A에게 R 전송   
         - A는 공유된 대칭키를 통해 R을 암호화하여 B에게 전송   
            - A만 같은 대칭키를 가지고 nonce를 암호화할 수 있기 때문에 B는 A인것을 확인할 수 있다.   
      - __A-B 사이에 대칭키를 어떻게 만드는지에 대한 고려가 필요하다.__   

      - __ap5.0__   
         - ap4.0처럼 대칭키를 사용하지 않고 공개키를 사용하는 방식   
         - nonce와 공개키를 사용한다.   
      - __과정__   
         - A가 B에게 자신이 A라고 전송   
         - B는 A에게 R 전송   
         - A는 K-(R)을 통해 암호화 하여 B에게 전송   
            - B는 K+(K-(R)) = R 연산을 통해 오직 A만 K+(K-(R)) = R 같이 R을 암호화했던 Private key를 가질 수 있다는 것을 알게 된다.   
         - B는 A에게 공개키를 보내라고 메시지 전송   
         - A는 K+를 B에게 전송   
            - B는 A인것을 확인할 수 있다.   
            
            
   - __Message Integrity__   
      - 수신된 메세지가 믿을만한 것인지 확인하게 할 수 있게 해준다.   
         - 메세지의 내용이 변경되지 않았는지   
         - 메세지의 출발지가 내가 생각하는 곳과 같은지   
         - 메세지가 다시 전송되지 않았는지   
         - 메세지의 결과가 유지되었는지    

      - __Message Digests__   
         - 각기 다른 메세지의 길이를 Input, 고정된 문자 길이를 output으로 하는 H() 함수를 사용한다.    
         - H()는 many-to-1 함수   
         - H()는 hash 함수이다.   
         - __특징__   
            - 계산하기 쉽다.   
            - __비가역성(Irreversibility)__   
               - H(m)을 통해 m으로 되돌릴 수 없다.   
            - __충돌 저항(Collision resistance)__   
               - m 과 m'에 대한 H(m) = H(m')같은 결과를 만들어내기 어렵다.   
            - __random output처럼 보인다.__   
            
         - __Internet checksum은 hash fuction의 특성(many-to-one, 16-bit 고정된 길이의 sum을 산출)을 가지고 있지만 특정 hash 값을 가진 메세지가 있을 경우 같은 hash 값을 가진 다른 메세지를 찾을 가능성이 높다.__   
         
         - __Hash Function Algorithms__   
            - __MD5 hash function(RFC 1321)__   
               - 128-bit 메세지를 4단계 프로세스에 거쳐 요약한다.   
            - __SHA-1__   
               - US 표준[NIST, FIPS PUB 180-1]   
               - 160-bit 메세지를 요약한다.   
          
         - __Message Authentication Code (MAC)__   
            - 송신자를 인증한다.   
            - 메세지 무결성을 증명한다.   
            - 암호화가 필요없다.   
            - "keyed hash"라고도 불린다.   
            - __HMAC__    
               - 많이 사용되는 MAC 표준   
               - 포착하기 어려운 보안 결함들을 다룬다.   
               - __과정__   
                  ① 메세지의 앞부분을 암호와 연관시킨다.   
                  ② 1을 Hash한다.   
                  ③ 2의 앞부분을 암호와 연관시킨다.   
                  ④ 3을 Hash한다.   
               - ex) intra-AS routing protocol인 OSPF, End-point   

         - __디지털 서명(Digital Signatures)__   
            - 송신자는 디지털 방식으로 서류에 서명한다.   
            - MAC의 목표와 같지만 공개키 암호화 사용   
            - __간단한 서명 방식(= Digital signing)__   
               - 송신자는 메세지를 자신의 private 키로 암호화하여 전송한다.   
               - __but 메세지 전체를 암호화하기에 시간이 오래 걸리기 때문에 메세지의 hash 값을 private 키로 암호화하여 전송한다.__    
               
            - __공개키는 어떻게 인증을 받는가?__   
               - 공인 기관으로부터 발행된 인증서를 사용한다.   
               - __Certification authority(CA)__   
                  - 특정 개체(라우터, 사람)와, 개체의 Public key를 연결한다.   
                  - 개체는 CA를 가지고 자신의 Public key를 등록해야 한다.   
                     - 개체는 CA에 자신 신원의 증거를 제공해야 한다.   
                     - CA는 개체와 개체의 Public key를 연결하는 인증서를 생성한다.   
                     - 인증서는 CA에 의해 디지털 방식으로 서명된 개체의 Public key를 포함한다.   
                        - 인증서는 CA의 Private key로 암호화 되어 있다.   
                        
                  - __A가 B의 공개키를 알고 싶은 경우__   
                     - B의 인증서를 얻는다.(B나 다른 곳으로부터)   
                     - CA의 Public key를 B의 인증서에 적용하여 B의 Public key를 얻는다.   
                     
               - __인증기관의 키는 어떻게 인증을 받는가?__   
                  - root로 생각하고 인증기관은 그냥 인증되어있다고 가정하고 이용한다.   

### Securing TCP connections: SSL   
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

   - __Toy SSL: a simple secure channel__   
      - __Handshake__   
         - A와 B는 공유된 비밀을 교환하고 서로를 인증하기 위해 자신의 인증서와 Private key를 사용한다.   
      - __Key Derivation__   
         - A와 B는 key의 집합을 얻기 위해서 공유된 비밀을 사용한다.   
      - __Data Transfer__   
         - 전송되어야 할 데이터는 일련의 레코드에 분산된다.   
      - __Connection Closure__   
         - 안전하게 연결을 종료하기 위한 특별한 메세지가 있다.   
         
   - __Toy: a simple handshake__   
      - __A-B사이에 TCP 연결이 된 후의 상황__   
         - A는 B에게 메세지를 보낸다.   
         - B는 A에게 자신의 Public key 인증서를 보낸다.   
         - A는 자신의 Secret을 만들어 B의 Public key로 암호화하여 B에 전송한다.   
            - Kb+(MS) = EMS   
            - MS(Master Secret)   
            - EMS(Encrypted Master Secret)   
         
   - __Toy: Key derivation__   
      - __한번 이상의 암호화 수행에 같은 키를 계속 사용하는 것은 좋지 않다.__   
         - 키가 유출되었을 때 피해를 최소화하는 것이 좋다.   
         - MAC(Message Authentication Code)과 암호화를 위해서 다른 Key를 사용해야 한다.   
      - __4개의 키__   
         - __Kc__   
            - Client -> Server로 보낸 데이터에 대한 암호화 키   
         - __Mc__   
            - Client -> Server로 보낸 데이터에 대한 MAC 키   
         - __Ks__   
            - Server -> Client로 보낸 데이터에 대한 암호화 키   
         - __Ms__   
            - Server -> Client로 보낸 데이터에 대한 MAC 키   
      - __KDF(Key Derivation Function)__   
         - master secret과 추가적인 random data을 가지고 4개의 키를 생성한다.   
         
   - __Toy: data records__   
      - __data를 TCP에 작성하는 것처럼 일정한 stream로 암호화 하지 않는 이유는?__   
         - MAC을 맨 끝에 넣을 경우, 모든 데이터가 처리될 때까지 메세지 무결성을 확인할 수 없다.   
         - 예를 들어, 긴급한 메세지일 경우, 출력하기 전에 모든 바이트에 대한 무결성 확인을 어떻게 할 수 있을까?   
      - __대신에, stream을 일련의 record로 나눌 경우__   
         - 각 record는 MAC을 가지고 있어야 한다.   
         - 수신자는 record가 도착할 때 마다 각 record에 따라 행동할 수도 있다.   
      - __문제: 수신자는 data로부터 MAC을 구별해야 할 필요가 있다.__   
         - 가변 길이 record를 사용해야한다.   
         
      - __데이터 또는 MAC에 대해서만 hash해서 전송할 경우__   
         - 해커가 data를 조작하고 공개된 hash함수를 사용하여 새로운 MAC을 생성하여 전송할 수 있다.   
      - __그러므로 데이터, MAC을 같이 hash하면 어느 누구도 접근할 수 없다.__   
         - IP Packet(TCP Segment(SSL Record[LEN,DATA,MAC]))   
         - 출발지, 목적지 등 DATA, MAC 외의 정보는 알 수 있으나 DATA, MAC은 접근을 할 수 없어 내용을 알 수가 없다.   
         - 내용도 숨기고 목적지도 숨기길 원한다면 TOR 사용   
         - __그러나 해커는 DATA, MAC을 다른 SSL의 DATA, MAC과 바꿔 record의 순서를 바꾸거나 record를 replay 할 수가 있다.__   
         
   - __Toy: Sequence Numbers__   
      - 해커는 DATA, MAC을 다른 SSL의 DATA, MAC과 바꿔 record의 순서를 바꾸거나 record를 replay 할 수가 있다.   
      - __그러므로 MAC에 sequence number를 넣는다.__   
         - TCP의 sequence number가 아니라 SSL의 순서일 뿐이다.   
         - MAC에 DATA, MAC, Sequenct number를 hash한 값을 넣는다.   
            - MAC = MAC(Mx, sequence||data)   
         - header에 담겨있지 않고 count 하는데만 사용된다.(no sequence number field)  
         - __해커는 여전히 모든 record에 대해 replay 할 수 있다.__   
            - random nonce를 사용하여 해결할 수 있다.   
         - __그러나 데이터가 다 전송되지 않은 상태에서 해커가 TCP FIN을 보내 연결을 종료할 수 있다.__   
         
   - __Toy: Control information__   
      - 데이터가 다 전송되지 않은 상태에서 해커가 TCP FIN을 보내 연결을 종료할 수 있다.   
      - __header에 record type 1 bit를 추가한다.__   
         - type 0 : data   
         - type 1 : 종료   
         - MAC에 DATA, MAC, Sequenct number,type를 hash한 값을 넣는다. 
            - MAC = MAC(Mx, sequence||type||data)   
            
### IPsec   
   - __IPSec service__   
      - __데이터 무결성(Data integrity)__   
      - __최초 인증(Origin authentication)__   
      - __replay attack 방지(Replay attack prevention)__     
      - __기밀성(Confidentiality)__   
      - __다른 서비스 모델을 제공하는 두 가지 프로토콜__   
         - __AH(Authentication Header)__   
            - 데이터 무결성, source 인증을 제공하지만 기밀성은 제공하지 않는다.   
         - __ESP(Encapsulation Security Protocol)__   
            - 데이터 무결성, source 인증, 기밀성을 제공한다.   
            - AH보다 더 사용된다.   
            
   - __IPsec Transport Mode__   
      - IPSec datagram은 end-system에 의해 발행되고 받아진다.   
      - 상위 레벨 프로토콜을 보호한다.   
      
   - __IPsec – tunneling mode__   
      - End 라우터는 IPSec을 알고 있다.   
         - host - Router(IPSec) - Router(IPSec) - host   
      - 호스트는 IPSec을 알 필요가 없다.   
      - Tunneling mode   
         - host - Router(IPSec) - Router - host(IPSec)   
   - __Four combinations are possible__   
      - Host mode with AH   
      - Host mode with ESP   
      - Tunnel mode with AH   
      - Tunnel mode with ESP   
         - 가장 중요하고 가장 흔하다.   

### Firewalls   
   - __Firewalls: Why__   
      - __서비스 거부 공격 방지(prevent denial of service attacks)__   
         - __SYN flooding__   
            - 해커는 많은 가짜 TCP를 만들 수 있다.   
            - 실제 연결을 위한 어떠한 자원도 존재하지 않는다.   
      - __내부 데이터의 부적절한 접근/수정 방지(prevent illegal modification/access of internal data)__   
         - ex) 해커는 CIA의 홈페이지를 다른 것으로 바꿀 수 있다.   
      - __내부 네트워크에 오직 인증된 사용자만 접근 허락(allow only authorized access to inside network)__   
         - 인증된 사용자 또는 호스트들의 집합   
      - __three types of firewalls__   
         - __stateless packet filters__   
         - __stateful packet filters__   
         - __application gateways__   
         
   - __Stateless packet filtering__   
      - router firewall을 통해 내부 네트워크가 인터넷에 연결   
      - 라우터는 패킷별로 filter한다.   
         - TCP/UDP source and destination port numbers   
         - source IP address, destination IP address   
         - ICMP message type   
         - TCP SYN and ACK bits   
         - __위의 4가지에 근거하여 drop/forward 할 지 결정한다.__   
         
      - __Access Control List(ACL)__   
         - 들어오는 패킷에 적용되는 rule의 table   
   - __Stateful packet filtering__   
      - __Stateless packet filtering은 TCP 연결이 되어이지 않아도 packet을 허용한다는 문제점이 있다.__      
      - __모든 TCP 연결 상태를 추적한 후 연결이 되어 있을 경우에만 packet을 허용한다.__   
         - SYN, FIN을 추적하여 들어오고 나가는 패킷이 정당한지 확인한다.   
         - 방화벽에서 활동하지 않는 연결에 대한 timeout을 통해 packet을 더 이상 허용하지 않는다.   
      - __ACL은 Packet을 허용하기전에 연결 상태 테이블을 확인하기 위해 check conxion을 추가   
      
   - __Application gateways__   
      - IP/TCP/UDP 필드는 물론 Application data에 따라 packet을 filter한다.   
         
    
