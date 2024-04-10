---
title: gradle - Deprecated Gradle features were used in this build, making it incompatible with Gradle 9.0
date: 2024-04-10 16:07:00 +0900
categories: [JAVA, SPRING, GRADLE]
tags: [spring, test, gradle]  # TAG names should always be lowercase
authors: [gonnichiwa]
---

## 관련 warning msg
```
Deprecated Gradle features were used in this build, making it incompatible with Gradle 9.0.

You can use '--warning-mode all' to show the individual deprecation warnings and determine if they come from your own scripts or plugins.
```

## 조치
### gradle build --warning-mode all
```
$ gradle build --warning-mode all
> Task :test
The automatic loading of test framework implementation dependencies has been deprecated. This is scheduled to be removed in Gradle 9.0. Declare the desired test framework directly on the test suite or explicitly declare the test framework implementation dependencies on the test's runtime classpath. Consult the upgrading guide for further information: https://docs.gradle.org/8.6/userguide/upgrading_version_8.html#test_framework_implementation_dependencies

...
BUILD SUCCESSFUL in 8s
7 actionable tasks: 5 executed, 2 up-to-date
```

### 문서 참조 (왜?)

![](https://blog.kakaocdn.net/dn/bLY9uD/btsGvrXDn1R/MQMt5EsWBiIeyiKW56ZAF0/img.png)
https://docs.gradle.org/8.6/userguide/upgrading_version_8.html#test_framework_implementation_dependencies

- gradle test 수행 시 jvm의 test 프레임웍 로드 중에 testframework 의존 버전 충돌 일어날 가능성 있음
- gradle 9.0 부터 위 프레임웍 로드 단계 삭제 예정
- `TestNG` 쓰고있다면 영향없음
- 쓰고있는 JUnit 버전별로 문서 하단 코드 스니펫 참조하여 추가하시오

### 버전확인
![](https://blog.kakaocdn.net/dn/chuZyZ/btsGuuAso6R/k0KKAPtJAPobcUbnQkK3M0/img.png)
- JUnit.jupiter api 사용중 (Junit 5)

### build.gradle 수정
![](https://blog.kakaocdn.net/dn/cpACTM/btsGuMAVzIm/N3xQK3zFP9lYttAVEspvr0/img.png)
- `testImplementation`은 쓰고있는 junit jupitor 버전에 맞춰서 넣음

### 결과
![](https://blog.kakaocdn.net/dn/bW6vU8/btsGwsuysme/Qzxt1fUOMwLJJuLdQTZaE0/img.png)
- 워닝 메세지 사라짐

