---
title: FAILURE- Build failed with an exception.
Incompatible because this component declares a component for use during compile-time, compatible with Java 17 and the consumer needed a component for use during runtime, compatible with Java 10
date: 2024-03-28 18:15:00 +0900
categories: [SPRING, JAVA, MAC, GRADLE]
tags: [gradle, spring, java, mac]  # TAG names should always be lowercase
authors: [gonnichiwa]
---

## gradle build와 실패 메세지 처리

### command not found
- 말그대로 커맨드 잘못입력했거나
- 실행 권한 없거나

```
$ sudo ./gradlew build
sudo: ./gradlew: command not found

$ ls -al | grep gradlew
-rw-r--r--   1 gonnichiwa  staff  8692  3 29 18:10 gradlew
```
실행권한이 없음.

```
$ sudo chmod 755 ./gradlew
$ ls -al | grep gradlew
-rwxr-xr-x   1 gonnichiwa  staff  8692  3 29 18:10 gradlew

```
r(읽기,4)w(쓰기,2)x(실행,1) 이므로  
`owner/group/other` 별로 권한 부여 가능함.

실행권한 `755` 줌

### build 실패

```
$ ./gradlew build
............10%...100%

Welcome to Gradle 8.6!
...
FAILURE: Build failed with an exception.
Incompatible because this component declares a component for use during compile-time, compatible with Java 17 and the consumer needed a component for use during runtime, compatible with Java 10

```
gradle 플러그인 컴파일러 자바 버전과 내 mac 로컬의 자바 버전 안맞단다.

![](https://blog.kakaocdn.net/dn/FtHUW/btsGbRW0x4A/cC0J1cBzkox2YkkXqhO0S0/img.png) 
_gradle plugin 버전에 따라 의존하는 java 버전이 상향됐다 8 -> 17or21_

intellij 로 오픈시키면 jdk 버전 맞춰 다운받을줄 알았더니 동작안함.<br/>
intelilj project close (`File` - `Close project`) 해주고
  
  
[이전 포스트](https://gonnichiwa.github.io/posts/aidaboat-1-openproject/#%EA%B8%B0%EC%88%A0%EC%A0%81)에서 개발환경 java 21로 맞추기로 했으니 mac 환경설정도 이에 따라 맞춰준다.

### OpenJDK 설치
  [가이드대로](https://gonnichiwa.github.io/posts/howto-setup-openjdk-on-mac/)
  설치 한 뒤 gradlew 구동

```
$ ./gradlew build
...
BUILD SUCCESSFUL in 10s
```
  

  
