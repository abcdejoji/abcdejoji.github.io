---
title: "default method"
categories: [ "Java & Spring" ]
tags: [ "[인프런] 김영한의 실전 자바 - 고급 3편" ]
date: "2025-09-30 23:39:00"
---

## 등장 배경

- 기존 인터페이스에 새로운 메서드를 추가하면 이미 배포된 구현체들이 전부 컴파일 에러가 나는 문제가 발생
- 디폴트 메서드를 통해 인터페이스에 기본 구현을 제공함으로써, 하위 호환성을 꺠뜨리지 않고 기능을 확장 가능

## 사용 예시

```java
public interface MyInterface {
  void existingMethod();

  default void newMethod() {
    System.out.println("새로 추가된 디폴트 메서드입니다.");
  }
}
```

- `default` 키워드를 통해 메서드 몸통을 인터페이스에 직접 정의
- 필요한 경우 구현 클래스에서 재정의하면 되고, 재정의 하지 않으면 인터페이스의 기본 구현 사용

## 장점

- 기존 코드를 수정하지 않고도 인터페이스에 새 기능을 추가 가능

## 주의 사항

- 하위 호환성을 위한 최소환의 활용
- 인터페이스 본연의 추상화 목적 유지
- 다중 상속(충돌) 문제
- 상태(state)를 두지 말 것

## 참고

- [https://www.inflearn.com/course/김영한의-실전-자바-고급-3](https://www.inflearn.com/course/%EA%B9%80%EC%98%81%ED%95%9C%EC%9D%98-%EC%8B%A4%EC%A0%84-%EC%9E%90%EB%B0%94-%EA%B3%A0%EA%B8%89-3)
