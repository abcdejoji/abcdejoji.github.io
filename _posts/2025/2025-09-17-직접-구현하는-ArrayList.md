---
title: "직접 구현하는 ArrayList"
categories: [ "Java & Spring" ]
tags: [ "인프런 - 실전 자바 중급 2편", "김영한", "자료구조" ]
date: "2025-09-17 18:00:00"
---

# 배열

## 특징

### 인덱스

![](/assets/img/posts/2025/2025-09-17-직접-구현하는-ArrayList/502892016390958.png)

- 배열은 메모리상 순서대로 붙어서 존재
- `int`는 `4byte`를 차지
- 배열의 시작 위치부터 시작해서 자료의 크기와 인덱스 번호를 곱하여 원하는 메모리 위치를 찾음
- 따라서 데이터가 아무리 많아도 한 번의 연산으로 필요한 위치를 찾을 수 있음

### 데이터 추가 - 첫번 째 및 중간 위치에 추가

추가하는 위치로 부터 데이터를 오른쪽으로 한칸씩 밀어야 하므로 O(n) 만큼 걸립니다.

![](/assets/img/posts/2025/2025-09-17-직접-구현하는-ArrayList/502906242964333.png)
![](/assets/img/posts/2025/2025-09-17-직접-구현하는-ArrayList/502918150299250.png)

### 데이터 추가 - 마지막 위치에 추가

마지막 위치에 추가만 하면 되므로 O(1) 만큼 걸립니다.

![](/assets/img/posts/2025/2025-09-17-직접-구현하는-ArrayList/505158554691083.png)

## 단점

- 배열의 길이를 동적으로 변경할 수 없음
- 데이터를 추가하기 불편
  - 오른쪽으로 한 칸씩 데이터를 미는 로직을 작성해야 함

> 이런 문제를 해결 한 자료구조가 `List` 입니다.

# ArrayList

- **배열**: 순서가 있고 중복을 허용하지만 크기가 정적으로 고정
- **리스트**: 순서가 있고 중복을 허용하지만 크기가 동적으로 변할 수 있음

> `ArrayList`는 `List` 인터페이스의 구현체 중 하나입니다.

## 직접 구현

### 시작

```java
public class MyArrayListV1 {

  // 기본 배열이 크기
  private static final int DEFAULT_CAPACITY = 5;

  // 다양한 타입의 데이터를 보관하기 위해 Object 배열 사용
  private Object[] elementData;

  // 리스트 크기
  private int size = 0;

  public MyArrayListV1() {
    elementData = new Object[DEFAULT_CAPACITY];
  }

  public MyArrayListV1(int initialCapacity) {
    elementData = new Object[initialCapacity];
  }

  public int size() {
    return size;
  }

  // 리스트에 데이터 추가
  public void add(Object e) {
    elementData[size] = e;
    size++;
  }

  // 항목 조회
  public Object get(int index) {
    return elementData[index];
  }

  // 항목 변경
  public Object set(int index, Object element) {
    Object oldValue = get(index);
    elementData[index] = element;
    return oldValue;
  }

  // 인수와 같은 데이터가 있는 인덱스의 위치를 반환
  public int indexOf(Object o) {
    for (int i = 0; i < size; i++) {
      if (o.equals(elementData[i])) {
        return i;
      }
    }
    return -1;
  }

  @Override
  public String toString() {
    return Arrays.toString(Arrays.copyOf(elementData, size)) + " size=" + size + ", capacity=" + elementData.length;
  }

}
```

이 코드의 문제는 크기가 고정되어 있다는 점입니다. `DEFAULT_CAPACITY` 의 개수가 넘는 데이터가 들어오면 예외가 발생합니다.

### 동적 배열

```java
public class MyArrayListV2 {

  private static final int DEFAULT_CAPACITY = 5;

  private Object[] elementData;
  private int size = 0;

  public MyArrayListV2() {
    elementData = new Object[DEFAULT_CAPACITY];
  }

  public MyArrayListV2(int initialCapacity) {
    elementData = new Object[initialCapacity];
  }

  public int size() {
    return size;
  }

  public void add(Object e) {
    //추가
    if (size == elementData.length) {
      grow();
    }
    elementData[size] = e;
    size++;
  }

  //추가
  private void grow() {
    int oldCapacity = elementData.length;
    int newCapacity = oldCapacity * 2;
    elementData = Arrays.copyOf(elementData, newCapacity);
  }

  public Object get(int index) {
    return elementData[index];
  }

  public Object set(int index, Object element) {
    Object oldValue = get(index);
    elementData[index] = element;
    return oldValue;
  }

  public int indexOf(Object o) {
    for (int i = 0; i < size; i++) {
      if (o.equals(elementData[i])) {
        return i;
      }
    }
    return -1;
  }

  @Override
  public String toString() {
    return Arrays.toString(Arrays.copyOf(elementData, size)) + " size=" + size + ", capacity=" + elementData.length;
  }

}
```

- `grow` 메소드를 추가
- `add` 메서드 실행 시 크기를 비교하여 배열 사이즈가 부족하면 배열의 크기를 2배로 새롭게 생성 및 복사

### 기능 추가

```java
public class MyArrayListV3 {

  private static final int DEFAULT_CAPACITY = 5;

  private Object[] elementData;
  private int size = 0;

  public MyArrayListV3() {
    elementData = new Object[DEFAULT_CAPACITY];
  }

  public MyArrayListV3(int initialCapacity) {
    elementData = new Object[initialCapacity];
  }

  public int size() {
    return size;
  }

  public void add(Object e) {
    if (size == elementData.length) {
      grow();
    }
    elementData[size] = e;
    size++;
  }

  //코드 추가
  public void add(int index, Object e) {
    if (size == elementData.length) {
      grow();
    }
    shiftRightFrom(index);
    elementData[index] = e;
    size++;
  }

  private void grow() {
    int oldCapacity = elementData.length;
    int newCapacity = oldCapacity * 2;
    elementData = Arrays.copyOf(elementData, newCapacity);
  }

  //코드 추가, 요소의 마지막부터 index까지 오른쪽으로 밀기
  private void shiftRightFrom(int index) {
    for (int i = size; i > index; i--) {
      elementData[i] = elementData[i - 1];
    }
  }

  public Object get(int index) {
    return elementData[index];
  }

  public Object set(int index, Object element) {
    Object oldValue = get(index);
    elementData[index] = element;
    return oldValue;
  }

  //코드 추가
  public Object remove(int index) {
    Object oldValue = get(index);
    shiftLeftFrom(index);

    size--;
    elementData[size] = null;
    return oldValue;
  }

  //코드 추가, 요소의 index부터 마지막까지 왼쪽으로 밀기
  private void shiftLeftFrom(int index) {
    for (int i = index; i < size - 1; i++) {
      elementData[i] = elementData[i + 1];
    }
  }

  public int indexOf(Object o) {
    for (int i = 0; i < size; i++) {
      if (o.equals(elementData[i])) {
        return i;
      }
    }
    return -1;
  }

  @Override
  public String toString() {
    return Arrays.toString(Arrays.copyOf(elementData, size)) + " size=" + size + ", capacity=" + elementData.length;
  }

}
```

특정 인덱스에 요소를 추가 및 삭제 할 수 있는 기능을 추가하였습니다.

현재는 모든 데이터를 Object 타입을 사용하여 처리하고 있습니다. 따라서 사용하는 시점에 다운 캐스팅을 해줘야 하는데 이는 타입 안전성을 깨트립니다.

이런 문제를 제네릭을 통해 해결 할 수 있습니다.

### 제네릭을 사용하여 타입 안전성 확보

```java
public class MyArrayListV4<E> {

  private static final int DEFAULT_CAPACITY = 5;

  private Object[] elementData;
  private int size = 0;

  // new E[]은 불가능 하기 떄문에 여전히 Object 타입을 사용
  // 조회 시, 제네릭 타입으로 다운 캐스팅하여 반환
  public MyArrayListV4() {
    elementData = new Object[DEFAULT_CAPACITY];
  }

  public MyArrayListV4(int initialCapacity) {
    elementData = new Object[initialCapacity];
  }

  public int size() {
    return size;
  }

  public void add(E e) {
    if (size == elementData.length) {
      grow();
    }
    elementData[size] = e;
    size++;
  }

  public void add(int index, E e) {
    if (size == elementData.length) {
      grow();
    }
    shiftRightFrom(index);
    elementData[index] = e;
    size++;
  }

  private void shiftRightFrom(int index) {
    for (int i = size; i > index; i--) {
      elementData[i] = elementData[i - 1];
    }
  }

  // 제네릭을 통해 이미 타입을 확보 했기 때문에 다운 캐스팅 시 안전
  @SuppressWarnings("unchecked")
  public E get(int index) {
    return (E) elementData[index];
  }

  public E set(int index, E element) {
    E oldValue = get(index);
    elementData[index] = element;
    return oldValue;
  }

  public E remove(int index) {
    E oldValue = get(index);
    shiftLeftFrom(index);

    size--;
    elementData[size] = null;
    return oldValue;
  }

  private void shiftLeftFrom(int index) {
    for (int i = index; i < size - 1; i++) {
      elementData[i] = elementData[i + 1];
    }
  }

  public int indexOf(E o) {
    for (int i = 0; i < size; i++) {
      if (o.equals(elementData[i])) {
        return i;
      }
    }
    return -1;
  }

  private void grow() {
    int oldCapacity = elementData.length;
    int newCapacity = oldCapacity * 2;
    elementData = Arrays.copyOf(elementData, newCapacity);
  }

  @Override
  public String toString() {
    return Arrays.toString(Arrays.copyOf(elementData, size)) + " size=" + size + ", capacity=" + elementData.length;
  }

}
```

## ArrayList 빅오 정리

- 데이터 추가 및 삭제
  - 마지막에 추가: O(1)
  - 앞, 중간에 추가: O(n)
- 인덱스 조회: O(1)
- 데이터 검색: O(n)

## 단점

- 배열을 사용하므로 배열 뒷 부분에 사용되지 않고 낭비되는 메모리 존재
- 데이터를 중간에 추가하거나 삭제할 때 비효율적

> 데이터를 앞이나 중간에 추가하거나 삭제할 때 비효율적인 단점을 해결한 자료구조가 `LinkedList`

# 참고

- [https://www.inflearn.com/course/김영한의-실전-자바-중급-2](https://www.inflearn.com/course/%EA%B9%80%EC%98%81%ED%95%9C%EC%9D%98-%EC%8B%A4%EC%A0%84-%EC%9E%90%EB%B0%94-%EC%A4%91%EA%B8%89-2)
