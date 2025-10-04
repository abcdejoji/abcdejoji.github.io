---
title: 래퍼, Class, System, Math, Random
categories: [ "Java & Spring" ]
tags: [ "인프런 - 실전 자바 중급 1편", 김영한 ]
date: 2025-10-05 19:00:00
---

# 래퍼 클래스

## 기본형의 한계

- **객체가 아님**: 객체는 유용한 메서드를 제공할 수 있는데, 기본형은 객체가 아니므로 메서드를 제공할 수 없습니다. 또한, 객체 참조가 필요한 컬렉션 프레임워크, 제네릭도 사용할 수 없습니다.
- **null 일수 없다**: 때로는 `없음` 이라는 상태가 필요할 수 있는데, 기본형 타입은 `null` 을 가질 수 없습니다.

## 자바 래퍼 클래스

- `byte` → `Byte`
- `short` → `Short`
- `int` → `Integer`
- `long` → `Long`
- `float` → `Float`
- `double` → `Double`
- `char` → `Character`
- `boolean` → `Boolean`

### 특징

- 불변 객체
- `equals()`로 비교

### 박싱, 언박싱, 오토 박싱

```java
public static void main(String[] args) {
  // Primitive -> Wrapper
  int value = 7;
  Integer boxedValue = Integer.valueOf(value); // 박싱

  // Wrapper -> Primitive
  int unboxedValue = boxedValue.intValue(); // 언박싱

  System.out.println("boxedValue = " + boxedValue);
  System.out.println("unboxedValue = " + unboxedValue);
}
```

```java
public static void main(String[] args) {
  // Primitive -> Wrapper
  int value = 7;
  Integer boxedValue = value; // 오토 박싱(Auto-boxing)

  // Wrapper -> Primitive
  int unboxedValue = boxedValue; // 오토 언박싱(Auto-Unboxing)

  System.out.println("boxedValue = " + boxedValue);
  System.out.println("unboxedValue = " + unboxedValue);
}
```

### parseInt() vs valueOf()

- `parseInt()` : 기본형을 반환
- `valueOf()` : 래퍼 타입을 반환

### 기본형, 래퍼 클래스 어떤것을 사용?

CPU 연산을 아주 많이 수행하는 특수한 경우가 아닌 이상 유지보수하기 더 쉬운 것을 선택하면 됩니다.

# Class 클래스

## 주요 기능

- **타입 정보 얻기**: 클래스의 이름, 슈퍼클래스, 인터페이스, 접근 제한자 등과 같은 정보를 조회할 수 있습니다.
- **리플렉션**: 클래스에 정의된 메서드, 필드, 생성자 등을 조회하고, 이들을 통해 객체 인스턴스를 생성하거나 메서드를 호출하는 등의 작업을 할 수 있습니다.
- **동적 로딩과 생성**: `Class.forName()` 메서드를 사용하여 클래스를 동적으로 로드하고, `newInstance()`메서드를 통해 새로운 인스턴스를 생성할 수 있습니다.
- **애노테이션 처리**: 클래스에 적용된 `애노테이션(annotation)`을 조회하고 처리하는 기능을 제공합니다.

```java
public class ClassMetaMain {
  public static void main(String[] args) throws Exception {
    //Class 조회
    Class clazz = String.class; // 1. 클래스에서 조회
    //Class clazz = new String().getClass(); // 2. 인스턴스에서 조회
    //Class clazz = Class.forName("java.lang.String"); // 3. 문자열로 조회

    // 모든 필드 출력
    Field[] fields = clazz.getDeclaredFields();
    for (Field field : fields) {
      System.out.println("field = " + field.getType() + " " + field.getName());
    }

    // 모든 메서드 출력
    Method[] methods = clazz.getDeclaredMethods();
    for (Method method : methods) {
      System.out.println("method = " + method);
    }

    // 상위 클래스 정보 출력
    System.out.println("Superclass: " + clazz.getSuperclass().getName());

    // 인터페이스 정보 출력
    Class[] interfaces = clazz.getInterfaces();
    for (Class i : interfaces) {
      System.out.println("Interface: " + i.getName());
    }
  }
}
```

# System 클래스

```java
public class SystemMain {

  public static void main(String[] args) {
    // 현재 시간(밀리초)를 가져온다.
    long currentTimeMillis = System.currentTimeMillis();
    System.out.println("currentTimeMillis = " + currentTimeMillis);

    // 현재 시간(나노초)를 가져온다.
    long currentTimeNano = System.nanoTime();
    System.out.println("currentTimeNano = " + currentTimeNano);

    // 환경 변수를 읽는다.
    System.out.println("getenv= " + System.getenv());

    // 시스템 속성을 읽는다.
    System.out.println("properties = " + System.getProperties());
    System.out.println("Java version: " + System.getProperty("java.version"));

    // 배열을 고속으로 복사한다.
    char[] originalArray = {'h', 'e', 'l', 'l', 'o'};
    char[] copiedArray = new char[5];
    System.arraycopy(originalArray, 0, copiedArray, 0, originalArray.length);

    // 배열 출력
    System.out.println("copiedArray = " + copiedArray);
    System.out.println("Arrays.toString = " + Arrays.toString(copiedArray));

    // 프로그램 종료
    System.exit(0);
  }
}
```

# Math 클래스

```java
public class MathMain {

  public static void main(String[] args) {
    // 기본 연산 메서드
    System.out.println("max(10, 20): " + Math.max(10, 20)); // 최대값
    System.out.println("min(10, 20): " + Math.min(10, 20)); // 최소값
    System.out.println("abs(-10): " + Math.abs(-10)); // 절대값

    // 반올림 및 정밀도 메서드
    System.out.println("ceil(2.1): " + Math.ceil(2.1)); // 올림
    System.out.println("floor(2.1): " + Math.floor(2.1)); // 내림
    System.out.println("round(2.5): " + Math.round(2.5)); // 반올림

    // 기타 유용한 메서드
    System.out.println("sqrt(4): " + Math.sqrt(4)); //제곱근
    System.out.println("random(): " + Math.random()); //0.0 ~ 1.0 사이의 double 값
  }
}
```

# Random

```java
public class RandomMain {

  public static void main(String[] args) {
    Random random = new Random();
    //        Random random = new Random(1); //seed가 같으면 Random의 결과가 같다.

    int randomInt = random.nextInt();
    System.out.println("randomInt: " + randomInt);

    double randomDouble = random.nextDouble();//0.0d ~ 1.0d
    System.out.println("randomDouble: " + randomDouble);

    boolean randomBoolean = random.nextBoolean();
    System.out.println("randomBoolean: " + randomBoolean);

    // 범위 조회
    int randomRange1 = random.nextInt(10);//0 ~ 9까지 출력
    System.out.println("0 ~ 9: " + randomRange1);

    int randomRange2 = random.nextInt(10) + 1;// 1 ~ 10까지 출력
    System.out.println("1 ~ 10: " + randomRange2);
  }
}
```

# 참고

- [https://www.inflearn.com/course/김영한의-실전-자바-중급-1](https://www.inflearn.com/course/%EA%B9%80%EC%98%81%ED%95%9C%EC%9D%98-%EC%8B%A4%EC%A0%84-%EC%9E%90%EB%B0%94-%EC%A4%91%EA%B8%89-1)
