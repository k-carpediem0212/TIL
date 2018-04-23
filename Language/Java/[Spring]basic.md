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





외부 파일을 이용한 설정
1. Environment 객체
  Context -> getEnvironment() -> Environment -> getPropertySource -> PropertySources
2. 프로퍼티 파일을 이용한 설정
xml
  context:property-placeholder 를 이용 설정 파일 위치를 지정
  Property 태그를 이용 값 설정
Java파일
  java 파일 이용
  어노테이션 설정
3. 프로파일 속성을 이용한 설정
  xml
    profile 속성 이용
  Java파일
    @Configuration
    @Profile("명")





environmentaware implement 를 통해 environment 객체 받아와서
initializingBean 생성 시 초기화 해줄 수 있도록 구현.




AOP
상속을 통한 공통적인 기능 구현에는 한계가 있다.
(다중 상속 불가, 불필요한 로직이 클래스 안에 존재=> 코드 방대, 유지보수 어려움)
공통 기능을 분리 시켜 놓고, 공통 기능을 필요로 하는 핵심기능들에서 사용하는 방식(재활용)

Aspect : 공통기능
Advice : Aspect 기능 자체
JointPoint : Advice를 적용해야 되는 부분 (ex 필드, 메소드, 스프링에서는 메소드만 해당)
pointcut : JointPoint의 부분으로 실제로 Advice가 적용되는 부분
Weaving : Advice를 핵심 기능에 적용하는 행위


*스프링에서 AOP 구현에는 Proxy를 사용.

AOP 구현 방식
1. XML 스키마 이용
    1) AOP 의존 설정
    2) AOP 클래스 생성
    3) xml 파일 설정

2. @Asepct Annotation 이용

aop:before : 메소드 실행 전에 advice 실행
aop:after-returning : 정상적으로 메소드 실행 후에 advice 실행
aop:after-throwing : 메소드 실행중 exception 발생 시 advice 실행
aop:after : 메소드 실행중 exception이 발생하여도 advice 실행(advice 끝나면 무조건 수행)
aop:around : 메소드 실행 전/후 및 exception 발생 시 advice 실행(before와 after 합친거)
