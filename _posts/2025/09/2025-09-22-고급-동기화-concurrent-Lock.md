---
title: "고급 동기화 - concurrent.Lock"
categories: [ "Java & Spring" ]
tags: [ "[인프런] 김영한의 실전 자바 - 고급 1편" ]
date: "2025-09-22 00:50:00"
---

## synchronized 단점

- **무한 대기**: `BLOCKED` 상태의 스레드는 락이 풀릴 때 까지 무한 대기
- **공정성**: 락 획득을 대기 중인 스레드 중 어떤 스레드가 락을 획득할 지 알 수 없음

위 두 문제를 해결 한 것이 `Lock` 입니다.

## LockSupport

`LockSupport`를 사용하면 무한 대기 상태를 해결할 수 있습니다.

### 기능

- `park()`: 스레드를 `WAITING` 상태로 변경
- `parkNanos(nanos)`: 스레드를 나노초 동안만 `TIMED_WAITING` 상태로 변경
- `unpark()`: `WAITING` → `RUNNABLE` 상태로 변경

> - `BLOCKED` : 인터럽트가 들어와도 무시하고 무한 대기
> - `WAITING` : 인터럽트가 들어오면 적용

## ReentrantLock

- `Lock` 인터페이스의 대표 구현체 중 하나
- `LockSupport`를 사용하여 `synchronized`의 단점을 해결

### 무한 대기 문제 해결

#### Lock 인터페이스

```java
public interface Lock {
  void lock();

  void lockInterruptibly() throws InterruptedException;

  boolean tryLock();

  boolean tryLock(long time, TimeUnit unit) throws InterruptedException;

  void unlock();

  Condition newCondition();
}
```

- `lock()`
  - 락을 획득
  - 다른 스레드가 이미 락을 획득 했다면 풀릴 때 까지 `WAITING` 상태로 대기
  - 대기 중 인터럽트 응답 X
  - 즉, 무한 대기 상태
- `lockInterruptibly()`
  - 락을 획득
  - 대기 중 인터럽트 응답 O
- `tryLock()`
  - 락 획득을 시도하고, 즉시 성공 여부 반환
- `tryLock(long time, TimeUnit unit)`
  - 주어진 시간 동안 락 획득 시도
  - 대기 중 인터럽트 응답 O
- `unlock()`
  - 락 해제
  - 락을 획득한 스레드가 `finally`를 통해 반드시 호출
- `newCondition()`
  - `Condition` 객체를 생성하여 반환
  - `Condition` 객체는 락과 결합하여 사용
  - 스레드가 특정 조건을 기다리거나 신호를 받을 수 있도록 함

> `lock()` 을 제외한 락 획득 메서드는 모두 인터럽트에 응답 하므로 무한 대기 상태에서 빠져나올수 있습니다.

> 여기서 사용하는 락은 모니터 락이 아닌 객체 내부의 락입니다.

### 공정성 문제 해결

```java
// 비공정 모드 락
private final Lock nonFairLock = new ReentrantLock();

// 공정 모드 락
private final Lock fairLock = new ReentrantLock(true);
```

`ReentrantLock` 객체를 생성할 때 인자 값으로 `true`를 넘기면 공정 모드로 생성됩니다.

#### 비공정 모드

- **성능 우선**: 락을 획득하는 속도가 빠름
- **선점 가능**: 새로운 스레드가 기존 대기 스레드 보다 먼저 락을 획득할 수 있음
- **기아 현상 가능성**: 특정 스레드가 계속해서 락을 획득 못할 수 있음

#### 공정 모드

- **성능 저하**: 락을 획득하는 속도가 느림
- **공정성 보장**: 대기 큐에서 먼저 대기한 스레드가 락을 먼저 획득
- **기아 현상 방지**: 모든 스레드가 언젠가 락을 획득할 수 있게 보장

### 사용 예시

```java
public class BankAccountV4 implements BankAccount {

  private int balance;

  private final Lock lock = new ReentrantLock();

  public BankAccountV4(int initialBalance) {
    this.balance = initialBalance;
  }

  @Override
  public boolean withdraw(int amount) {
    log("거래 시작: " + getClass().getSimpleName());

    try {
      lock.lock(); // lock 걸기

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
    } finally {
      lock.unlock(); // lock 해제
    }

    log("거래 종료: " + getClass().getSimpleName());
    return true;
  }

  @Override
  public int getBalance() {
    try {
      lock.lock(); // lock 걸기
      return balance;
    } finally {
      lock.unlock(); // lock 해제
    }
  }
}

```

## 참고

- [https://www.inflearn.com/course/김영한의-실전-자바-고급-1](https://www.inflearn.com/course/%EA%B9%80%EC%98%81%ED%95%9C%EC%9D%98-%EC%8B%A4%EC%A0%84-%EC%9E%90%EB%B0%94-%EA%B3%A0%EA%B8%89-1)
