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
   
__Packet__: 데이터 전송 단위   
   
대부분 TCP를 사용하며 처리해야 할 작업이 많기 때문에 비용이 더 비싸지만 UDP보다 속도가 느리다.   
   
__Protocol__: 컴퓨터와 컴퓨터 사이, 또는 한 장치와 다른 장치 사이에서 데이터를 원활히 주고받기 위하여 약속한 여러 가지 규약   
   
#### Network Core   
데이터 전송 방식
1. Circuit switching   
- 출발지에서 목적지까지의 길을 미리 지정   
ex) 무선 전화   
2. Packet switching   
- 사용자가 보내는 데이터를 패킷 단위로 받아서 올바른 방향으로 포워딩   
ex) 인터넷   
   
Circuit switching VS Packet switching   
- 1mb/s link가 있고 각 유저가 active일 때 100kb/s를 보낸다고 가정할 경우   
   
Circuit switching 사용 시 최대 10명의 유저까지 지원   
Packet switching 사용 시 받는 대로 보내기 때문에 제한이 없지만 10명 이상이 몰릴 경우    

peer-peer model: 
ex) skype, bittorrent, kazaa
