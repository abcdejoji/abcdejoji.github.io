---
title: "Stream API"
categories: [ "Java & Spring" ]
tags: [ "[인프런] 김영한의 실전 자바 - 고급 3편" ]
date: "2025-09-30 22:43:00"
---

## Stream API 란?

- 스트림(Stream): 자바 8부터 추가된 기능으로, 데이터 흐름을 추상화해서 다루는 도구
- 컬렉션(Collection) 배열 등의 요소들을 연산 파이프라인을 통해 연속적인 형태로 처리할 수 있게 해줌

## 특징

- **데이터 소스를 변경하지 않음**
  - 스트림에서 제공하는 연산들은 원본 컬렉션을 변경하지 않고 결과만 새로 생성
- **일회성**
  - 한 번 소비 된 스트림은 다시 사용할 수 없으며, 필요하다면 새로 스트림을 생성해야 함
- **파이프라인**
  - 중간 연산 들이 이어지다가 최종 연산을 만나면 연산이 수행되고 종료
- **지연 연산**
  - 중간 연산은 필요할 때까지 실제로 동작하지 않고, 최종 연산이 실행될 때 한 번에 처리
- **병렬 처리 용이**
  - 스트림으로 부터 병렬 스트림(Parallel Stream)을 쉽게 만들 수 있어서, 멀티코어 환경에서 병렬 연산을 비교적 단순한 코드로 작성할 수 있음

## Stream 생성

| 생성 방법            | 설명                     | 코드 예시                         |
|------------------|------------------------|-------------------------------|
| 컬렉션              | 컬렉션에서 스트림 생성           | list.stream()                 |
| 배열               | 배열에서 스트림 생성            | Arrays.stream(arr)            |
| Stream.of(…)     | 직접 요소를 입력해 스트림 생성      | Stream.of(”a”, “b”)           |
| 무한 스트림(iterate)  | 무한 스트림 생성(초깃값 + 함수)    | Stream.iterate(0, n → n +2)   |
| 무한 스트림(generate) | 무한 스트림 생성(Supplier 사용) | Stream.generate(Math::random) |

## 중간 연산

| 연산        | 설명                                    |
|-----------|---------------------------------------|
| filter    | 조건에 맞는 요소만 남김                         |
| map       | 요소를 다른 형태로 변환                         |
| flatMap   | 중첩 구조 스트림을 일차원으로 평탄화                  |
| distinct  | 중복 요소 제거                              |
| sorted    | 요소 정려                                 |
| peek      | 중간 처리(로그, 디버깅)                        |
| limit     | 앞에서 N개의 요소만 추출                        |
| skip      | 앞에서 N개의 요소를 건너뛰고 이후 요소만 추출            |
| takeWhile | 조건을 만족하는 동안 요소 추출(java 9+)            |
| dropWhile | 조건을 만족하는 동안 요소를 버리고 이후 요소 추출(java 9+) |

## 최종 연산

| 연산              | 설명                                  |
|-----------------|-------------------------------------|
| collect         | Collector를 사용하여 결과 수집(다양한 형탤 변환 가능) |
| toList(java16+) | 스트림을 불변 리스트로 수집                     |
| toArray         | 스트림을 배열로 변환                         |
| forEach         | 각 요소에 대해 동작 수행(반환값 없음)              |
| count           | 요소 개수 반환                            |
| reduce          | 누적 함수를 사용해 모든 요소를 단일 결과로 합침         |
| min/max         | 최솟값, 최댓값                            |
| findFirst       | 조건에 맞는 첫 번째 요소                      |
| findAny         | 조건에 맞는 아무 요소                        |
| anyMatch        | 하나라도 조건을 만족하면 true                  |
| allMatch        | 모두 조건이 만족하면 true                    |
| noneMatch       | 하나도 조건이 만족하지 않으면 true               |

## 기본 특화형 스트림

- IntStream
- LongStream
- DoubleStream

### 주요 기능 및 메서드

| 메서드/기능                         | 설명                                                    |
|--------------------------------|-------------------------------------------------------|
| sum()                          | 모든 요소의 합계                                             |
| average()                      | 모든 요소의 평균                                             |
| summaryStatistics()            | 최솟값, 최댓값, 합계, 개수, 평균 등이 담긴 IntSummaryStatistics 객체 반환 |
| mapToLong() <br> mapToDouble() | 타입 변환                                                 |
| mapToObj()                     | 객체 스트림으로 변환                                           |
| boxed()                        | 박싱 된 객체 스트림으로 변환                                      |
| sum, min(), max,(), count()    | 합계, 최솟값, 최댓값, 개수                                      |

### 성능

- 스트림보다 전통적인 for문이 1.5배 ~ 2배 정도 빠름
  - 여기서 말하는 스트림은 `Integer`같은 객체를 다루는 `Stream`을 의미
  - 박싱/언박싱 오버헤드가 발생
- 기본형 특화 스트림은 전통적인 랙문에 가까운 성능을 보여줌
  - 전통적인 `for`문과 거의 비슷하거나 전통적인 `for`문인 10% ~ 30% 정도 더 빠름
  - 박싱/언박싱 오버헤드가 없음
  - 내부적으로 최적화된 연산을 수행

## 컬렉터

- `collect`: 반환 값을 만들어내는 최종 연산

| 기능     | 메서드 예시                                                                                        | 설명                                                                          |
|--------|-----------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------|
| List   | toList() <br/> toUnmodifiableList()                                                           | List로 모음 <br/> 불면 리스트를 만듦                                                   |
| Set    | toSet() <br/> toCollection(HashSet::new)                                                      | Set으로 모음 <br/> 특정 타입으로 모을려면 toCollection() 사용                               |
| Map    | toMap(keyMapper, valueMapper) <br/> toMap(keyMapper, valueMapper, mergeFunction, mapSupplier) | Map 형태로 수집 중복키 발생 시, <br/> mergeFunction으로 해결하고 mapSupplier로 맵 타입을 지정할 수 있음 |
| 그룹화    | groupingBy(classifier) <br> groupingBy(classifier, downstreamCollector)                       | 특정 기준 함수에 따라 그룹별로 스트림을 요소로 묶음 <br> 각 그룹에 대해 추가로 적용할 다운스트림 컬렉터를 지정할 수 있음     |
| 분할     | partitioningBy(predicate) <br> partitioningBy(predicate, downstreamCollector)                 | 결과가 true, false 두 가지로 나뉘어 2개 그룹으로 분할                                        |
| 통계     | counting(), summingInt(), averagingInt(), summarizingInt()                                    | 개수, 합계, 평균, 통계                                                              |
| 리듀싱    | reducing(…)                                                                                   | 스트림의 reduce()와 유사하게, Collector환경에서 요소를 하나로 합치는 연산을 할 수 있음                   |
| 문자열 연결 | joining(delimiter, prefix, suffix)                                                            | 문자열 스트림을 하나로 합쳐서 연결                                                         |
| 매핑     | mapping(mapper, downstreamCollector)                                                          | 각 요소를 다른 값으로 변환 한 뒤 다운 스트림 컬렉터로 넘김                                          |

### 다운 스트림

- 그룹화 된 이후 각 그룹 내부에서 추가적인 연산 또는 결과물을 정의하는 역할

| Collector           | 사용 메서드 예시                                                      | 설명                                              |
|---------------------|----------------------------------------------------------------|-------------------------------------------------|
| counting()          | Collectors.counting()                                          | 요소의 개수                                          |
| summingInt()        | Collectors.summingInt(…)                                       | 합산 값                                            |
| averagingInt()      | Collectors.averagingInt(…)                                     | 평균 값                                            |
| minBy, maxBy()      | Collectors.minBy(Comparator) <br> Collectors.maxBy(Comparator) | 최소값, 최댓값                                        |
| summarizingInt()    | Collectors.summarizingInt(…)                                   | 통계                                              |
| mapping()           | Collectors.mapping(변환 함수, 다운 스트림)                              | 각 요소를 다른 값으로 변환한 뒤, 변환 된 값을 다시 다른 Collector로 수집 |
| collectingAndThen() | Collectors.collectingAndThen(다른 컬렉터, 변환 함수)                    | 다운 스트림 컬렉터의 결과를 최종적으로 한 번 더 가공                  |
| reducing()          | Collectors.reducing(초깃값, 변환 함수, 누적 함수)                         | 스트림의 reduce()와 유사하게, 그룹 내 요소들을 하나로 합치는 로직을 정의   |
| toList(), toSet()   | Collectors.toList() <br> Collectors.toSet()                    | List, Set로 수집 <br> toCollection(…)으로 구현체 지정 가능  |

## 참고

- [https://www.inflearn.com/course/김영한의-실전-자바-고급-3](https://www.inflearn.com/course/%EA%B9%80%EC%98%81%ED%95%9C%EC%9D%98-%EC%8B%A4%EC%A0%84-%EC%9E%90%EB%B0%94-%EA%B3%A0%EA%B8%89-3)
