---
title: "URL과 매칭되는 Local Static Resources 읽어오기"
categories: [ "Java & Spring" ]
date: "2024-08-24 00:00:00"
---

AWS S3 같은 서비스를 이용하고 있다면, 업로드 된 파일의 URL을 통해 파일을 읽어올 수 있습니다.

하지만, AWS S3가 아닌 서버 자체에 파일이 있는 경우 외부에서 직접 접근할 수 없으므로, 요청을 받으면 서버가 로컬에 있는 파일을 읽어 응답을 내려주어야 합니다.

Spring Boot에서는 이런 부분을 설정을 통해 간단하게 할 수 있게 해줍니다.

```java

@Configuration
public class WebMvcConfig implements WebMvcConfigurer {

  @Value("${base-file-path}")
  private String baseFilePath;

  @Override
  public void addResourceHandlers(ResourceHandlerRegistry registry) {
    registry.addResourceHandler("/images/**")
      .addResourceLocations("file://%s/".formatted(baseFilePath));
  }
}

```

- `/images` 하위로 오는 모든 경로는 `file://BASE_FILE_PATH/` 로 매핑
- `localhost:8080/images/a.pdf` → `file://BASE_FILE_PATH/a.pdf`

## 참고

- [로컬 환경에서 정적 리소스 리로드 하기](https://bottom-to-top.tistory.com/39)
