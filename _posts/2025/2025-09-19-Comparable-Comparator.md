---
title: "Comparable, Comparator"
categories: [ "Java & Spring" ]
tags: [ "[인프런] 김영한의 실전 자바 - 중급 2편" ]
date: "2025-09-19 20:48:00"
---

데이터를 정렬할 때 사용하는 인터페이스

- `Comparable`: **“비교 할 수 있는”** 이라는 뜻
- `Comparator`: **“비교자”** 라는 뜻

## 주요 메서드

```java
public interface Comparable<T> {
  public int compareTo(T o);
}
```

```java
public interface Comparator<T> {
  int compare(T o1, T o2);
}
```

## 객체 정렬

- `Comparable` 를 상속받은 객체는 정렬이 가능합니다.

```java
public class MyUser implements Comparable<MyUser> {

  private String id;
  private int age;

  public MyUser(String id, int age) {
    this.id = id;
    this.age = age;
  }

  @Override
  public int compareTo(MyUser o) {
    return Integer.compare(this.age, o.age);
  }

  @Override
  public String toString() {
    return "MyUser{" +
      "id='" + id + '\'' +
      ", age=" + age +
      '}';
  }
}
```

```java
public class SortMain3 {

  public static void main(String[] args) {
    MyUser myUser1 = new MyUser("a", 30);
    MyUser myUser2 = new MyUser("b", 20);
    MyUser myUser3 = new MyUser("c", 10);

    MyUser[] array = {myUser1, myUser2, myUser3};
    System.out.println("기본 데이터");
    System.out.println(Arrays.toString(array));

    System.out.println("Comparable 기본 정렬");
    Arrays.sort(array);
    System.out.println(Arrays.toString(array));
  }
}
```

- 정렬 할 때 `Comparator`을 넘겨 정렬 할 수도 있습니다.

```java
public class IdComparator implements Comparator<MyUser> {

  @Override
  public int compare(MyUser o1, MyUser o2) {
    return o1.getId().compareTo(o2.getId());
  }
}
```

```java
public class SortMain3 {

  public static void main(String[] args) {
    MyUser myUser1 = new MyUser("a", 30);
    MyUser myUser2 = new MyUser("b", 20);
    MyUser myUser3 = new MyUser("c", 10);

    // 오름차순 정렬
    System.out.println("IdComparator 정렬");
    Arrays.sort(array, new IdComparator());
    System.out.println(Arrays.toString(array));

    // 내림차순 정렬
    System.out.println("IdComparator().reversed() 정렬");
    Arrays.sort(array, new IdComparator().reversed());
    System.out.println(Arrays.toString(array));
  }
}
```

> `List`는 `sort()` 메서드를 제공합니다.

> `TreeSet`에 객체를 담을 때에는 순서를 알 수 없으므로 `Comparator`를 생성자로 넘겨 정렬 순서를 알려주어야 합니다.

## 참고

- [https://www.inflearn.com/course/김영한의-실전-자바-중급-2](https://www.inflearn.com/course/%EA%B9%80%EC%98%81%ED%95%9C%EC%9D%98-%EC%8B%A4%EC%A0%84-%EC%9E%90%EB%B0%94-%EC%A4%91%EA%B8%89-2)
