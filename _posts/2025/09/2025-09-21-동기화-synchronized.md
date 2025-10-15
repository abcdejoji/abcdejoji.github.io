---
title: "동기화 - synchronized"
categories: [ "Java & Spring" ]
tags: [ "[인프런] 김영한의 실전 자바 - 고급 1편" ]
date: "2025-09-21 03:21:00"
---

## synchronized란?

동시성을 해결하기 위해 자바에서 제공하는 문법입니다.

## 출금 예제

아래 코드는 1000원이 있는 은행에서 800원씩 2번을 동시에 출금하는 로직입니다.

```java
public interface BankAccount {

  boolean withdraw(int amount);

  int getBalance();
}
```

```java
public class BankAccountV1 implements BankAccount {

  private int balance;

  public BankAccountV1(int initialBalance) {
    this.balance = initialBalance;
  }

  @Override
  public boolean withdraw(int amount) {
    log("거래 시작: " + getClass().getSimpleName());

    // 잔고가 출금액 보다 적으면, 진행하면 안됨
    log("[검증 시작] 출금액: " + amount + ", 잔액: " + balance);
    if (balance < amount) {
      log("[검증 실패] 출금액: " + amount + ", 잔액: " + balance);
      return false;
    }

    // 잔고가 출금액 보다 많으면, 진행
    log("[검증 완료] 출금액: " + amount + ", 잔액: " + balance);
    sleep(1000);
    balance -= amount;
    log("[출금 완료] 출금액: " + amount + ", 잔액: " + balance);

    log("거래 종료: " + getClass().getSimpleName());
    return true;
  }

  @Override
  public int getBalance() {
    return balance;
  }
}
```

```java
public class BankMain1 {

  public static void main(String[] args) throws InterruptedException {
    BankAccount account = new BankAccountV1(1000);

    Thread t1 = new Thread(new WithdrawTask(account, 800), "t1");
    Thread t2 = new Thread(new WithdrawTask(account, 800), "t2");

    t1.start();
    t2.start();

    // 검증 완료까지 잠시 대기
    sleep(500);
    log("t1 state: " + t1.getState());
    log("t2 state: " + t2.getState());

    t1.join();
    t2.join();

    log("최종 잔액: " + account.getBalance());
  }
}
```

예상 대로라면 처음엔 800원을 출금하고, 두 번째엔 출금에 실패해야 합니다.

### 예상과 다른 결과

```bash
2025-09-21 02:31:29.630 [       t1] 거래 시작: BankAccountV1
2025-09-21 02:31:29.630 [       t2] 거래 시작: BankAccountV1
2025-09-21 02:31:29.635 [       t1] [검증 시작] 출금액: 800, 잔액: 1000
2025-09-21 02:31:29.635 [       t1] [검증 완료] 출금액: 800, 잔액: 1000
2025-09-21 02:31:29.635 [       t2] [검증 시작] 출금액: 800, 잔액: 1000
2025-09-21 02:31:29.635 [       t2] [검증 완료] 출금액: 800, 잔액: 1000
2025-09-21 02:31:30.117 [     main] t1 state: TIMED_WAITING
2025-09-21 02:31:30.118 [     main] t2 state: TIMED_WAITING
2025-09-21 02:31:30.639 [       t1] [출금 완료] 출금액: 800, 잔액: 200
2025-09-21 02:31:30.639 [       t1] 거래 종료: BankAccountV1
2025-09-21 02:31:30.641 [       t2] [출금 완료] 출금액: 800, 잔액: -600
2025-09-21 02:31:30.641 [       t2] 거래 종료: BankAccountV1
2025-09-21 02:31:30.647 [     main] 최종 잔액: -600
```

분명 체크 로직을 작성했는데도 2번 다 출금이 되어버리고 심지어는 금액이 -600원이 되었습니다.

> 정말 동시에 실행되어 출금은 2번 되고 잔액은 200원이 될 수도 있습니다.

### 어째서 이런 현상이?

이 현상은 공유자원에 동시에 접근하면서 발생합니다.

![](/assets/img/posts/2025/09/2025-09-21-동기화-synchronized/20735317407250.png)

#### 출금 처리 과정

- `t1 스레드`가 `balance` 변수를 조회 (1000원)
- `t2 스레드`가 `balance` 변수를 조회 (1000원)
- `t1 스레드`의 검증 로직 실행 (통과)
- `t2 스레드`의 검증 로직 실행 (통과)
- 두 스레드 모두 `sleep()` 실행
- `t1 스레드`가 `balance` 값 수정 (1000원 - 800원 = 200원)
- `t2 스레드`가 `balance` 값 수정 (200원 - 800원 = -600원)
  - 정말 동시에 실행 됐다면 `t2 스레드`도 1000원 - 800원 = 200원

> 즉, `t1 스레드`에서 출금이 완료되기 전에 `t2 스레드`에서 잔액에 접근하는 것이 문제입니다.

### synchronized를 사용하여 동시 접근 막기

```java
public class BankAccountV2 implements BankAccount {

  private int balance;

  public BankAccountV2(int initialBalance) {
    this.balance = initialBalance;
  }

  // synchronized 추가
  @Override
  public synchronized boolean withdraw(int amount) {
    log("거래 시작: " + getClass().getSimpleName());

    // 잔고가 출금액 보다 적으면, 진행하면 안됨
    log("[검증 시작] 출금액: " + amount + ", 잔액: " + balance);
    if (balance < amount) {
      log("[검증 실패] 출금액: " + amount + ", 잔액: " + balance);
      return false;
    }

    // 잔고가 출금액 보다 많으면, 진행
    log("[검증 완료] 출금액: " + amount + ", 잔액: " + balance);
    sleep(1000);
    balance -= amount;
    log("[출금 완료] 출금액: " + amount + ", 잔액: " + balance);

    log("거래 종료: " + getClass().getSimpleName());
    return true;
  }

  @Override
  public synchronized int getBalance() {
    return balance;
  }
}
```

이렇게 메서드에 `synchronized`를 넣어주면 해당 메서드는 동시에 여러 스레드에서 접근할 수 없게 됩니다.

즉, `t1 스레드`에서 출금이 완료 된 후에야 `t2 스레드`가 실행됩니다.

### synchronized 코드 블럭

#### 임계 영역(critical section)

- 여러 스레드가 동시에 접근해서는 안되는 공유 자원을 접근하거나 수정하는 부분

위 로직에서 임계 영역은 아래 로직입니다.

```java
public synchronized boolean withdraw(int amount) {
  // 잔고가 출금액 보다 적으면, 진행하면 안됨
  log("[검증 시작] 출금액: " + amount + ", 잔액: " + balance);
  if (balance < amount) {
    log("[검증 실패] 출금액: " + amount + ", 잔액: " + balance);
    return false;
  }

  // 잔고가 출금액 보다 많으면, 진행
  log("[검증 완료] 출금액: " + amount + ", 잔액: " + balance);
  sleep(1000);
  balance -= amount;
  log("[출금 완료] 출금액: " + amount + ", 잔액: " + balance);
}
```

이 부분만 `synchronized`를 걸면 더 효율적으로 처리 할 수 있습니다.

```java
public class BankAccountV3 implements BankAccount {

  private int balance;

  public BankAccountV3(int initialBalance) {
    this.balance = initialBalance;
  }

  @Override
  public boolean withdraw(int amount) {
    log("거래 시작: " + getClass().getSimpleName());

    // synchronized 코드 블럭 추가
    synchronized (this) { // this는 락을 가져올 객체
      // 잔고가 출금액 보다 적으면, 진행하면 안됨
      log("[검증 시작] 출금액: " + amount + ", 잔액: " + balance);
      if (balance < amount) {
        log("[검증 실패] 출금액: " + amount + ", 잔액: " + balance);
        return false;
      }

      // 잔고가 출금액 보다 많으면, 진행
      log("[검증 완료] 출금액: " + amount + ", 잔액: " + balance);
      sleep(1000);
      balance -= amount;
      log("[출금 완료] 출금액: " + amount + ", 잔액: " + balance);
    }

    log("거래 종료: " + getClass().getSimpleName());
    return true;
  }

  @Override
  public synchronized int getBalance() {
    return balance;
  }
}

```

## synchronized 작동 원리

- 모든 인스턴스는 내부에 자신만의 락(Lock)을 가지고 있음
  - 모니터 락(monitor lock)이라고도 부름
- 스레드가 `synchronized` 키워드가 있는 메서드에 진입하려면 반드시 해당 인스턴스의 락이 있어야 함

> `t1 스레드`에서 `withdraw()` 실행 중이라면 `t2 스레드`에서는 해당 인스턴스의 락이 없기 때문에 `getBalance()` 접근도 못함

1. `t1 스레드`에서 출금 중이라면 `t2 스레드`는 `BLOCKED` 상태로 무한 대기를 함
2. `t1 스레드`가 출금을 끝내면 락을 반환하고 대기중이던 `t2 스레드`는 락을 획득하면서 `RUNNABLE` 상태로 바뀌고 출금을 실행함

> 락을 대기하는 스레드가 많을 경우 락 획득 순서는 보장되지 않습니다.

## 정리

### 장점

- 프로그래밍 언어에 문법으로 제공
- 아주 편리한 사용
- **자동 잠금 해제**: `synchronized` 메서드나 블록이 완료되면 자동으로 락을 대기중인 다른 스레드의 잠금 해제

### 단점

- **무한 대기**: `BLOCKED` 상태의 스레드는 락이 풀릴 때 까지 무한 대기
  - 특정 시간까지만 대기하는 타임아웃 X
  - 중간에 인터럽트 X
- **공정성**: 락이 돌아왔을 때 `BLOCKED` 상태의 여러 스레드 중에 어떤 스레드가 락을 획득할지 알 수 없음

## 참고

- [https://www.inflearn.com/course/김영한의-실전-자바-고급-1](https://www.inflearn.com/course/%EA%B9%80%EC%98%81%ED%95%9C%EC%9D%98-%EC%8B%A4%EC%A0%84-%EC%9E%90%EB%B0%94-%EA%B3%A0%EA%B8%89-1)
