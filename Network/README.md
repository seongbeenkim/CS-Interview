# Network

## 1. 컴퓨터 네트워크 기본   
### 네트워크 구성 요소   
__라우터__ : 메세지를 전달받아서 데이터를 전달, Network, Data link, Physical 계층으로 구성   
__네트워크 엣지__ : 어플리케이션, 호스트 (노트북, 데스크탑)    
__네트워크 코어__ : 라우터, 네트워크   
__네트워크 접근, 물리적 장치__ : 통신 링크 (무선랜,wifi 등)   
   
#### Network edge       
__end system(hosts)__: 어플리케이션 실행 ex) web, email   
__client/server model__: client host는 요청하고 항상 server로부터 서비스를 제공받는다.   
ex) Web browser/server, email client/server    
   
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
   
#### Network Core   
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

#### 라우터에서 Packet delay   
__1. nodal processing delay(처리 지연)__: 라우터에서 패킷을 받았을 때 검사(최종 목적지가 어디인지 다음 라우터는 무엇인지)하고 output link를 결정한다.   
__2. queuing delay(큐잉 지연)__: output link에 전송될 때까지 큐에서 대기하는 시간, 라우터의 혼잡 레벨에 따라 다르다.   
__3. transmission delay(전송 지연)__: 전송 시 걸리는 delay, output link로 첫번째 비트가 나가는 순간부터 마지막 비트가 나가는 순간까지 걸리는 시간   
- R = 링크 대역폭(bps), L = 패킷 크기(bits), L/R = 비트를 링크로 보내는데 걸리는 시간   
- R이 클수록 transmission delay가 적게 걸린다.   

__4. propagation delay(전파 지연)__: 데이터가 output link로 전달된 후 다음 라우터까지 도달하는데 걸리는 시간    
- d = 물리적 링크의 길이, s = propagation 속도, propagation delay = d/s   
- s는 전파이기 때문에 조절할 수 없으며, d가 짧을 수록 propagation delay가 적게 걸린다.   
   
#### delay 줄이는 방법   
__nodal processing delay__: 좋은 성능의 라우터를 사용   
__queuing delay__: x, 인터넷 사용량이 항상 다르기 때문이다.   
__transmission delay__: 링크 대역폭을 확장   
__propagation delay__: 물리적 링크 변경

__queue의 크기는 제한적이기 때문에 꽉 찬 상태에서 계속하여 패킷이 온다면 패킷 손실이 발생한다.__   
__- TCP에서 패킷 손실이 일어날 경우 어떻게 재전송을 하는가?__   
   1. 이전 라우터가 직접 재전송하는 방법
   2. 처음부터 재전송   
   __- 2번의 방법으로 처음부터 재전송을 한다. -> 현재 인터넷 디자인__   
   __1번의 방법을 사용하지 않는 이유__ - 라우터는 빠른 속도로 전달해야하기 위해 해야할 작업이 많으므로 이 전달을 위한 목적으로만 사용을 하기 때문이다.   
   dumb core: TCP 사이의 모든 라우터를 지칭 (TCP - 라우터 - 라우터 - 라우터 ... - TCP)   
   
__peer-peer model__: minimal use of dedicated servers   
ex) skype, bittorrent, kazaa   
   
## 2. 어플리케이션 계층   
우리가 사용하는 프로세스(프로그램)가 존재하는 계층  
프로세스는 app 개발자에 의해 통제되며 Socket을 통해 transport 계층과 연결된다.   
어플리케이션 계층을 제외한 transport, network, link, physical 계층은 OS에 의해 통제되며 physical 계층끼리 인터넷을 통해 통신한다.   
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
         
   __HTTP request, receive 하기 전에 TCP 연결을 먼저 해야한다.__   
   
   - __non-persistent HTTP__: TCP를 연결을 통해 최대 하나의 객체가 전달된 후 TCP 연결이 끊긴다. 다운로드하는 다수의 객체는 다수의 TCP 연결이 필요하다.   
      - RTT: 작은 패킷이 client에서 server로 갔다 다시 올 동안의 시간   
      - non-persistent HTTP response time = 2RTT + 파일 전송 시간
      ex) TCP 연결을 위한 RTT 하나, HTTP request와 반환 받아야할 response를 위한 RTT 하나, file 전송 시간 
   - __persistent HTTP__: 하나의 TCP 연결을 통해 다수의 객체가 전달될 수 있다.   
      - Pipelining 기법: 다수의 객체를 요청할 경우 요청할 수 만큼 연속적으로 request를 하고 연속적으로 response 받는 방식을 사용하여 client와 server간 요청과 응답의 효율성을 높인다.   
      
   __일반 web은 persistent HTTP pipelining 기법을 사용한다__   
      
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
### Multiplexing/Demultiplexing   
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
      - Message error   
      - Message loss   
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
                         - Window 안에 있는 Packet들은 수신자가 제대로 Packet을 못받을 경우 재전송해야하기 때문에 버퍼에 반드시 저장되어있어야 한다.   
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
                         - 버퍼가 순서대로 꽉 차게 되면 어플리케이션 계층에 넘겨주고 마지막 받은 Packet에 대한 ACK을 송신자에게 보내고 버퍼를 비운다.   
                     - 0,1,2,3 Packet 4개를 보냈는데 0에 대한 ACK를 받을 경우 1,2,3,4로의 범위로 바뀌고 4에 대해서 전송하고 Timer를 켜고 1에 대한 ACK를 받을 경우 2,3,4,5의 범위로 바꾸고 5를 전송하고 Timer를 키는데 2에 대한 ACK을 받지 못하고 2에 대한 Timer 제한 시간이 다 된 경우 2~5를 다시 재전송한다.   
                     - __즉 Packet loss 일어난 Packet만 다시 보낸다.__   
                     - But, Sequence number가 계속 증가하는데 Sequence number의 값의 크기는 최대한 작게 하는 것이 좋다.   
                        - __그러므로 Window size가 n일 경우 Sequence number의 범위를 2n으로 할 경우 재전송할 Packet에 대한 혼선없이 전송이 가능하다.__   
                     - __문제점은 모든 Packet이 Timer를 가져야하기 때문에 Window size가 클 경우 복잡해지기 때문에 잘 사용하지 않는다.__   
                        - 그래서 보통 Window를 대표하는 Timer를 하나를 가지고 사용한다.   

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
      - 수신자와 송신자가 각각 window 크기 만큼 고유의 send, receive buffer 2개를 가지고 있다.   
   - __full duplex data__   
      - 같은 연결에서 데이터가 양방향으로 갈 수 있다.   
      - MSS(Maximum Segment Size)   
   - __연결지향형__   
      - 3 way handshaking을 통해 데이터 교환전에 송신자와 수신자 연결   
   - __흐름 제어__   
      - 수신자가 받을 수 있는 만큼만 송신자가 데이터 전송   
   - __혼잡 제어__   
      - 내부 네트워크에서 전송할 수 있는 만큼만 데이터 전송   
   - __TCP Segment(32bit) 구조__   
      - __출발지 Port번호(16bit), 도착지 Port번호(16bit)__    
         - 2^16-1 대략 6만개 정도의 Port 번호가 존재   
      - __Sequence Number(32bit)__   
         - Seq n: data의 첫번째 byte의 number   
      - __Acknowledgement Number(32bit)__   
         - ACK n: 다음으로 받아야 할 byte의 Sequence Number, 받은 Seq n + 1을 보낸다.   
         - __데이터를 받을 경우 ACK을 바로 보내지 않는다. 일정 시간을 기다린 후 ACK를 보내는 이유(pending)__   
            1. 내가 보내고자 하는 데이터를 같이 보낼수도 있다.   
            2. pipelining 방식으로 데이터가 오기 때문에 일일이 ACK을 하지 않고 __현재까지 수신된 byte들을 단 하나의 ACK로 일괄 확인응답하는 Cumulative ACK__을 한다.   
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
      
      
                     
                  
      
      
      
   
         
   
      
      
   
