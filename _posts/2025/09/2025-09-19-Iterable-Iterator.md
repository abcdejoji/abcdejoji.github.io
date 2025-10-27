---
title: "Iterable, Iterator"
categories: [ "Java & Spring" ]
tags: [ "[인프런] 김영한의 실전 자바 - 중급 2편" ]
date: "2025-09-19 20:40:00"
---

데이터를 순회할 때 사용하는 인터페이스

- `Iterable`: **“반복 가능한”** 이라는 뜻
- `Iterator`: **“반복자”** 라는 뜻

## 주요 메서드

```java
public interface Iterable<T> {

  Iterator<T> iterator();
}
```

- `iterator()`: Iterator 반복자 반환

```java
public interface Iterator<E> {

  boolean hasNext();

  E next();
}
```

- `hasNext()`: 다음 요소가 있는지 확인. 있으면 `True` 반환
- `next()`: 다음 요소를 반환

## 상속 구조

![](/assets/img/_posts/2025/09/2025-09-19-Iterable-Iterator/626851964660291.png)

- `Collection` 인터페이스를 상속받은 자료구조는 모두 `Iterable`과 `Iterable`을 통해 순회 가능
- `Map`은 `Collection`을 상속하지 않기 때문에 순회할 수 없으나, `keySet()`, `values()`, `entrySet()`을 통해 추출하여 순회 가능

## 직접 만들어 보기

```java
public class MyArrayIterator implements Iterator<Integer> {

  private int currentIndex = -1;
  private int[] targetArr;

  public MyArrayIterator(int[] targetArr) {
    this.targetArr = targetArr;
  }

  @Override
  public boolean hasNext() {
    return currentIndex < targetArr.length - 1;
  }

  @Override
  public Integer next() {
    return targetArr[++currentIndex];
  }
}
```

```java
public class MyArray implements Iterable<Integer> {

  private int[] numbers;

  public MyArray(int[] numbers) {
    this.numbers = numbers;
  }

  @Override
  public Iterator<Integer> iterator() {
    return new MyArrayIterator(numbers);
  }

}
```

```java
public class MyArrayMain {

  public static void main(String[] args) {
    MyArray myArray = new MyArray(new int[]{1, 2, 3, 4});

    Iterator<Integer> iterator = myArray.iterator();
    System.out.println("iterator 사용");
    while (iterator.hasNext()) {
      Integer value = iterator.next();
      System.out.println("value = " + value);
    }
  }
}
```

- `Iterable`을 구현한 객체에서 `Iterator`를 사용하여 순회

## 향상된 for문

자바에서는 `향상된 for문`이라는 문법을 제공합니다. `향상된 for문`을 통해 더욱 간결하게 코드 작성이 가능합니다.

```java
public class MyArrayMain {

  public static void main(String[] args) {
    MyArray myArray = new MyArray(new int[]{1, 2, 3, 4});

    Iterator<Integer> iterator = myArray.iterator();
    System.out.println("iterator 사용");
    while (iterator.hasNext()) {
      Integer value = iterator.next();
      System.out.println("value = " + value);
    }

    // 향상된 for문
    System.out.println("for-each 사용");
    for (int value : myArray) {
      System.out.println("value = " + value);
    }
  }
}
```

> `향상된 for`문은 `Iterable`을 구현한 객체여야 합니다.

> `향상된 for문`을 컴파일 하면 위 `while문`으로 변환 됩니다.

## 참고

- [https://www.inflearn.com/course/김영한의-실전-자바-중급-2](https://www.inflearn.com/course/%EA%B9%80%EC%98%81%ED%95%9C%EC%9D%98-%EC%8B%A4%EC%A0%84-%EC%9E%90%EB%B0%94-%EC%A4%91%EA%B8%89-2)
