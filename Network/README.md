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
- 신뢰성있고 순서대로 데이터 전달   
- 흐름 제어 - 수신자의 수신 속도에 맞춰 송신자가 데이터 전달   
- 혼잡 제어 - 네트워크 혼잡 시 송신자의 데이터 전송률을 줄임    
   
__2. Connectionless service   
UDP(User Datagram Protocol)__   
- no 신뢰성, 순서, 흐름 제어, 혼잡 제어   
   
- 대부분 TCP를 사용하며 처리해야 할 작업이 많기 때문에 비용이 더 비싸지만 UDP보다 속도가 느리다.   
   
__Packet__: 인터넷에서의 데이터 전송 단위, 비트들의 집합   
   
__Protocol__: 컴퓨터와 컴퓨터 사이, 또는 한 장치와 다른 장치 사이에서 데이터를 원활히 주고받기 위하여 약속한 여러 가지 규약   
   
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


   
   
      
      
   
