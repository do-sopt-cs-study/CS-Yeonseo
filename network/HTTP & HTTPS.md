# 📚 HTTP & HTTPS: 웹 통신의 기초와 보안 버전의 차이

# 🌟 HTTP란?

## 정의

- `HTTP(Hyper Text Transfer Protocol)`이란 서버/클라이언트 모델을 따라 데이터를 주고 받기 위한 프르토콜이다.
- 인터넷에서 하이퍼 텍스트를 교환하기 위한 통신 규약이다.
- 응용 계층(Application Layer) 프로토콜로, TCP/IP 위에서 작동한다.
- 80번 포트를 사용한다.
  - 서버가 80번 포트에서 요청을 기다린다
  - 클라이언트는 80번 포트로 요청을 보낸다.

## 구조 및 작동 방식

![Untitled (9)](https://github.com/do-sopt-cs-study/CS-Yeonseo/assets/77691829/3c1ce9dd-5dfc-4804-997d-c89f6792caac)

- 상태를 가지고 있지 않은 Stateless 프로토콜이다.
- Method, Path, Version, Headers, Body 등으로 구성된다.
  - Method를 통해 여러 유형의 요청과 응답을 정의한다.
    - ex) `HTTP GET`, `HTTP PUT`
- 서버는 숫자 코드 및 데이터 양식으로 다양한 HTTP 응답을 전송한다.
  ex.
  - `200` - OK (정상)
  - `400` - Bad request (잘못된 요청)
  - `404` - Resource not found (리소스 찾을 수 없음)

## HTTP의 문제점

- 암호화가 되지 않는 평문 데이터를 전송하는 프로토콜로, 제 3자가 정보를 조회할 수 있다.
- 따라서 은행 거래나 비밀번호와 같은 중요한 정보를 주고 받을 경우, 정보가 노출 되기 쉽다는 보안 상 문제가 있다.

⇒ 이를 해결하기 위해 HTTPS가 등장하였다.

<br/>

---

# 🌟HTTPS란?

## 정의

- HTTPS(Hyper Text Transfer Protocol Secure)는 HTTP에 데이터 암호화가 추가 된 프로토콜이다.
- HTTP의 보안 취약점을 보완하기 위해 등항한 프로토콜로, 네트워크 상에서 중간에 제 3자가 정보를 볼 수 없도록 암호화를 지원하고 있다.
- 기밀성(사생활 보호), 데이터 무결성, ID 및 디지털 사용한 인증을 제공하는 방식이다.
- •보호의 수준은 웹 브라우저의 구현 정확도와 서버 소프트웨어, 지원하는 암호화 알고리즘에 따라 달라진다.
- 443번 포트를 사용한다.

## 작동 방식

- HTTPS는 HTTP 요청 및 응답을 TLS/SSL 기술에 결합했다.
- HTTPS는 독립된 인증 기관(CA)에서 TLS/SSL 인증서를 획득하여야 한다.
  - 인증서는 사용자가 사이트에 제공하는 정보를 암호화로 바꿔서 데이터로 전송한다.
  - 암호화하여 데이터베이스에 저장되기 때문에, 실제로 해킹을 당하더라도 해독할 수 없다.
  - HTTPS 인증서 발급 과정:
    ![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/719bebde-f624-4f14-821b-056d3cd72c51/c938fd2b-7397-47d1-a69a-cc6c3a6c5d74/Untitled.png)
  1. A기업은 HTTP 기반의 애플리케이션에 HTTPS를 적용하기 위해 공개키/개인키를 발급한다.
  2. CA 기업에게 돈을 지불하고, 공개키를 저장하는 인증서의 발급을 요청한다.
  3. CA 기업은 CA기업의 이름, 서버의 공개키, 서버의 정보 등을 기반으로 인증서를 생성하고, CA 기업의 개인키로 암호화하여 A기업에게 이를 제공한다.
  4. A기업은 클라이언트에게 암호화된 인증서를 제공한다.
  5. 브라우저는 CA기업의 공개키를 미리 다운받아 갖고 있어, 암호화된 인증서를 복호화한다.
  6. 암호화된 인증서를 복호화하여 얻은 A기업의 공개키로 세션키를 공유한다.

## 장단점

- 장점

  1. `보안` & `신뢰`

  - HTTPS의 가장 큰 장점은 보안이다.
  - 즉, 데이터 정보들을 암호화함으로써 중요한 정보를 보호할 수 있다.
  - 이를 통해 웹사이트의 무결성을 보호해준다.
  - 또한, 인증된 기관으로부터 TLS/SSL 인증서를 발급 받았음을 보장함으로 사이트의 신뢰도를 높일 수 있다.

  1. `SEO`

  - 네이버, 다음은 물론이고 구글 역시 검색 엔진 최적화(SEO: Search Engine Optimization) 관련 내용을 HTTPS 웹사이트에 대해서 적용하고 있다.
  - 즉, 키워드 검색 시 상위 노출되는 기준 중 하나가 보안 요소(tls/ssl화)이다.

  1. `모바일 친화`

  - HTTPS를 사용하면 모바일 친화적인 웹사이트를 만드는데 적용할 수 있는 여러 웹 기술을 사용할 수 있다.
  - 대표적으로 모바일 기기에서 훨씬 빠르게 콘텐츠를 로딩 하기 위한 HTML 프레임워크인 AMP(Accelerated Mobile Pages), 앱 설치과정 없이 URL 접속으로 어플과 유사한 퍼포먼스를 구현하는 프런트엔드 개발 환경인 PWA(Progressive Web App) 등의 웹 기술은 HTTPS를 적용한 웹사이트에서만 사용 할 수 있다.

<br/>

- 출처

https://mangkyu.tistory.com/98

[https://haileychoi15.medium.com/http-https-차이점-59b7be74cbd4](https://haileychoi15.medium.com/http-https-%EC%B0%A8%EC%9D%B4%EC%A0%90-59b7be74cbd4)

[https://haileychoi15.medium.com/http-https-차이점-59b7be74cbd4](https://haileychoi15.medium.com/http-https-%EC%B0%A8%EC%9D%B4%EC%A0%90-59b7be74cbd4)
