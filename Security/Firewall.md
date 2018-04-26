# Firewall (방화벽)
> 침입 차단 시스템
> 외부 네트워크와 내부 네트워크 사이의 정보 흐름을 통제하는 H/W 및 S/W 시스템
> 인터넷 망과 내부 망 사이에 존재하는 일종의 보호막

- 특징
    + ACL(Access Control List, 접근 제어 목록)을 통해 네트워크에 전송되는 트래픽에 대한 보안정책을 설정
    + 완벽한 보안을 보장하지는 않지만, 비교적 적은 비용으로 효과적인 차단이 가능
    + 보안 소프트웨어를 여러 호스트에 분산시키지 않고 Firewall에 집중하여 효율적인 관리가 가능
- 기능
    + 접근 통제
        * 정해진 규칙에 따라 외부에서 내부로 유입되는 메시지를 차단 및 허용하는 기능 제공 (Packet Filtering)
    + 사용자 인증
        * Firewall은 내/외부 네트워크 사이의 접점이기 때문에 Firewall을 지나가는 트래픽에 대한 사용자들의 신분을 증명하는 기능이 필요
        * 사용자 인증에서는 ID/PW 및 공개키 인증서를 이용하여 사용자를 식별하는 기능과 이를 검증하는 인증 과정으로 구성
        * 메시지 인증은 VPN과 같은 신뢰할 수 있는 통신회선을 통해 전송되는 메시지의 신뢰성을 보장
    + 감사 및 로그 기능
        * 망 사용 통계치 및 접속자 관리
    + 프라이버시 보호
        * 내부 네트워크 정보 유출 방지
        * 이증 DNS 기능과 Proxy 기능 등을 제공함으로써 개인정보와 관련된 정보의 노출을 막거나 공격자로부터 보호
    + 서비스 통제
        * 안전하지 못하거나 위험성이 존재하는 서비스를 필터링 함으로써, 내부 네트워크의 호스트가 갖고 있는 취약점을 감소 시킴
    + NAT
        * 사설 IP 주소를 공인 IP로 변환시켜 내부 네트워크의 IP가 외부에 공개 되지 않음

- 기본 보안 정책
    + 최소 권한
        * 사용자/관리자는 필요한 권한만을 가져야 함
    + 다중 방어
        * Firewall이 완벽하더라도 2중/3중으로 대비해야 함 (Firewall 공격 대비)
    + Choke Point(폐색부)
        * 공격자가 좁은 채널(입구)를 사용하도록 만들고, 그 곳을 모니터링하여 통제
    + 보안 취약 부분 점검(최적화)
        * 시스템에서 가장 약한 부분의 강도 = 전체 강도
        * 취약점은 제거, 제거가 불가능하면 주의깊게 관리 필요
    + 단순성
        * 관리자가 쉽게 이해하고 수정할 수 있어야 함

- 단점
    + Choke Point를 이용하여 접근 통제를 수행하기 때문에 Telnet,FTP 등 내/외부가 함께 사용하는 서비스에 장애 발생 가능
    + Back Door를 통하는 트래픽은 차단이 불가능
    + 패킷 내용 분석이 불가능하여 전자우편을 통해 유입되는 바이러스는 차단 불가 (ex. 랜섬웨어)
    + 악의적인 내부 사용자 및 스파이에 의한 침해를 방어하지 못함
    + 모든 트래픽이 Firewall를 경유하므로, 시스템 부하가 증가, 속도 저하

- 침입 차단 시스템 차단 방법에 따른 분류
    + 패킷 필터링 시스템
        * 패킷을 선택적으로 통제하는 네트워크 보안 메커니즘
        * Firewall에서 사용되는 라우터는 '스크린 라우터' 또는 '패킷 필터링 라우터'라고 불림
        * 발신지/목적지 주소, 사용중인 세션, 어플리케이션 프로토콜에 기반하여 데이터 흐름을 통제
        * 패킷 헤더 정보를 이용하기때문에 비교적 간단, 복잡한 환경 설정이나 절차가 필요하지 않음
    ```
    * 패킷 필터링 시스템의 보안 정책
      1. Default Accept
          거부 할 패킷을 정의, 나머지 모든 패킷은 허용
      2. Default Deny
          기본적으로 수신된 패킷은 거부, 허용될 패킷을 정의

    * 단점
        서비스 별 제어가 불가능
        로그/감사 기능 부족
        원격 관리 불가
        규칙 설정이 어렵고 테스트 불가
        UNIX 원격 명령에 대해서는 적용이 잘 안됨
    ```

    + 스테이트풀 패킷 검사 방화벽
        * 패킷 필터링과 동일하게 패킷 정보를 검토하지만, TCP 연결에 관한 정보를 기록
        * TCP 순서번호를 추적하여 세션하이재킹 공격을 차단

    + 응용 게이트웨이 방화벽
        * Proxy 서버를 사용
        * 응용 게이트웨이 호스트(Proxy)만 외부로 공개, 내부 네트워크 호스트는 외부에 노출 되지 않음
        * 모든 트래픽에 대해 인증 가능, 효과적인 로깅 기능 수행 가능
        * 인증과 로깅 소프트웨어와 하드웨어는 응용 게이트웨이에만 설치 됨으로 비용이 적게 소모
    + 회로 레벨 게이트웨이 방화벽
        * 독립적인 시스템 또는 응용 레벨 게이트웨이에 의해 수행되는 기능으로 운용 가능
        * 종단-종단간 연결을 중간에서 분리
        * 내부 사용자를 신뢰할 경우 사용


- 침입 차단 시스템 구성 방법에 따른 분류
    + 듀얼 홈드 게이트 웨이
        * 침입 차단 시스템 호스트에 두 개의 네트워크 인터페이스가 설치되어 내/외부 간 직접적인 트래픽을 차단
        * 주소변환을 통해 내/외부 데이터 전달
    + 스크린 호스트 게이트웨이
        * 듀얼 홈드 게이트와 스크린 라우터를 혼합하여 구성
        * 스크린 라우터는 정해진 정책에 따라 트래픽을 차단
        * 베스천 호스트는 내부 네트워크와 스크린 라우터 사이에 위치
    + 스크린 서브넷 게이트웨이
        * 내부네트워크와 외부네트워크 사이에 스크린 네트워크를 두고 스크린 네트워크와 내/외부 네트워크 사이에는 스크린 라우터를 위치하여 구성

***
# 개인 방화벽
> 개인 컴퓨터와 인터넷 또는 기업 네트워크간 트래픽을 제어
> 전형적으로 소프트웨어 모듈
> 허가 받지 않은 사용자 차단

***
# DMZ
> 외부 방화벽과 내부방화벽 사이의 구간
> 외부에서 접속 가능해야 하며 보호되어야 하는 시스템을 주로 DMZ에 위치 (ex, 웹서버, 이메일 서버 DNS 서버 등)

'외부 방화벽'은 인터넷이나 다른 WAN으로 연결되는 경계 라우터의 바로 안쪽 LAN이나 기업 네트워크의 끝에 위치하여 한개 이상의 '내부 방화벽'이 기업 네트워크 집단을 보호



***



```
** 용어 정리
1. 베스천 호스트(Hastion Hosts)
      외부 공격에 대해 가장 중요한 수비 부분
      침입 차단 시스템 관리자가 중점적으로 관리해야할 부분

```
***