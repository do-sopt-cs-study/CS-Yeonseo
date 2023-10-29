# 📚 TLS/SSL 프로토콜의 역할과 handshake: 암호화된 통신 설정 과정

# 🌟 SSL과 TLS

## SSL(Secure Socket Layer) 정의

- **암호화 기반 인터넷 보안 프로토콜로, 전달되는 모든 데이터를 암호화하고 특정한 유형의 사이버 공격도 차단한다.**
- 탄생 배경 :
  - 본래 웹에서의 데이터는 누구나 읽을 수 있는 일반 텍스트 형태로 전송 되었다.
  - 이러한 문제로 인터넷 통신의 개인정보 보호, 인증, 데이터 무결성이 보장이 필요해졌다.
  - 이를 위해 Netscape가 1995년 처음 개발하였다.
- TLS의 전신으로, SSL 3.0 이후 업데이트 되지 않고 사용 중단이 권장되고 있다.

## TLS(Transport Layer Security)의 정의

- 최신 암호화 프로토콜이다.
- **SSL의 업데이트 버전이다.** ⇒ SSL 암호화로 혼용해서 부르는 경우도 많다.
  - 더욱 향상된, 안전한 버전의 SSL로 TLS 이후에 SSL은 사용되지 않는다.
  - SSL 3.0 (최종 버전)과 TLS의 최초 버전의 차이가 크지 않다.
  - SSL의 업데이트 버전이며, 소유권 변경의 이유로 명칭만 다르다고 볼 수 있다.

## **SSL vs TLS (정리)**

- 둘다 보안 통신에 사용되는 **암호화 프로토콜**이다.
- SSL은 컴퓨터 네트워크에서 통신 보안을 제공하는 프로토콜이다.
- **TLS는 SSL의 업데이트 버전이며 추가 개인 정보 보호 및 보안 기능으로 구성된다.**

## TLS/SSL이 하는 것

- TLS/SSL 프로토콜은 **암호화, 인증, 무결성을** 기본적으로 지원한다.
  - `**암호화(Encryption)**` : 제 3자로부터 전송되는 데이터를 숨긴다.
  - `**인증(Authentication**)` : 정보를 교환하는 당사자가 요청된 당사자임을 보장한다.
  - **`무결성(Integrity)`** : 데이터가 위조되거나 변조되지 않았는지 확인한다.
- **HTTPS 뿐만 아니라 FTP, XMPP 등 응용 계층(Application Layer) 프로토**콜의 종류에 상관 없이 사용할 수 있다는 장점이 있다.
  - ❓`HTTPS`
    - HTTP는 HTTP 계층 아래에 TLS/SSL이라는 보안 계층이 추가 된 것으로, **HTTP 메시지에 포함되는 콘텐츠 정보에 보안 요소가 추가되어 데이터를 안전하게 주고 받을 수 있다.**

---

# 🌟 TLS/SSL HandShake

## 정의

- HTTPS에서 클라이언트와 서버간의 통신 전에 **TLS/SSL 인증서로 신뢰성 여부(무결성)을 확인하고 대칭키를 전달하는 과정**

## TLS/SSL HandShake 과정

![Untitled (8)](https://github.com/do-sopt-cs-study/CS-Yeonseo/assets/77691829/3ca1b1fe-d27f-4e24-b4a9-0f592019052d)

- **Step 1 - `Client : Client Hello`**
  - 클라이언트는 서버에게 **"client hello"** 메시지를 암호화된 정보와 함께 전송한다.
  - 이때 암호화된 정보에는 **TLS Version, 암호화 방식, Client Random Data(Client가 생성한 난수, 대칭키 만들 때 사용), Session Id, SNI(서버명)** 등이 포함된다.
- **Step 2 - `Server : Server Hello`**
  - 서버는 클라이언트가 보낸 Client Hello에 대해 **"server hello"메시지로** 클라이언트에게 응답한다.
  - **TLS Version, 암호화 방식(Client가 보낸 암호화 방식 중에 Server가 사용 가능한 암호화 방식을 선택), Server Random Data(Server에서 생성한 난수, 대칭키를 만들 때 사용), SessionID(유효한 Session ID)**이 포함된다.
- **Step 3 - `Server : Server Certificate`**
  - **서버의 인증서를 클라이언트에게 보내는 단계**로 필요에 따라 CA의 Certificate도 함께 전송한다.
  - 클라이언트는 이 패킷을 통해 서버의 인증서가 무결한지 검증한다.
- **Step 4 - `Server : Server Hello Done`**
  - 서버가 클라이언트에게 보낼 메시지를 모두 보냈다는 뜻
- **step 5 - `Client : Client Key Exchange`**
  - `pre-master secret(임시 키`) :
    - 인증서(CA)가 무결한지 검증되면, 클라이언트는 본격적으로 키를 주고 받기 위해 실제 데이터 통신에서 사용될 임시 키(pre-master secret)을 생성한다.
    - 클라이언트는 클라이언트의 난수와 서버의 난수를 조합하여 pre-master seceret을 만든다.
    - 임시 키는 대칭 키이기 때문에 노출 되어서는 안되는 정보이다. 따라서 앞서 갖고 있던 공개키로 암호화하여 서버에게 전달된다.
- **step 6 - `Server & Client : Change Cipher Spec`**
  - 키를 받은 서버는 자신이 갖고 있던 **비밀키로 복호화하여 임시 키(pre-master secret)을 전달 받는다.**
    → 이 단계를 거쳐 클라이언트와 서버가 같은 키를 갖게 된다.
  - **세션 키를 생성한다.**
    - 클라이언트와 서버는 클라이언트가 생성한 무작위 키, 서버가 생성한 무작위 키, premaster secret 키를 통해 세션 키를 생성한다.
    - 양쪽은 같은 키가 생성되어야 한다.
- **step 7 - `Server & Client : Finished`**

  - **클라이언트 준비 완료**
    - 클라이언트는 handshake 과정이 완료되었다는 finished 메시지를 서버에 보내면서, 지금까지 보낸 교환 내역들을 해싱 후 그 값을 대칭키로 암호화하여 같이 담아 보내준다.
  - **서버 준비 완료**
    - 서버도 동일하게 교환 내용들을 해싱한 뒤 클라이언트에서 보내준 값과 일치하는 지 확인한다. 일치하면 서버도 마찬가지로 finished 메시지를 이번에 만든 대칭키로 암호화하여 보낸다.
  - 클라이언트는 해당 메시지를 대칭키로 복호화하여 서로 통신이 가능한 신뢰받은 사용자란 걸 인지하고, **앞으로 클라이언트와 서버는 해당 대칭키로 데이터를 주고받을 수 있게 된다.**
    ⇒ SSL/TLS Handshake를 성공적으로 마치고 종료한다.
    ⇒ 이후 세션키를 사용해 대칭키 기법으로 통신이 계속된다.
    ⇒ 데이터 전송이 끝나면 세션을 종료해 통신을 마치고, 통신에서 사용한 세션 키도 함께 삭제한다.

- 출처

https://brunch.co.kr/@swimjiy/47

https://kanoos-stu.tistory.com/46

https://velog.io/@yuseogi0218/TLS-SSL-HandShake

https://www.cloudflare.com/ko-kr/learning/ssl/what-happens-in-a-tls-handshake/
