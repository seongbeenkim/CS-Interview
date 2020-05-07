# Network

## 1. 컴퓨터 네트워크 기본   
### 네트워크 구성 요소   
__라우터__ : 메세지를 전달받아서 데이터를 전달   
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
- no 신뢰성, 흐름 제어, 혼잡 제어   
   
- 대부분 TCP를 사용하며 처리해야 할 작업이 많기 때문에 비용이 더 비싸지만 UDP보다 속도가 느리다.   
   
__Packet__: 데이터 전송 단위   
   
__Protocol__: 컴퓨터와 컴퓨터 사이, 또는 한 장치와 다른 장치 사이에서 데이터를 원활히 주고받기 위하여 약속한 여러 가지 규약   
   
#### Network Core   
__데이터 전송 방식__   
__1. Circuit switching__   
- 출발지에서 목적지까지의 길을 미리 지정하고 전송   
ex) 무선 전화   

__2. Packet switching__   
- 사용자가 보내는 데이터를 패킷 단위로 받아서 올바른 방향으로 전송   
ex) 인터넷   
   
__Circuit switching VS Packet switching__   
- 1mb/s link가 있고 각 유저가 active일 때 100kb/s를 보낸다고 가정할 경우   
   
Circuit switching 사용 시 최대 10명의 유저까지 지원   
Packet switching 사용 시 받는 대로 보내기 때문에 제한이 없어 일반적으로 많이 사용하지만 10명 이상이 몰릴 경우 문제가 생길 수 있다.   

#### 라우터에서 Packet delay   
__1. nodal processing delay__: 라우터에서 패킷을 받았을 때 검사하고 output link를 결정한다.   
__2. queuing delay__: output link에 전송될 때까지 큐에서 대기하는 시간, 라우터의 혼잡 레벨에 따라 다르다.   
__3. transmission delay__: 전송 시 걸리는 delay, output link로 첫번째 비트가 나가는 순간부터 마지막 비트가 나가는 순간까지 걸리는 시간   
- R = 링크 대역폭(bps), L = 패킷 크기(bits), L/R = 비트를 링크로 보내는데 걸리는 시간   
- R이 클수록 transmission delay가 적게 걸린다.   

__4. propagation delay__: 데이터가 output link로 전달된 후 다음 라우터까지 도달하는데 걸리는 시간    
- d = 물리적 링크의 길이, s = propagation 속도, propagation delay = d/s   
- s는 전파이기 때문에 조절할 수 없으며, d가 짧을 수록 propagation delay가 적게 걸린다.   
   
#### delay 줄이는 방법   
__nodal processing delay__: 좋은 성능의 라우터를 사용   
__queuing delay__: x, 인터넷 사용량이 항상 다르기 때문이다.   
__transmission delay__: 링크 대역폭을 확장   
__propagation delay__: 물리적 링크 변경

__queue의 크기는 제한적이기 때문에 꽉 찬 상태에서 계속하여 패킷이 들어온다면 패킷 유실이 발생한다.__   
__- TCP에서 패킷 유실이 일어날 경우 어떻게 재전송을 하는가?__   
   1. 이전 라우터가 직접 재전송하는 방법
   2. 처음부터 재전송   
   __- 2번의 방법으로 처음부터 재전송을 한다. -> 현재 인터넷 디자인__   
   __1번의 방법을 사용하지 않는 이유__ - 라우터는 빠른 속도로 전달해야하기 위해 해야할 작업이 많으므로 이 전달을 위한 목적으로만 사용을 하기 때문이다.   
   dumb core: TCP 사이의 모든 라우터를 지칭 (TCP - 라우터 - 라우터 - 라우터 ... - TCP)   
   
__peer-peer model__: minimal use of dedicated servers   
ex) skype, bittorrent, kazaa   

