---
title: "Stack, Queue, Deque"
categories: [ "Java & Spring" ]
tags: [ "[인프런] 김영한의 실전 자바 - 중급 2편", "자료구조" ]
date: "2025-09-19 01:21:00"
---

## Stack

- LIFO(Last In First Out): 후입 선출

![](/assets/img/posts/2025/2025-09-19-Stack-Queue-Deque/626614480060708.png)
![](/assets/img/posts/2025/2025-09-19-Stack-Queue-Deque/626621678029583.png)

> 성능이 좋지 않아 사용하지 않습니다. 대신 `Deque`를 사용하면 됩니다.

## Queue

- FIFO(First In First Out): 선입 선출

![](/assets/img/posts/2025/2025-09-19-Stack-Queue-Deque/626636131918250.png)

## Deque

- Stack, Queue의 기능을 모두 사용할 수 있는 자료구조

![](/assets/img/posts/2025/2025-09-19-Stack-Queue-Deque/626648041785291.png)

- `offerFirst()`, `push()`: 앞에 추가
- `offerLast()`, `offer()`: 뒤에 추가
- `pollFirst()`, `pop()`, `poll()`: 앞에서 꺼냄
- `pollLast()`: 뒤에서 꺼냄

### 상속 구조

![](/assets/img/posts/2025/2025-09-19-Stack-Queue-Deque/626655891917708.png)

### ArrayDeque vs LinkedList

- `ArrayDeque`는 특별한 원형 큐 자료를 사용
- 둘 다 앞 뒤 입력 모두 O(1)의 성능을 제공
- 이론적으로는 `LinkedList`가 삽입, 삭제가 자주 발생할 때 더 효율적일 수 있으나, 실제론 배열을 사용하는 `ArrayDeque`가 더 나은 성능을 보임

> `ArrayDeque`가 `LinkedList`보다 거의 모든 면에서 성능이 좋기 때문에 `ArrayDeque`를 사용하면 됩니다.

## 참고

- [https://www.inflearn.com/course/김영한의-실전-자바-중급-2](https://www.inflearn.com/course/%EA%B9%80%EC%98%81%ED%95%9C%EC%9D%98-%EC%8B%A4%EC%A0%84-%EC%9E%90%EB%B0%94-%EC%A4%91%EA%B8%89-2)
