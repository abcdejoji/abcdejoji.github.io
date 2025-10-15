---
title: "MySQL JOIN"
categories: [ "DB" ]
tags: [ "[인프런] 김영한의 실전 데이터베이스 - 기본편", "RDBMS", "MySQL" ]
date: "2025-10-07 02:35:00"
---

## Inner Join

![](/assets/img/posts/2025/10/2025-10-07-MySQL-Join/534746625540625.png)

```sql
select *
from 테이블A a
       join 테이블B b on a.id = b.aid
```

> `inner join`이지만 `inner`는 생략 가능합니다.

## Outer Join

### Left Join

![](/assets/img/posts/2025/10/2025-10-07-MySQL-Join/534316385832958.png)

```sql
select *
from 테이블A a
       left join 테이블B b on a.id = b.aid
```

### Right Join

![](/assets/img/posts/2025/10/2025-10-07-MySQL-Join/534338583325250.png)

```sql
select *
from 테이블A a
       right join 테이블B b on a.id = b.aid
```

> `left outer join`, `right outer join`이지만 `outer`는 생략 가능합니다.

> 순서만 바꾸면 동일한 결과이기 때문에 가독성 관계로 `left join`을 많이 쓰고 `right join`은 잘 사용하지 않습니다.

## Self Join

- 본인 테이블을 조인 하는 방법
- 테이블을 별칭으로 구분하여 작성

```sql
select *
from 테이블A a
       join 테이블A b on a.id = b.aid
```

## Cross Join

- `ON` 조건 없이 한 테이블의 모든 행을 다른 테이블의 모든 행과 각각 연결하는 방법

```sql
select *
from 테이블A a
       cross join 테이블B b
```

- 만약 `테이블A`에 데이터가 3개이고 `테이블B`의 데이터가 4개면 `3*4`인 `12개`의 행이 결과로 나옵니다.

> 데이터가 많은 테이블에 사용하면 결과 행이 기하급수적으로 늘어나 서버에 심각한 부하를 줄 수 있으므로 주의해야 합니다.

## 참고

- [https://www.inflearn.com/course/김영한-실전-데이터베이스-기본편](https://www.inflearn.com/course/%EA%B9%80%EC%98%81%ED%95%9C-%EC%8B%A4%EC%A0%84-%EB%8D%B0%EC%9D%B4%ED%84%B0%EB%B2%A0%EC%9D%B4%EC%8A%A4-%EA%B8%B0%EB%B3%B8%ED%8E%B8)
