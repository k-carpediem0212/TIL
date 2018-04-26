# Transaction
> 데이터베이스의 상태를 변화시키기 위해 수행하는 작업의 단위
> 데이터베이스 내에서 한꺼번에 수행되어야하는 일련의 연산들

- 특징
  + Atomicity (원자성)
      * 트랜잭션이 데이터베이스에 모두 반영되거나, 전혀 반영되지 않아야한다.
  + Consistency (일관성)
      * 트랜잭션의 작업 처리 결과가 항상 일관성 있어야 한다.
  + Isolation (독립성)
      * 둘 이상의 트랜잭션이 실행 시, 한 트랜잭션이 다른 트랜잭션 연산에 끼어들 수 없다.
  + Durability (지속성)
      * 트랜잭션이 성공적으로 완료되었을 경우, 결과는 영구적으로 반영되야한다.

- 트랜잭션 연산
  + COMMIT
      * 하나의 트랜잭션이 성공적으로 끝난 상태를 알리는 연산
      * 수행했던 트랜잭션이 로그에 적재
      * 이 후에 ROLLBACK 연산을 수행했던 트랜잭션 단위로 할 수 있도록 도와줌
  + ROLLBACK
      * 하나의 트랜잭션 처리가 비정상적으로 종료되어 원자성이 깨진 경우, 트랜잭션을 처음부터 다시 시작하거나, 트랜잭션의 부분적으로만 연산된 결과를 다시 취소시킨다.
      * 이후에 사용자가 트랜잭션이 처리된 단위로 ROLLBACK도 가능하다.