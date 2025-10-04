---
title: DateTime Format Response 일괄 적용
categories: [ "Java & Spring" ]
date: 2024-09-13
---

`JSON`으로 `Response`로 `LocalDateTime` 혹은 `LocalDate`처럼 시간 관련 값을 내려줄 때, 각각 `format` 형식을 적용하여 주는 방법이 있습니다.

- 기존: yyyy-MM-dd HH:mm:ss
- 변경: yyyy.MM.dd HH:mm:ss

만약 위 처럼 모든 시간 응답 값 포맷을 변경해야 하면 굉장히 많은 곳을 수정해야 합니다.

이런 문제를 해결하고자 `Spring Boot`에서는 `JSON`으로 변환 과정 중간에 `Custom`할 수 있도록 잘 설계 되어 있습니다.

```java

@Configuration
public class ContactAppConfig {

  private static final String dateFormat = "yyyy-MM-dd";
  private static final String dateTimeFormat = "yyyy-MM-dd HH:mm:ss";

  @Bean
  public Jackson2ObjectMapperBuilderCustomizer jsonCustomizer() {
    return builder -> {
      builder.simpleDateFormat(dateTimeFormat);
      builder.serializers(new LocalDateSerializer(DateTimeFormatter.ofPattern(dateFormat)));
      builder.serializers(new LocalDateTimeSerializer(DateTimeFormatter.ofPattern(dateTimeFormat)));
    };
  }
}

```

`Spring Boot`는 `JSON` 변환 라이브러리로 기본적으로 `Jackson`을 사용합니다.

위의 코드는 `JSON`으로 `serializers`할 때, `LocalDateTime`과 `LocalDate`를 지정한 `format`으로 변환되도록 설정 한 것입니다.

# 참고

- [Formatting JSON Dates in Spring Boot](https://www.baeldung.com/spring-boot-formatting-json-dates)
