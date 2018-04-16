Spring 생명 주기
1. 생성
    Context 객체 생성
2. 설정
    Bean 설정 (xml파일 및 설정 Java파일 로드)
3. 사용
    getBean을 이용한 생성된 Bean 사용.
4. 종료
    Context Close를 통한 종료(자원 해제)


Bean 생명주기

InitializingBean => afterPropertiesSet 메소드 제공(override), 초기화 과정에서 호출
DisposableBean => destroy 메소드 제공(override)
소멸 과정에서 호출

컨테이너가 소멸되면 빈은 자동 소멸
빈만 소멸하기 위해서는 Bean의 destory 메소드를 이용


@PostConstruct
빈 초기화 과정에서 호출되는 메소드 지정
@PreDestory
빈 소멸 과정에서 호출되는 메소드 지정

빈의 Scope
default singleton
