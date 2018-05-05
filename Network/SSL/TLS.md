# SSL/TLS
최초의 SSL은 안전하지 않는 전문들을 암호화하여 안전하게 전달하기 위해 Netscape사에서 개발되었다.

SSL version 1은 실제로 발표되지는 않았었고, SSL version 2가 공개적으로 발표되었지만 많은 보안 취약점이 존재하였다. 그래서 이를 보안한 것이 SSL version 3이며 이를 기반으로 표준화 시킨 것이 TLS이다. SSL/TLS라고도 많이 불린다.

실제 매칭되는 버전은 아래와 같다.
  + SSL v3 : SSL v3.0
  + SSL/TLS v1.0 : SSL v3.1
  + SSL/TLS v1.1 : SSL v3.2  
  + SSL/TLS v1.2 : SSL v3.3

SSL/TLS의 가장 큰 목적은 **"데이터를 안전하게 전달하는 것"**이다.
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
          완전 협상을 통해 생성한 premaster secret, server random, client random을 조합/해시하여 이 값을 생성
      * Is resumable
        : 현재 세션의 재사용 가능 여부에 대한 Flag
          (재사용 된다는 것은 여러 연결에 의해 다시 사용될 수 있는지의 의미)
