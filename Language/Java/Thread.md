
Java에서 Thread를 구현하는 방법 - 두 가지 방법 모두 핵심은 run()
  1. Thread 상속
  2. Runnable 인터페이스 구현





Thread
쓰레드에 대해 start는 한번만 호출이 가능(한번 start가 된 Tread는 재사용이 불가)

Runnable

Java쓰레드 실행 과정
1. main 메서드 에서 쓰레드의 start를 호출
2. start 메서드에서 쓰레드가 작업 수행에 필요한 새로운 호출 스택을 생성
3. 생성된 호출 스택에 run메서드를 호출해서 쓰레드 작업을 수행
4. 스케쥴러에 의해 쓰레드가 번갈아 가면서 실행
