---
title: "MySQL UNION"
categories: [ "DB" ]
tags: [ "[인프런] 김영한의 실전 데이터베이스 - 기본편", "RDBMS", "MySQL" ]
date: "2025-10-09 00:25:00"
---

# UNION

- `JOIN`이 테이블을 수평으로 붙여 컬럼을 늘리는 기술이라면 UNION은 여러 결과 집합을 수직으로 붙여 행을 늘리는 기술
- 흩어져 있는 데잉터를 하나의 집합으로 모으는데 서용
- `UNION`으로 연결하는 `SELECT` 문은 컬럼 개수가 같고 같은 위치의 컬럼은 호환 가능한 데이터 타입이어야 함
- 최종 결과의 컬럼 이름은 첫 번째 `SELECT` 문의 컬럼 이름을 따름
- 두 결과 집헙을 합친뒤 중복되는 행은 자동을 제거하여 고유한 값만 남김

# UNION ALL

- `UNION`과 `UNION ALL`의 유일한 차이점은 중복 처리 여부
- 성능은 중복제거를 하지 않는 `UNION ALL`이 훨씬 빠름
- 중복을 명확히 제거해야 하는 경우가 아니라면 항상 `UNION ALL`을 우선적으로 사용하는 것이 좋음

# UNION 정렬

- 전체 쿼리의 가장 마지막에 한 번만 사용
- `ORDER BY` 절에는 첫 번째 `SELECT` 문의 컬럼 이름이나 별칭만 사용할 수 있음
- 서로 다른 이름의 컬럼을 합쳐 정렬 할 때는 별칭을 사용하는 것을 권장

# 사용 예시

```sql
select c1, c2
from table1
union all
select c1, c2
from table2
order by c1
```

# 참고

- [https://www.inflearn.com/course/김영한-실전-데이터베이스-기본편](https://www.inflearn.com/course/%EA%B9%80%EC%98%81%ED%95%9C-%EC%8B%A4%EC%A0%84-%EB%8D%B0%EC%9D%B4%ED%84%B0%EB%B2%A0%EC%9D%B4%EC%8A%A4-%EA%B8%B0%EB%B3%B8%ED%8E%B8)
