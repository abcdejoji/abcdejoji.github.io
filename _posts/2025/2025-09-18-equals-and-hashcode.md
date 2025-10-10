---
title: "equals and hashCode"
categories: [ "Java & Spring" ]
tags: [ "[인프런] 김영한의 실전 자바 - 중급 2편" ]
date: "2025-09-18 22:00:00"
---

## 문자열 해시코드

문자열을 `index` 에 넣을 수 없으므로 숫자로 변환을 해주어야 합니다. 문자열은 문자로 이루어져있고 문자는 아스키 코드를 통해 숫자로 변환할 수 있습니다.

### 예시 코드

```java
public class StringHashMain {

  static final int CAPATICY = 10;

  public static void main(String[] args) {
    //char
    char charA = 'A';
    char charB = 'B';
    System.out.println(charA + " = " + (int) charA);
    System.out.println(charB + " = " + (int) charB);

    //hash
    System.out.println("hash(A) = " + hashCode("A"));
    System.out.println("hash(B) = " + hashCode("B"));
    System.out.println("hash(AB) = " + hashCode("AB"));

    //hashIndex
    System.out.println("hashIndex(A) = " + hashIndex(hashCode("A")));
    System.out.println("hashIndex(B) = " + hashIndex(hashCode("B")));
    System.out.println("hashIndex(AB) = " + hashIndex(hashCode("AB")));
  }

  static int hashCode(String str) {
    char[] charArray = str.toCharArray();
    int sum = 0;
    for (char c : charArray) {
      sum += c;
    }
    return sum;
  }

  static int hashIndex(int value) {
    return value % CAPATICY;
  }
}
```

이렇게 해시 코드와 해시 인덱스를 얻어 똑같이 적용할 수 있습니다.

## 자바의 hashCode()

자바의 `Object`는 `hashCode()` 메서드를 제공합니다.

- 보통 오버라이딩 해서 사용
- 기본 구현은 객체의 참조 값을 기반으로 해시 코드 생성
- 자바에서 제공하는 `hashCode`는 단순 아스키 코드 값을 더하는 게 아닌 더 복잡한 로직으로 산출 됩니다.

> 보통 IntelliJ 같은 IDE에서 제공하는 기능으로 `equals`와 `hashCode`를 오버라이딩 하여 사용

## equals, hashCode 중요성

`hash`를 사용하는 컬렉션을 사용할 때, 반드시 `equals`와 `hashCode` 모두 오버라이딩하여 사용해야 합니다.

`hashCode`만 오버라이딩하여 사용 시, 해시 충돌로 인해 같은 인덱스에 들어가 있는 데이터를 순차 검색 할 때 다른 인스턴스라면 `equals`가 다르므로 `false`를 반환하게 됩니다.

따라서 반드시 `equals`와 `hashCode` 모두 오버라이딩 하여 사용해야 합니다.

## 출처

- [https://www.inflearn.com/course/김영한의-실전-자바-중급-2](https://www.inflearn.com/course/%EA%B9%80%EC%98%81%ED%95%9C%EC%9D%98-%EC%8B%A4%EC%A0%84-%EC%9E%90%EB%B0%94-%EC%A4%91%EA%B8%89-2)
