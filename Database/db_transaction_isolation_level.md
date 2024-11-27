# 트랜잭션 격리 레벨

## 트랜잭션의 격리 수준 (Isolation Level)

: 여러 트랜잭션이 동시에 처리될 때, 특정 트랜잭션이 다른 트랜잭션에서 변경하거나 조회하는 데이터를 볼 수 있게 허용할 지 여부를 결정하는 것

ACID 중 Isolation(독립성, 고립성)을 구현하는 개념으로,
동시에 여러 트랜잭션이 처리될 때 트랜잭션끼리 얼마나 서로 고립되어 있는 지를 나타내는 것

### Serializable

- 가장 엄격한 격리 수준
- 이름 그대로 트랜잭션을 순차적으로 진행시킨다
- 동일한 레코드에 동시 접근 불가능, 어떠한 데이터 부정합 문제 발생 X, But. 동시처리 성능이 매우 떨어진다
- 순수한 SELECT 작업에서도 대상 레코드에 넥스트 키 락을 읽기 잠금(공유락, Shared Lock)으로 건다
- 한 트랜잭션에서 넥스트 키 락이 걸린 레코드를 다른 트랜잭션에서는 절대 추가/수정/삭제 할 수 없다
- 안전하지만 가장 성능이 떨어지므로, 극단적으로 안전한 작업이 필요한 경우가 아니라면 사용 X

### Repeatable Read

![image.png](/Database/img/transaction_level_1.png)

사용자 B가 테이블에서 트랜잭션을 시작하고, SELECT문을 실행시켰다. 그리고 사용자 A가 트랜잭션을 시작하고 테이블을 수정하고서, 아직 끝나지 않은 사용자 B의 트랜잭션에서 다시 SELECT문을 실행시키면 Undo 로그에 있던 값을 가져오게 된다

A의 트랜잭션이 시작되고 커밋까지 됐지만, 해당 트랜잭션(12)은 현재 트랜잭션(10)보다 나중에 실행되었기 때문에 조회 결과로 기존과 동일한 데이터를 얻게된다

→ Repeatable Read는 어떤 트랜잭션이 읽은 데이터를 다른 트랜잭션이 수정하더라도 동일한 결과를 반환할 것을 보장해준다 - 한 트랜잭션이 시작됐을 때는 다른 트랜잭션이 시작되더라도 한 트랜잭션보다 먼저 실행됐던 트랜잭션의 데이터만 반영한다(새로운 레코드의 추가는 막지 않는다)

![image.png](/Database/img/transaction_level_2.png)

유령 읽기(Phantom Read) : 트랜잭션이 끝나기 전에 다른 트랜잭션에 의해 추가된 레코드가 발견될 경우

→ 이전의 SELECT문에서는 MVVC때문에 Undo 로그가 존재하여 유령 읽기 문제가 발생하지 않는다

하지만, 잠금을 사용할 경우 유령 읽기 문제가 발생할 수 있다. SELECT … FOR UPDATE구문은 배타적 잠금(비관적 잠금, 쓰기 잠금)을 건다. **잠금을 걸 경우에는 데이터 조회가 Undo 로그를 참고하는 것이 아니고 테이블을 참고**하므로, 다른 트랜잭션에서 수행한 작업에 의해 레코드가 보였다 안보였다 하는 현상을 유령 읽기 라고 한다.

MySQL에서는 갭 락이 존재하기 때문에 위의 상황에서는 문제가 발생하지 않는다

<MySQL 기준>

- SELECT FOR UPDATE 이후 SELECT: 갭락 때문에 팬텀리드 X
- SELECT FOR UPDATE 이후 SELECT FOR UPDATE: 갭락 때문에 팬텀리드 X
- SELECT 이후 SELECT: MVCC 때문에 팬텀리드 X
- SELECT 이후 SELECT FOR UPDATE: 팬텀 리드 O

### Read Committed

: 커밋된 데이터만 조회 가능. 유령 읽기(Phantom Read) + 반복 읽기 불가능(Non-Repeatable Read)문제가 발생 가능

![image.png](/Database/img/transaction_level_3.png)

위의 그림은 아직 사용자 A 트랜잭션이 커밋되지 않아 사용자 B 트랜잭션이 Undo 로그에서 가져와 데이터를 읽는 것을 알 수 있다

![image.png](/Database/img/transaction_level_4.png)

Read Committed에서 반복 읽기를 수행하면 다른 트랜잭션의 커밋 여부에 따라 조회 결과가 달라질 수 있다

→ 데이터 부정합 문제 : Non-Repeatable Read(반복 읽기 불가능)

일반적인 경우에는 크게 문제가 되지 않지만, 금전적인 처리와 연결되면 문제가 발생할 수 있다
ex) A 트랜잭션에서는 오늘 입금된 총 합을 계산, B 트랜잭션에서는 계속해서 입금 내역을 커밋
⇒ Read Committed에서는 같은 트랜잭션일지라도 조회할 때마다 입금된 내역이 달라지므로 문제가 생길 수 있다.

### Read Uncommitted

: 커밋하지 않은 데이터 조차도 접근할 수 있는 격리 수준.

![image.png](/Database/img/transaction_level_5.png)

Dirty Read(오손 읽기) : 어떤 트랜잭션의 작업이 완료되지 않았는데도, 다른 트랜잭션에서 볼 수 있는 부정합 문제

이러한 Dirty Read 상황은 시스템에 상당한 버그를 초래할 것 이다.

→ RDBMS 표준에서 인정하지 않을 정도로 정합성에 문제가 많은 격리 수준이다. 따라서 MySQL을 사용하면 최소한 Read Committed이상의 격리 수준을 사용해야한다.

### 정리 및 요약

| **격리 수준**        | **특징**                                                           | **발생 가능한 문제**                          |
| -------------------- | ------------------------------------------------------------------ | --------------------------------------------- |
| **Read Uncommitted** | 다른 트랜잭션이 커밋하지 않은 데이터를 읽을 수 있음.               | Dirty Read, Non-Repeatable Read, Phantom Read |
| **Read Committed**   | 커밋된 데이터만 읽을 수 있음.                                      | Non-Repeatable Read, Phantom Read             |
| **Repeatable Read**  | 같은 트랜잭션 내에서 같은 데이터를 읽으면 항상 동일한 값을 반환.   | Phantom Read                                  |
| **Serializable**     | 트랜잭션이 직렬적으로 실행된 것처럼 동작하여 완벽한 일관성을 보장. | 없음 (하지만 성능 저하)                       |

## 출처

https://mangkyu.tistory.com/299

## 예상 질문

1. 트랜잭션 격리 레벨에는 어떤 종류가 있고, 간략한 설명
2. Read Committed가 사용될 경우 위험한 경우 예시 한 가지
3. 각각 격리 수준에서 발생할 수 있는 문제점
