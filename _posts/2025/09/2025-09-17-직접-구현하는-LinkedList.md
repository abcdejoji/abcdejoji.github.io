---
title: "직접 구현하는 LinkedList"
categories: [ "Java & Spring" ]
tags: [ "[인프런] 김영한의 실전 자바 - 중급 2편", "자료구조" ]
date: "2025-09-17 22:00:00"
---

## ArrayList의 단점

- 배열을 사용하므로 배열 뒷 부분에 사용되지 않고 낭비되는 메모리 존재
- 데이터를 중간에 추가하거나 삭제할 때 비효율적

이를 해결하고자 나온 것이 `LinkedList` 입니다.

## 노드와 연결

낭비되는 메모리 없이 필요한 만큼만 메모리를 확보해서 사용하고, 앞이나 중간에 데이터를 추가하거나 삭제할 때도 효율적인 자료 구조가 노드와 노드를 연결하는 방식입니다.

```java
public class Node {

  Object item;
  Node next;

  public Node(Object item) {
    this.item = item;
  }

  @Override
  public String toString() {
    StringBuilder sb = new StringBuilder();
    Node x = this;
    sb.append("[");
    while (x != null) {
      sb.append(x.item);
      if (x.next != null) {
        sb.append("->");
      }
      x = x.next;
    }
    sb.append("]");
    return sb.toString();
  }
}
```

![](/assets/img/posts/2025/09/2025-09-17-직접-구현하는-LinkedList/504375244426000.png)

```java
public class NodeMain1 {

  public static void main(String[] args) {
    //노드 생성하고 연결하기: A -> B -> C
    Node first = new Node("A");
    first.next = new Node("B");
    first.next.next = new Node("C");

    System.out.println("모든 노트 탐색하기");
    Node x = first;
    while (x != null) {
      System.out.println(x.item);
      x = x.next;
    }
  }
}
```

```bash
모든 노트 탐색하기
A
B
C
```

### 조회 및 추가

```java
public class NodeMain3 {

  public static void main(String[] args) {
    //노드 생성하고 연결하기: A -> B -> C
    Node first = new Node("A");
    first.next = new Node("B");
    first.next.next = new Node("C");
    //A -> B -> C

    System.out.println(first);

    //모든 노드 탐색하기
    System.out.println("모든 노트 탐색하기");
    printAll(first);

    //마지막 노드 조회하기
    Node lastNode = getLastNode(first);
    System.out.println("lastNode = " + lastNode);

    //특정 index의 노드 조회하기
    int index = 2;
    Node index2Node = getNode(first, index);
    System.out.println("index2Node = " + index2Node.item);

    //데이터 추가하기
    System.out.println("데이터 추가하기");
    add(first, "D");
    System.out.println(first);
    add(first, "E");
    System.out.println(first);
    add(first, "F");
    System.out.println(first);
  }

  private static void printAll(Node node) {
    Node x = node;
    while (x != null) {
      System.out.println(x.item);
      x = x.next;
    }
  }

  private static void add(Node node, String param) {
    Node lastNode = getLastNode(node);
    lastNode.next = new Node(param);
  }

  private static Node getLastNode(Node node) {
    Node x = node;
    while (x.next != null) {
      x = x.next;
    }
    return x;
  }

  private static Node getNode(Node node, int index) {
    Node x = node;
    for (int i = 0; i < index; i++) {
      x = x.next;
    }
    return x;
  }
}
```

## LinkedList - 단일 연결 리스트

> 자바는 이중 연결 리스트로 되어 있으나 여기 예제는 단일 연결 리스트 입니다.

### 직접 구현

#### 시작

```java
public class MyLinkedListV1 {

  private Node first;
  private int size = 0;

  // 마지막에 데이터 추가
  public void add(Object e) {
    Node newNode = new Node(e);
    if (first == null) {
      first = newNode;
    } else {
      Node lastNode = getLastNode();
      lastNode.next = newNode;
    }
    size++;
  }

  // 마지막 노드 찾기
  private Node getLastNode() {
    Node x = first;
    while (x.next != null) {
      x = x.next;
    }
    return x;
  }

  // 특정 위치에 있는 데이터 변경
  public Object set(int index, Object element) {
    Node x = getNode(index);
    Object oldValue = x.item;
    x.item = element;
    return oldValue;
  }

  // 특정 위치에 있는 데이터 반환
  public Object get(int index) {
    Node node = getNode(index);
    return node.item;
  }

  // 특정 위치에 있는 노드 반환
  private Node getNode(int index) {
    Node x = first;
    for (int i = 0; i < index; i++) {
      x = x.next;
    }
    return x;
  }

  // 데이터 검색 및 검색된 위치 반환
  public int indexOf(Object o) {
    int index = 0;
    for (Node x = first; x != null; x = x.next) {
      if (o.equals(x.item))
        return index;
      index++;
    }
    return -1;
  }

  public int size() {
    return size;
  }

  @Override
  public String toString() {
    return "MyLinkedListV1{" +
      "first=" + first +
      ", size=" + size +
      '}';
  }
}
```

#### 데이터 추가

- 첫 번째 위치에 데이터 추가
  ![](/assets/img/posts/2025/09/2025-09-17-직접-구현하는-LinkedList/504530768373875.png)
  ![](/assets/img/posts/2025/09/2025-09-17-직접-구현하는-LinkedList/504539556478125.png)
  ![](/assets/img/posts/2025/09/2025-09-17-직접-구현하는-LinkedList/504550662351166.png)
  ![](/assets/img/posts/2025/09/2025-09-17-직접-구현하는-LinkedList/504555543289000.png)
  - 배열의 경우 첫 번쨰 항목에 데이터가 추가되면 모든 데이터를 오른쪽으로 하나씩 밀어야 하지만 연결 리스트는 새로 생성한 노드의 참조만 변경하면 끝
  - 즉, O(1)의 성능

- 중간 위치에 데이터 추가
  ![](/assets/img/posts/2025/09/2025-09-17-직접-구현하는-LinkedList/504593585966250.png)
  ![](/assets/img/posts/2025/09/2025-09-17-직접-구현하는-LinkedList/504602788054416.png)
  ![](/assets/img/posts/2025/09/2025-09-17-직접-구현하는-LinkedList/504609371538291.png)
  ![](/assets/img/posts/2025/09/2025-09-17-직접-구현하는-LinkedList/504616998882416.png)
  - 배열의 경우 데이터가 추가되면 인덱스 위치부터 모든 데이터를 오른쪽으로 하나씩 밀어야 하지만 연결 리스트는 새로 생성한 노드의 참조만 변경하면 끝
  - O(n)의 성능
    - 변경은 O(1)
    - 해당 노드를 찾는데 O(n)

```java
public void add(int index, Object e) {
  Node newNode = new Node(e);
  if (index == 0) { // 첫 번째 위치의 데이터 추가
    newNode.next = first;
    first = newNode;
  } else { // 중간 위치에 데이터 추가
    Node prev = getNode(index - 1);
    newNode.next = prev.next;
    prev.next = newNode;
  }
  size++;
}
```

#### 데이터 삭제

- 첫 번째 위치의 데이터 삭제
  ![](/assets/img/posts/2025/09/2025-09-17-직접-구현하는-LinkedList/504697389439500.png)
  ![](/assets/img/posts/2025/09/2025-09-17-직접-구현하는-LinkedList/504703053497708.png)
  ![](/assets/img/posts/2025/09/2025-09-17-직접-구현하는-LinkedList/504709536373125.png)
  ![](/assets/img/posts/2025/09/2025-09-17-직접-구현하는-LinkedList/504719315663708.png)
  - 배열의 경우 첫 번쨰 항목에 데이터가 추가되면 모든 데이터를 왼쪽으로 하나씩 밀어야 하지만 연결 리스트는 새로 생성한 노드의 참조만 변경하면 끝
  - 즉, O(1)의 성능

- 중간 위치의 데이터 삭제
  ![](/assets/img/posts/2025/09/2025-09-17-직접-구현하는-LinkedList/504751565845375.png)
  ![](/assets/img/posts/2025/09/2025-09-17-직접-구현하는-LinkedList/504756788728416.png)
  ![](/assets/img/posts/2025/09/2025-09-17-직접-구현하는-LinkedList/504764604027083.png)
  ![](/assets/img/posts/2025/09/2025-09-17-직접-구현하는-LinkedList/504769428602875.png)
  - 배열의 경우 데이터가 추가되면 인덱스 위치부터 모든 데이터를 왼쪽으로 하나씩 밀어야 하지만 연결 리스트는 새로 생성한 노드의 참조만 변경하면 끝
  - O(n)의 성능
    - 삭제는 O(1)
    - 해당 노드를 찾는데 O(n)

```java
public Object remove(int index) {
  Node removeNode = getNode(index);
  Object removedItem = removeNode.item;
  if (index == 0) { // 첫 번째 위치의 데이터 삭제
    first = removeNode.next;
  } else { // 중간 위치의 데이터 삭제
    Node prev = getNode(index - 1);
    prev.next = removeNode.next;
  }
  removeNode.item = null;
  removeNode.next = null;
  size--;
  return removedItem;
}
```

- **배열 리스트**: 삭제할 위치는 O(1)으로 빠르게 찾지만, 데이터를 한 칸씩 밀어야 하는 부분이 O(n)으로 오래 걸림
- **연결 리스트**: 삭제할 위치는 O(n)으로 느리게 찾지만, 노드의 참조 값만 변경하면 되므로 O(1)으로 빠름

### 제네릭을 사용하여 타입 안전성 확보

```java
public class MyLinkedListV3<E> {
  private Node<E> first;
  private int size = 0;

  public void add(E e) {
    Node<E> newNode = new Node<>(e);
    if (first == null) {
      first = newNode;
    } else {
      Node<E> lastNode = getLastNode();
      lastNode.next = newNode;
    }
    size++;
  }

  private Node<E> getLastNode() {
    Node<E> x = first;
    while (x.next != null) {
      x = x.next;
    }
    return x;
  }

  public void add(int index, E e) {
    Node<E> newNode = new Node<>(e);
    if (index == 0) {
      newNode.next = first;
      first = newNode;
    } else {
      Node<E> prev = getNode(index - 1);
      newNode.next = prev.next;
      prev.next = newNode;
    }
    size++;
  }

  public E set(int index, E element) {
    Node<E> x = getNode(index);
    E oldValue = x.item;
    x.item = element;
    return oldValue;
  }

  public E remove(int index) {
    Node<E> removeNode = getNode(index);
    E removedItem = removeNode.item;
    if (index == 0) {
      first = removeNode.next;
    } else {
      Node<E> prev = getNode(index - 1);
      prev.next = removeNode.next;
    }
    removeNode.item = null;
    removeNode.next = null;
    size--;
    return removedItem;
  }

  public E get(int index) {
    Node<E> node = getNode(index);
    return node.item;
  }

  private Node<E> getNode(int index) {
    Node<E> x = first;
    for (int i = 0; i < index; i++) {
      x = x.next;
    }
    return x;
  }

  public int indexOf(E o) {
    int index = 0;
    for (Node<E> x = first; x != null; x = x.next) {
      if (o.equals(x.item))
        return index;
      index++;
    }
    return -1;
  }

  public int size() {
    return size;
  }

  @Override
  public String toString() {
    return "MyLinkedListV1{" +
      "first=" + first +
      ", size=" + size +
      '}';
  }

  // 내부에서만 사용하므로 private 정적 내부 클래스로 정의
  private static class Node<E> {
    E item;
    Node<E> next;

    public Node(E item) {
      this.item = item;
    }

    @Override
    public String toString() {
      StringBuilder sb = new StringBuilder();
      Node<E> temp = this;
      sb.append("[");
      while (temp != null) {
        sb.append(temp.item);
        if (temp.next != null) {
          sb.append("->");
        }
        temp = temp.next;
      }
      sb.append("]");
      return sb.toString();
    }
  }
}
```

## 배열 리스트 vs 연결 리스트

- **배열 리스트**: 데이터를 조회할 일이 많고, 뒷 부분에 데이터를 추가한다면 배열 리스트가 보통 더 좋은 성능을 제공
- **연결 리스트**: 앞쪽에 데이터를 추가하거나 삭제할 일이 많다면 연결 리스트가 더 좋은 성능을 제공

> 자바에서 제공하는 `LinkedList`는 이중 연결 리스트로 이전 노드, 마지막 노드의 정보도 가지고 있기 때문에 마지막 노드에 추가 삭제 하더라도 O(1)의 성능을 보여줍니다.

## 참고

- [https://www.inflearn.com/course/김영한의-실전-자바-중급-2](https://www.inflearn.com/course/%EA%B9%80%EC%98%81%ED%95%9C%EC%9D%98-%EC%8B%A4%EC%A0%84-%EC%9E%90%EB%B0%94-%EC%A4%91%EA%B8%89-2)
