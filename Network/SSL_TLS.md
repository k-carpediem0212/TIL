# SSL/TLS
최초의 SSL은 안전하지 않는 전문들을 암호화하여 안전하게 전달하기 위해 Netscape사에서 개발되었다.

SSL version 1은 실제로 발표되지는 않았었고, SSL version 2가 공개적으로 발표되었지만 많은 보안 취약점이 존재하였다. 그래서 이를 보안한 것이 SSL version 3이며 이를 기반으로 표준화 시킨 것이 TLS이다. SSL/TLS라고도 많이 불린다.

실제 매칭되는 버전은 아래와 같다.
  + SSL v3 : SSL v3.0
  + SSL/TLS v1.0 : SSL v3.1
  + SSL/TLS v1.1 : SSL v3.2  
  + SSL/TLS v1.2 : SSL v3.3

SSL/TLS의 가장 큰 목적은 "데이터를 안전하게 전달하는 것"이다.
TCP 패킷은 전송되는 데이터가 모두 노출이 되어있어 악의적인 공격자에 의해 쉽게 탈취 및 조작이 가능하다. 이를 방지하도록 도움을 주는 것이 SSL/TLS이다.

SSL/TLS는 클라이언트-서버 환경에서 TCP 기반의 Application에 대한 End-To-End 보안서비스를 제공하기 위해 만들어진 전송계층 보안 프로토콜이다.

SSL/TLS는 TCP/IP 위에서 동작한다. 이 때문에 SSL/TLS가 가진 보안성과 TCP/IP의 안전성을 가지고 있다.

TCP/IP Layer에서 TCP/IP Layer와 Application Layer 사이에서 동작한다고 보는 것이 적당하다. 다양한 TCP 기반의 Application Protocol에 보안 서비스를 제공한다.


## SSL/TLS 보안서비스
SSL/TLS는 기밀성, 무결성, 인증을 제공한다.
  1. 기밀성(Confidentiality)
      대칭키 암호화를 이용한 송/수신 메시지 암호화를 통해 기밀성을 제공
  2. 무결성(Integrity)
      메시지 인증 코드(MAC:Massage Authentication Code)를 통해 송/수신 메시지의 위/변조 여부를 확인할 수 있는 무결성 제공
  3. 인증(Authentication)
      공개키 인증서를 이용한 클라이언트-서버 간 상호 인증 수행


## SSL/TLS Layer 구성
SSL/TLS 계층은 세부적으로 상위 계층과 하위 계층으로 나누어 진다.
  + 상위 계층
    - Handshake Protocol
      종단 간에 보안 파라미터(Security Parameter)를 협상하기 위한 프로토콜
    - Change Cipher Spec Protocol
      종단 간에 협상된 보안 파라미터를 이후부터 적용/변경함을 알리기 위한 프로토콜
    - Alert Protocol
      SSL/TLS 통신 과정에서 발생하는 오류를 통보하기 위한 프로토콜
    - Application Protocol
      Application 계층의 데이터를 전달하기 위한 프로토콜
  + 하위 계층
    - Record Protocol
      적용/변경된 보안 파라미터를 이용하여 실제 암/복호화, 무결성 보호, 압축/압축해제 등의 기능을 제공하는 프로토콜

또한, SSL/TLS는 상태 유지(Stateful) 프로토콜이다.
  + Session과 Connection 기반의 상태유지 프로토콜
    - Full Handshake(완전 협상)을 통해 Session을 생성하고, 이 세션 정보를 공유하는 다수의 Connection을 Abbreviated Hadnshake(단축 협상)을 통해 생성한다.
  + 세션 상태(Session State) 정보
    - 양 종단 간에 완전 협상을 통해 생성되는 상태정보
    - 세셔인 유지되는 동안 지속적으로 다수의 연결에 의해 사용되는 보안 파라미터 정보가 관리된다.
    - 대표적인 상태정보로 "대칭 암호 알고리즘", "HMAC용 해시 알고리즘" 등이 있다.
    - 상태 정보 내 필드
      * Session ID
        : 세션 식별자 (32byte)
      * Peer Certificate
        : 상대방의 인증서
      * Cipher Spec
        : 암호 명세 (대칭 암호 알고리즘, 키 길이, 블록 암호 모드, HMAC용 해쉬 알고리즘)
      * Compression Method
        : 압축 방식 (현재는 Null만 정의)
      * Master Secret
        : 키 블럭을 생성하기 위해 서버와 클라이언트가 공유하는 비밀 값(48Byte)
        : 완전 협상을 통해 생성한 premaster secret, server random, client random을 조합/해시하여 이 값을 생성
      * Is resumable
        : 현재 세션의 재사용 가능 여부에 대한 Flag
          (재사용 된다는 것은 여러 연결에 의해 다시 사용될 수 있는지의 의미)
  + SSL/TLS 연결 상태(Connection state) 정보
    - 실제 통신이 이루어지는 단위
    - 단축 협상을 통해 생성되며 세션 상태를 공유하며 통신을 수행
    - 대표적인 상태정보로 암호키, 인증키 등이 존재
    - 연결 상태 정보내 필드
      * server/client random
        : 단축 협상을 통해 서버-클라이언트가 생성한 난수 (32Byte)
        : 세션에 저장된 master secret과 단축 협상을 통해 생성된 client random, server random을 조합/해시하여 Key Block을 생성한 후 이를 이용하여 다양한 키를 만들어냄
      * server/client write key
        : 서버-클라이언트가 암호화에 사용하는 비밀키
      * server/client write MAC secret
        : 서버-클라이언트가 메시지 인증 코드(MAC) 생성시 사용하는 인증키
      * server/client write IV
        : 서버/클라이언트가 블록 암호 모드에 사용하는 IV(Initialize Vector)
      * sequence number
        : 전송 메시지 순번
  + 세션 정보와 연결 상태정보를 이용한 키 생성 과정
      1. 완전 협상을 통해 주고 받은 premaster secret, client random, server random을 조합하여 master secret을 생성
      2. master secret이 다수 연결에 이용 될 수 있도록 세션 상태 정보에 저장
      3. 단축 협상을 통해 주고 받은 client random, server random과 세션에 저장된 master secret을 조합하여 해시한 결과로 Key Block을 생성
      4. Key Block으로부터 서버/클라이언트 각각의 암호용 비밀키, MAC용 인증키, 블록 암호모드용 IV를 계산

## 완전 협상 과정(Handshake Protocol)
  1. Client Hello (Client -> Server)
      클라이언트가 지원 가능한 SSL/TLS 버전, Cipher Suites, 압축 방식 등을 서버에 전달하는 메시지
      + client random
        - 클라이언트가 생성한 32Byte 난수
      + session ID
        - 서버 세션을 식별하기 위한 ID
        - 처음 세션 생성 시에는 빈 값을 전달 (완전 협상)
        - 이미 생서된 세션 재사용 시에는 세션 ID를 전달(단축 협상)
      + cipher Suites
        - 클라이언트에서 지원 가능한 Cipher Suite 정보
        - "키 교환 및 인증 알고리즘", "암호 명세"로 구성
        - <형식> SSL/TLS_(A)_WITH_(B)
          A - 키교환 및 인증 알고리즘, B - Cipher Spec
        - ciper spec은 대칭 암호 알고리즘, 암호키 길이, 블록 암호 모드, HMAC용 해시 알고리즘 등으로 구성
  2. Server Hello (Server -> Client)
      사용할 SSL/TLS 버전, Cipher suite, 압축 방식 등을 클라이언트에 전달하는 메시지
      + server random
        - 서버가 생성하는 32Byte 난수
      + session ID
        - 새롭게 생성하거나 존재하는 세션 정보
  3. Server Certificate (Server -> Client)
      + 필요시 서버인증서 목록을 클라이언트에 전달하는 메시지
  4. Server Key Exchange (Server -> Client)
      + 키 교환 알고리즘
  5. Server Certificate Request (Server -> Client)
      + 필요시 클라이언트 인증을 위한 인증서를 요청하는 메시지
        * 요청 시에는 서버 측에서 인증 가능한 인증 기관 목록을 제공
  6. Server Hello Done (Server -> Client)
      + Server Hello 과정을 종료하는 메시지
  7. Client Certificate (Client -> Server)
      + 필요시 클라이언트 인증서 목록을 전달하는 메시지
  8. Client Key Exchange (Client -> Server)
      + 키 교환에 필요한 premaster secret을 생성하여 서버에 전달하는 메시지
      + 알고리즘에 따라 premaster secret을 생성하는 방식이 다르다.
          * RSA 방식 : premaster secret 생성 후 수신한 서버 인증서의 공개키를 이용하여 암호화 전송
          * Diffie-Hellman 방식 : 클라이언트 Diffie-Hellman 공개키를 생성하여 서버에 전달, 클라이언트와 서버는 각각 Diffie-Hellman 연산을 통해 공통의 premaster secret을 생성
  9. Certificate Verify (Client -> Server)
      + 필요시 클라이언트가 보낸 인증서에 대한 개인키를 클라이언트가 가지고 있음을 증명하는 메시지
      + 지금까지 Handshake 과정에서 주고받은 메시지와 master secret을 좋바한 해시값에 클라이언트 개인키로 서명하여 전달
  10. [Change Cipher Spec] (Client -> Server)
      + 협상한 cipher spec을 이후부터 적용/변경함을 알리는 메시지
  11. Finished (Client -> Server)
      + 협상 완료를 서버에 알리는 메시지
  12. [Change Cipher Spec] (Server -> Client)
      + 협상한 cipher spec을 이후부터 적용/변경함을 알리는 메시지
  12. Finished (Server -> Client)
      + 협상 완료를 클라이언트에 알리는 메시지



```
SSL/TLS 에서 Diffie-Hellman 키교환 유형
1. Ephemeral Diffie-Hellman (임시 디피-헬만)
    매 협상 시마다 새로운 Diffie-Hellman 개인키를 생성하고 공개 Diffie-Hellman 매개변수에 대한 서명을 통해 인증을 수행하는 방식
2. Anonymous Diffie-Hellman (익명 디피-헬만)
    임시 디피-헬만과 동일하게 새로운 Diffie-Hellman 개인키를 생성하지만 매개변수에 대한 인증을 수행하지 않기 때문에 중간자 공격에 취약하여 해당 키 교환 방식의 cipher suite는 사용하지 않는 것을 권장한다.
```
