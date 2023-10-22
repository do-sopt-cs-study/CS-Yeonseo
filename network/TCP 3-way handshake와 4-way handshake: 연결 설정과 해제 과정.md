# TCP 3-way handshake와 4-way handshake: 연결 설정과 해제 과정

<br/>
<br/>

## **🌟** TCP란 ?

### 1. TCP의 개념

- `TCP(전송 제어 프로토콜)`은 두 개의 호스트를 연결하고 데이터 스트림을 교환하게 해주는, 네트워크 프로토콜 중 하나
- IP와 함께 TCP/IP라는 명칭으로 사용된다.
- OSI 7계층 중 `4계층인 전송(Transport) 계층`에 위치하면서, 근거리 통신망이나 인트라넷, 인터넷에 연결된 컴퓨터에서 실행되는 프로그램 간에 **일련의 옥텟을 안정적으로, 순서대로, 에러 없이 교환할 수 있게 해주는 프로토콜이다.**

### 2. TCP의 특징

- (1) **신뢰성 보장** :
  - 패킷 손실, 중복, 순서 바뀜 등이 없도록 보장한다
  - TCP 하위 계층의 신뢰성 없는 서비스를 보완하여 신뢰성을 제공한다.
- **(2) 연결 지향적 :**
  - 3-way handshaking과정을 통해 연결을 설정하고, 4-way handshaking을 통해 연결을 해제한다.
  - 같은 전송 계층의 UDP가 비연결성인 것과 달리, TCP는 연결 지향적이다.
    - UDP : 비연결성, 비교적 속도 빠름, 비교적 신뢰성 낮음
    - TCP : 연결 지향적, 비교적 속도 느림, 비교적 신뢰성 높음
      ⇒ 즉, 연속성보다 신뢰성 있는 전송이 중요할 때 TCP를 사용한다.
- **(3) 흐름 제어** :
  - 흐름 제어 기능을 활용하여 송신 전송률 및 수신 처리율 속도를 일치시킨다.
  - 수신자의 버퍼 오버플로우를 방지하는 기능을 한다.
- **(4) 혼잡 제어 :**
  - 네트워크가 혼잡하여 네트워크 내 패킷 수가 넘치지지 않도록 방지하는 기능을 한다
  - 혼잡하다고 판단될 때는 패킷 전송량, 즉 송신율을 감속하는 혼잡제어 기법을 사용한다.

### 3. TCP 연결상태

1. `CLOSED` : 포트가 닫힌 상태
2. `LISTEN` : 포트가 열린 상태로 연결 요청 대기 상태
3. `SYN_RCV` : SYNC 요청을 받고 상대방의 응답 대기 상태
4. `ESTABLISHED` : 포트 연결 상태

### 4. TCP 플래그

1. `SYN` : 연결 설정 (Synchronize Sequence Number)
2. `ACK` : 응답 확인 (Acknowledgement)

   → 패킷을 받았다는 것을 의미

3. `FIN` : 연결 해제 (Finish)

   → 더 이상 전송할 데이터가 없음을 의미

## 5. TCP 세그먼트(Segment)

TCP에서의 패킷으로, 전송할 데이터 바이트와 TCP에 의해 데이터에 추가되는 헤더로 구성된다.

<br/>

---

<br/>

## **🌟 TCP의 3-Way Handshake**

### ❓ 3-Way Handshake란?

- TCP 통신을 이용하여 데이터를 전송하기 위해 **네트워크 연결을 설정(Connection Establish)하는 과정**
- 양쪽 모두 데이터를 전송할 준비가 되었다는 것을 보장, 데이터 전달 시작 전 다른 한 쪽이 준비 되었음을 알 수 있도록 한다.
  - TCP/IP 프로토콜을 이용해 통신을 하는 응용 프로그램이 데이터 전송 전 수신측과 사전에 세션을 수립하여 정확한 전송을 보장하는 과정이다.

Client가 Server에 연결을 요청 했다고 가정하자. TCP의 3-Way Handshake는 다음과 같은 3단계를 거쳐 네트워크 연결을 설정한다.

![Untitled (1)](https://github.com/do-sopt-cs-study/CS-Yeonseo/assets/77691829/af6da5fd-e154-4042-aefa-af80dafb8dba)

#### [Step 1] Client ⇒ Server : SYN

- Clinet는 Server에 connection을 생성하기 위해 **연결 요청 메시지 SYN(x)**를 보낸다.
- Client는 최초로 데이터를 전송할 때 Sequence Number를 임의의 랜덤 숫자로 지정하고, SYN 플래그 비트를 1로 설정한 세그먼트를 전송한다.
- Port 상태
  - **Client** : `Closed` → `SYN_SENT`
  - **Server** : `LISTEN`

#### [Step 2] Server ⇒ Client : ACK + SYN

- Server는 SYN(x)를 받고, Client에게 받았다는 신호로 **ACK(x+1)와 SYN(y) 패킷을 보낸다.**
  - 이때 Server는 Acknowledgement Number 필드를 (Sequence Number + 1)로 지정하고, SYN과 ACK 플래그 비트를 1로 설정한 세그먼트를 전송한다.
- 요청을 수락했으며, 포트를 열어달라는 메시지를 전송한 후, Client의 ACK 답을 기다린다.
- Port 상태
  - **Client** : `ClOSED`
  - **Server** : `SYN_RCV`

#### [Step 3] Client ⇒ Server : ACK

- Client는 Server의 ACK(x + 1)와 SYN(y) 패킷을 응답으로 받고, ACK(y+1)을 Server에 보낸다.
  - 이를 통해 Client가 수락을 확인 했음을 보내고, 연결을 맺는다
- 이때, 전송할 데이터가 있으면 이 단계에서 데이터를 전송할 수 있다.
- Port 상태
  - **Client** : `ESTABLISED`
  - **Server** : `SYN_RCV` ⇒ `ACK` ⇒ `ESTABLISHED`

---

## **🌟 TCP의 4-Way Handshake**

### ❓ 4-Way Handshake란?

- **TCP의 연결을 해제(Connection Termination)하는 과정이다.**
- 연결 해제를 뜻하는 `FIN 플래그`를 사용하여 안전하게 세션을 종료시키고, 더 이상 보낸 데이터가 없음을 나타낸다.
- Termination의 종류
  - `Graceful Connection Release (정상적인 연결 해제`) : 양쪽 커넥션이 서로 모두 커넥션을 닫을 때까지 연결되어 있다.
  - `Abrupt Connection Release (갑작스런 연결 해제)` : 갑자기 한 TCP 엔티티가 연결을 강제로 닫는 경우, 한 사용자가 두 데이터 전송 방향을 모두 닫는 경우이다.

Client가 Server에 연결 해제를 요청했다고 가정하자. TCP의 4-Way Handshake는 다음과 같은 3단계를 거쳐 네트워크 연결을 해제한다.

![Untitled](https://github.com/do-sopt-cs-study/CS-Yeonseo/assets/77691829/131094d1-6488-4a73-9033-f5053e6af0fb)

#### [Step 1] Client ⇒ Server : FIN(+ACK)

- Client가 close()를 호출하며 연결을 종료하겠다는 FIN 플래그(FIN 플래그에 ACK도 포함 됨)를 전송한다.
  - 이를 통해 더 이상 데이터를 전송하지 않을 것임을 알린다.
- 이때 Server가 FIN 플래그로 응답하기 전까지 연결을 계속 유지한다.
- Port 상태
  - **Client** : `FIN-WAIT_1`
  - **Server** : `ESTABLISHED`

#### [Step 2] Server ⇒ Client : ACK

- Server는 FIN을 받고, 확인했다는 ACK를 Client에 보낸다.
  - 이 때, Acknowledgement Number 필드를 (Sequence Number + 1)로 지정하고, ACK 플래그 비트를 1로 설정한 세그먼트를 전송한다.
- 이때 남은 데이터가 있다면 마저 전송을 마친 후에 close()를 호출한다.
- Port 상태
  - **Client** : `FIN-WAIT_1` → `FIN_WAIT_2`
  - **Server** : `CLOSE_WAIT` → 전송을 모두 마친 후 close() 호출

#### [Step 3] Server ⇒ Client : FIN

- 모든 데이터 전송을 완료했다면, Server는 연결 종료에 합의한다는 의미로 FIN 패킷을 Client에 보낸다.
- Server는 승인 번호를 보내줄 때까지 기다린다.
- Port 상태
  - **Client** : `FIN_WAIT_2`
  - **Server** : `LAST_ACK` → 승인 번호를 보내줄 때까지

#### [Step 4] Client ⇒ Server : ACK

- Client는 FIN을 받고, 확인했다는 ACK를 Server에 보낸다.
- 이 때 Client는 아직 받지 못한 데이터가 있을 수 있으므로 `TIME_WAIT`를 통해 기다린다.
  - 이를 통해, **의도치 않은 에러로 연결이 데드락에 빠지는 것을 방지**
  - 만약 에러로 종료가 지연되거나 타임이 초과되면 CLOSED로 돌아간다.
- Server는 ACK를 받고 소켓을 닫는다.
- Client는 TIME_WAIT이 끝나면 소켓을 닫는다.
- Port 상태
  - **Client** : `TIME_WAIT` → `CLOSED`
  - **Server** : `CLOSED`
    <br/>

---

<br/>

## 🌟 3-Way Handshake vs 4-Way Handshake

### 1. 3-Way HandSahke (연결 설정)

- 연결 설정 시에는 클라이언트의 연결 요청 시점과 서버의 연결 요청 시점이 거의 동시에 진행된다.
- 따라서 서버는 클라이언트에 대한 응답과 연결 요청을 `SYN + ACK` 로 압축하여 보낼 수 있다.

### 2. 4-Way HandSahke (연결 종료)

- 연결 종료 시에는 클라이언트가 데이터 전송을 마쳤다고 하더라도 **서버는 아직 보낼 데이터가 남아있을 수 있기 때문에**, `**FIN`에 대한 `ACK`만 우선적으로 보낸다.\*\*
- 데이터를 모두 전송한 이후에 `FIN` 메시지를 보낸다.

출처 :

https://gmlwjd9405.github.io/2018/09/19/tcp-connection.html
https://seolhee2750.tistory.com/261
