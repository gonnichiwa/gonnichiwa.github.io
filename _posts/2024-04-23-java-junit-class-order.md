---
title: junit5 - test class ordering
date: 2024-05-15 12:50:33 +0900
categories: [JAVA]
tags: [java, junit, classorder]  # TAG names should always be lowercase
authors: [gonnichiwa]
---

## 작성 목적

- TestClass들의 실행 순서를 number로 지정하고 싶을 때 사용하기 위함.

## Junit Test 클래스 수행 순서 ordering

+ resource/junit-platform.properties 생성
  - ![](https://blog.kakaocdn.net/dn/bjwJ3g/btsHq197mMG/RIensWhdKHlgTK8fiQweS1/img.png)

+ properties 
  - junit.jupiter.testclass.order.default = \
    org.junit.jupiter.api.ClassOrderer$OrderAnnotation


## 테스트 클래스 Ordering

- 두개의 클래스 `HelloControllerTest`, `LoginControllerTest` 에서
- `LoginControllerTest` 먼저 test 후 `HelloControllerTest` test 수행 의도

```java
@Order(1)
public class LoginControllerTest {...}
```

```java
@Order(2)
public class HelloControllerTest {...}
```

## 결과 확인

- gradle verification - test 로 확인


![](https://blog.kakaocdn.net/dn/caexoz/btsHrw9P9tm/szUb1L1moXsW7MLRQ2qZAk/img.png)


- 순서 바꾼 뒤 다시 test 수행

![](https://blog.kakaocdn.net/dn/bnE7rS/btsHqNqHfha/Fi88WdYGky8FhkFnFSk1vK/img.png)


### 참고

https://junit.org/junit5/docs/snapshot/user-guide/index.html#writing-tests-test-execution-order-classes

