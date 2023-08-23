# TCP 3 way handshake & 4 way handshake

앞선 장에서 정리했던 네트워크 OSI 4계층 트랜스포트 계층에서 사용되는 TCP & UDP  
TCP에서 3-way handshake 는 접속, 4-way handshake 접속 해제임. 과정을 알아보자.

## 3-way handshake

* 전송의 신뢰성을 보장하기 위해 통신 이전에 상대방 컴퓨터와 사전에 세션을 수립하는 과정.
* 양쪽 모두 통신 준비가 되어있다는 것을 확인하며 연결을 설정하는 과정임.

![image](https://github.com/wltnthss/Network/assets/60785586/ea37d784-4c95-4318-92d4-4f821756c48b)

### [State 정보]

* CLOSED: 포트가 닫힌 상태
* LISTEN: 포트가 열린 상태로 연결 요청 대기 중
* SYN_RECV: SYNC 요청을 받고 상대방의 응답을 기다리는 중
* ESTABLISHED: 포트 연결 상태
* TIME-WAIT: Server로부터 FIN을 수신하더라도 일정시간(default: 240초)동안 세션을 남겨놓고 잉여 패킷을 기다리는 과정

### [Flag 정보]

* TCP Header에는 CONTROL BIT(플래그 비트, 6bit)가 존재하며, 각각의 bit는 "URG-ACK-PSH-RST-SYN-FIN"의 의미를 가짐.
  * 즉, 해당 위치의 bit가 1이면 해당 패킷이 어떠한 내용을 담고 있는 패킷인지를 나타냄.
* SYN(Synchronize Sequence Number)
  * 연결 설정. Sequence Number를 랜덤으로 설정하여 세션을 연결하는 데 사용하며, 초기에 Sequence Number를 전송함.
  * 따라서, Connection을 생성할때 사용하는 flag.
* ACK(Acknowledgement)
  * 응답 확인. 패킷을 받았다는 것을 의미하는 flag.
  * Acknowledgement Number 필드가 유효한지를 나타냄.
  * 양단 프로세스가 쉬지 않고 데이터를 전송한다고 가정하면 최초 연결 설정 과정에서 전송되는 첫 번째 세그먼트를 제외한 모든 세그먼트의 ACK 비트는 1로 지정된다고 생각할 수 있음.
* FIN(Finish)
  * 연결 해제. 세션 연결을 종료시킬 때 사용되며, 더 이상 전송할 데이터가 없음을 의미.
  * 4way handshake에서 사용함.

### 3-way handshake 동작방식

1. Client가 Server에게 접속을 요청하는 SYN 플래그 요청 전송
2. Server 는 LISTEN 상태에서 SYN 이 들어온 것을 확인 후 SYN_RECV 상태로 바뀌어 SYN + ACK 플래그를 Client에게 전송함.
   그 후 Server는 다시 ACK 플래그를 받기 위해 대기상태로 변경됨
3. SYN + ACK 상태를 확인한 Client는 서버에게 ACK를 보내고 연결이 성립됨.

위와 같은 동작방식으로 3번의 핸드쉐이킹을 거쳐 연결을 맺는 것을 3-way handshake라고 함.

## 4-way handshake 

3-way handshake가 연결확립을 위해 진행했다면 4-way handshake는 세션을 종료하기 위해 수행되는 절차임.

![image](https://github.com/wltnthss/Network/assets/60785586/65b3bda8-87de-4ffd-a79a-48ff31cff53b)

### 4-way handshake 동작방식

1. Client가 Server에게 연결을 종료하겠다는 FIN 플래그를 전송함. 보낸 후에는 FIN-WAIT-1 상태로 변경됨.
2. FIN 플래그를 받은 Server는 확인메세지인 ACK를 Client에게 보냄. 그 후 CLOSE-WAIT 상태로 변경되고
   Client도 마찬가지로 Server에서는 종료될 준비가 됐다는 FIN을 받기위해 FIN-WAIT-2 상태로 변경됨.
3. CLOSE 준비가 다 된 후 Server는 Client 에게 FIN 플래그를 전송함.
4. Client는 해지 준비가 되었다는 정상응답인 ACK를 Server에게 보내줌. Client는 TIME-WAIT 상태로 변경됨.


