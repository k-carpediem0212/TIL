# 네트워크 (Network)
## 프로토콜 (Protocol)
> 시스템 간의 통신을 하기 위한 약속(규약)
> 구문, 의미, 타이밍으로 이루어져 있다.

- 구문
    송/수신 Data Format
- 의미
    데이터의 각 항목이 가지는 의미
- 타이밍
    데이터 송/수신 동작방식을 정의 하는 것


***

## OSI 7 Layer
> ISO(국제 표준화 기구)에서 제정한 개방형 시스템간 상호연결(OSI) 모델
> 분산된 이기종 시스템 간 네트워크 상호 호환을 위해 표준 아키텍처를 정의한 참조모델
> 목적에 맞게 계층적으로 나누어진 여러 프로토콜들을 조합하는 방식 (분할 정복)
> 상/하위 계층 간 인터페이스만 충족시키면 되도록 표준화 함

### (1) Physical Layer
> 디지털 데이터를 전기적인(광학적인) 신호로 변환하여 입출력을 담당하는 계층

- 특징
    + 주소정보가 없다.
      목적지를 인식할 수 없고, 전기적인 신호만 연결된 모든 노드에게 전달
- 데이터 단위
    + Bit 또는 Signal
- 주요 장비
    + HUB(허브, Dummy Hub)
      들어온 신호를 연결된 모든 포트로 전달하는 중계 장치
    + REPEATER(리피터)
      감쇠된 전송신호를 새롭게 재생(증폭)하여 다시 전달하는 재생 중계 장치
- 주요 공격 기법
    + Dummy Hub는 연결된 모든 노드로 Packet을 전송하기 때문에 허브에 연결되어 있는 모든 노드는 스니핑(Sniffing)이 용이하다. NIC(Network Interface Card)의 기본 동작모드는 자신을 목적지 주소(MAC 주소)로 하는 패킷이 아니면 폐기하지만 Promiscuous Mode로 설정하면 자신이 목적지가 아니더라도 모두 수신하여 손쉽게 스니핑이 가능하다.

### (2) Data-Link Layer
> 인접 노드간의 신뢰성 있는 Frame 전송을 담당하는 계층

- 특징
    + 목적지 노드를 찾아가기 위한 물리적인 주소로 MAC 주소(NIC별 고유)를 사용
    + LLC(Logical Link Control)와 MAC(Media Access Control)으로 하위 계층을 분리
      `LLC는 네트워크 계층과 연결 담당. 상위 계층으로 안전하고 정상적인 데이터를 전달하기 위해 데이터 오류 검출 및 상위 프로토콜에 대한 타입 확인 수행`
      `MAC은 물리 계층과의 연결을 담당. 신호변환을 통해 물리적 계층과 연결하고 MAC 주소를 통해 주소를 확인하여 정상적으로 데이터가 송/수신되고 있는지를 확인`
    + 인접 노드간 신뢰성 있는 전송 수행
      * '신뢰성 있는 전송'이란 데이터의 안전한 전송을 보장해준다는 의미 (Data-Link Control)
      * Flow Control (흐름 제어)
          송신노드가 수신노드의 처리속도를 고려하여 이를 초과하지 않도록 전송을 컨트롤한다.
          'Stop-And-Wait 방식'과 'Sliding Window 방식' 2 가지가 존재
          `Stop-And-Wait(정지 대기) 방식 : 송신 측에서 프레임을 전송 한 후 확인은답을 받을 때까지 대기하는 방식`
          `Sliding Window(슬라이딩 윈도우) 방식 : 송신 측에서 수신 측의 확인 응답을 받기 전에 수신 가능 범위 내에서 여러 프레임을 전송하는 방식`
      * Error Contorl (오류 제어)
          전송 중 여러가지 원인(주파수 혼란, 감쇄, 잡음 등)에 의한 오류나 손실 발생 시 이를 해결하기 위한 제어방식
          '후진 오류 수정 방식(BEC)'과 '전진 오류 수정 방식(FEC)' 2 가지가 존재
          `후진 오류 수정 방식(BEC, Backward Error Correction) : 송신 측에서 데이터 전송 시, 오류를 검출할 수 있는 부가 정보를 함께 전송하여 수신측에서 이를 점검하여 오류 발생 시 재전송을 요청(ARQ, Automatic Repeat Request)하는 방식`
          `전진 오류 수정 방식(FEC, Forward Error Correction) : 재전송이 불필요한 방식으로 송신 측에서 데이터 송신 시에 오류 검출 및 수정까지 가능한 부가정보를 함께 전송하는 방식`
      * Line Contorl (회선 제어)
          회선 구성 방식과 전송 방식에 따라 사용되는 전송 링크에 대한 제어 규범
          ```
          1. 회선 구성 방식(Line Configuration)
              (1) Point-To-Point(점-대-점)
                    두 장치간 전용 링크를 사용하는 방식
              (2) Multipoint(다중점)이 존재
                    셋 이상의 장치가 하나의 링크를 공유하는 방식
          2. 전송 방식(Transmission Mode)
              (1) Simplex(단방향)
                    단방향으로만 통신이 가능
              (2) Half-Duplex(반이중)
                    양방향으로 통신 가능
                    동시 통신 불가
              (3) Full-Duplex(전이중)
                    양방향 통신 가능
                    동시 통신 가능
          ```
- 데이터 단위
  + Frame
- 주요 장비
  + L2 Switch (스위치)
    * 내부적으로 MAC Address Table을 보유. 이 Table을 참조하여 목적지 MAC 주소에 해당하는 포트에 연결된 노드에게만 패킷을 전송
    * 특정 포트를 모니터링 하고자 한다면 모니터링 포트(Monitoring Port) 또는 네트워크 트래픽을 모니터링 할 수 있는 TAP 장비를 통해 패킷을 복제(Mirroring)하여 트래픽 분석 장비로 전달
    * 목적지로만 패킷을 전송하기 때문에 기본적으로 스니핑 불가. 스니핑을 하려면 스위치 재밍, ARP 스푸핑, ARP 리다이렉트, ICMP 리다이렉트, 스위치의 SPAN/Monitoring Port 이용 등이 있다.
    ```
    (1) Monitoring Port
          스위치 장비에서 제공하는 Port (관리 목적)
          스위치를 통과하는 모든 패킷을 복제해서 전달하는 포트
          시스코 장비에서는 SPAN(Switch Port Anlayzer) 포트라고 불림.
    (2) TAP 장비
          네트워크 상에서 전송되는 패킷을 중단 없이 모니터링 하기 위한 장비.
          네트워크 구간에 직접 연결하여 설치된 지점을 통과하는 모든 패킷 복제 및 모니터링
    ```
  + Bridge (브릿지)
    * 물리적으로 떨어진 동일한 LAN을 연결해주는 장비
- 주요 프로토콜
  + Ethernet, TokenRing, FDDI, X.25, Frame Relay 등
- 주요 공격 기법
  + Switch Jamming / MAC Flooding 공격
    * 스위치의 MAC Address Table의 버퍼를 OverFlow 시켜서 스위치가 허브처럼 동작하게 강제적으로 만드는 기법
    * 스위치는 Fail Safe/Open 정책을 따르는 장비이므로 장애가 발생하면 더미 허브처럼 연결된 모든 노드에 패킷을 전송
    * MAC Address Table을 OverFlow 시키기 위해 Source MAC 주소를 계속 변경하면서 패킷을 지속적으로 전송하는 방식으로 공격
  + ARP Spoofing
    * 공격자가 특정 호스트의 MAC 주소를 자신의 MAC 주소로 위조한 ARP Reply 패킷을 만들어 희생자에게 지속적으로 전송하면 희생자의 ARP Cache Table에 특정 호스트의 MAC 정보가 공격자의 MAC 정보로 변경된다.
    * 이를 통해 희생자로부터 특정 호스트로 나가는 패킷을 공격자로 향하도록 하여 스니핑하는 기법
    * 송/수신 패킷을 모두 스니핑 하기 위해서는 특정 호스트에도 ARP Spoofing을 수행
    * IP Forward 기능을 통해 희생자들이 스니핑을 인식하지 못하고 정상적인 통신이 되도록 한다.
      `일반적인 호스트 설정은 자신을 목적지 주소로 하지 않은 패킷을 폐기하지만 IP Forward 기능을 활성화 하면 라우터처럼 동작하여 자신이 목적지 주소가 아닌 패킷을 라우터처럼 라우팅 테이블을 참조하여 해당 목적지로 전송하게 된다.`
  + ARP Redirect
    * ARP Spoofing 공격의 일종
    * 공격자가 자신이 라우터/게이트웨이인 것처럼 MAC 주소를 위조
    * ARP Reply 패킷을 대상 네트워크에 지속적으로 브로드캐스트하면 해당 로컬 네트워크의 모든 호스트의 ARP CAche Table에 라이터/게이터웨이의 MAC 정보가 공격자의 MAC 정보로 변경
    * 이를 통해, 호스트에서 라우터로 나가는 패킷을 공격자가 스니핑하는 기법
    * ARP Spoofing과 동일하게 IP Forward를 이용하여 정상적인 것처럼 위장
  + ICMP Redirect
    * ICMP Redirect 메시지는 호스트-라우터 또는 라우터 간에 라우팅 경로를 재설정하기 위해 전송되는 메시지
    * 공격자가 특정 IP 또는 대역으로 나가는 패킷의 라우팅 경로를 자신의 주소로 위조한 ICMP Redirect 메시지를 생성하여 희생자에게 전송함으로써, 희생자의 라우팅 테이블을 변조하여 패킷을 스니핑하는 공격기법
    * ARP Spoofing과 동일하게 IP Forward를 이용하여 정상적인 것처럼 위장
  + Switch의 SPAN/Port Mirroring 기능 이용
    * Switch의 SPAN/Port Mirroring은 스위치를 통과하는 모든 트래픽을 볼 수 있는 기능으로 특정 포트에 분석장비를 접속하고 다른 포트의 트래픽을 분석장비로 자동 복사해 주는 기술
    * 공격자가 물리적으로 해당 포트에 접근할 수 있다면 손쉽게 스니핑 가능
```
장비 장애 관련 보안 정책
  (1) Fail Safe (장애 안전, Fail Open)
        - 장애 발생 시 모든 기능을 허용하는 정책
        - 보안성보다는 가용성을 더 중요시 하는 정책
  (2) Fail Secure (장애 보안, Fail Close)
        - 장애 발생 시 모든 기능을 차단하는 정책
        - 가용성보다는 보안성을 더 우선시하는 정책
```

### (3) Network Layer
> End Node간 라우팅을 담당

- 특징
    + 목적지를 찾아가기 위한 논리적인 주소로 IP를 사용
- 데이터 단위
    + Packet
- 주요 장비
    + Router / L3 Switch
      * Routing(최적의 경로를 선정해서 패킷을 포워딩 하는 기능)을 담당하는 장비
      * Data-Link 계층의 BroadCast와 MultiCast를 포워딩 하지 않음
      * 서로 다른 VLAN간에 통신을 가능 하게 함
      * 기본적인 보안 기능과 QoS(Quality of Service) 관련 기능을 지원하는 장비
- 주요 프로토콜
    + IP, IPX 등

### (4) Transport Layer
> End Node간 신뢰성있는 데이터 전송을 담당하는 계층
> Process To Process Communication

- 특징
    + 목적지를 찾아가기 위해(목적지 프로세스를 식별하기 위해) 논리적인 주소로 TCP/IP에서 Port를 사용
- 데이터 단위
    + Segment or Datagram
- 주요 장비
    + L4 Switch
        * SLB(Server Load Balancing) 기능
            서버 트래픽 부하분산과 Fail Over 기능 제공
            ```
              Fail Over
                장애 발생 시, 예비 시스템으로 자동전환되는 기능
            ```
- 주요 프로토콜
    + TCP, UDP, SCTP, SPX 등


```
** 알아 두기 **
신뢰성있는 데이터 전송을 보장하기 위한 기능 (TCP 해당)
  (1) 분할과 재조립
        원본 데이터를 전송 가능한 Segment 단위로 분할하여 전송
        목적지에서는 재조합하여 데이터를 복원
  (2) 연결 제어
        Connectionless 방식과 Connection-Oriented 방식 제공
          B. Connection-Oriented
              마치 물리적으로 연결 통로가 설정되어 있는 것처럼 동작
              필요한 정보를 주고 받는 연결설정과정과 연결종료과정을 거침
              대표적으로 TCP
          B. Connectionless
              양 호스트 사이에 연결 설정 및 종료과정이 없음
              대표적으로 UDP

  (3) 흐름 제어
        상호 간에 수신 가능한 만큼 전송해서 데이터 손실을 방지
        End Node간 흐름제어(Data-Link 계층은 인접 노드 간 흐름제어)
  (4) 오류 제어
        End Node간 전송중 오류 발생 시, 이를 교정
  (5) 혼잡 제어
        네트워크 혼잡도를 계산하여 전송량을 제어
```

### (5) Session Layer
> 논리적인 연결인 세션의 생성, 관리 및 종료를 담당하는 계층

### (6) Presentation Layer
> 데이터의 표현방식의 변환을 담당하는 계층
> ex) 인코딩/디코딩, 압축/압축해제, 암호화/복호화 등

### (7) Application Layer
> 사용자가 네트워크에 접근할 수 있는 인터페이스를 담당하는 계층
> 네트워크 서버/클라이언트 프로그램

- 데이터 단위
    + Data 또는 Message


***
##### 주요 용어 정리
```
1. Node
    Network에 연결된 Device를 의미
    (컴퓨터, 라우터, 스위치, 주변 기기 등)
2. Host
    Node 중 Server 역할을 하는 Node
3. End-Node (종단 노드)
    통신의 양 끝단에 해당하는 Node
    (최초 출발지/최종 목적지 Node)
4. Link
    Node-Node 간 Packet을 전달하기 위한 물리적인 통신경로
5. Intermediate Node (중계 노드)
    End-Node 사이에서 패킷을 중계해주는 역할을 하는 Node
6. Sniffing (스니핑)
    네트워크 중간에서 패킷을 도청하는 행위.
7. Routing (라우팅)
    목적지로 전송하기 위한 최적의 경로 설정 및 패킷을 교환하는 기능을 제공하는 것
8. 논리적인 주소
    변경이 가능한 주소
```
***
