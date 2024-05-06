---
title: gradlew build spring boot proj on mac
date: 2024-03-28 18:15:00 +0900
categories: [SPRING, JAVA, MAC, GRADLE]
tags: [gradle, spring, java, mac]  # TAG names should always be lowercase
authors: [gonnichiwa]
---

spring boot proj 소스 클론 이후 `gradlew` 빌드 과정을 기록함.

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
r(읽기,4)w(쓰기,2)x(실행,1) 이므로 `owner/group/other` 별로 권한 부여 가능함.

실행권한 주려고 `755` 줌

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

### 사족

- 이렇게 타기종 머신에서 clone받아 환경 세팅할때 자잘한 문제가 생기면 시간 잡아먹고 ~~집에 못가는~~ 요인이 될텐데

+ 추가 개발인력이 들어와서 가이드 필요 생겼을 때
  1. 로컬 개발환경에서 docker-compose로 docker 배포환경 셋하고 IDE에서 remote 디버깅 설정하는 방법이 있겠다 [여기처럼](https://dev.gmarket.com/72)

  - OR,

  2. jdk 설치 외 필요한 것들 (db, mq, redis etc) 설치 스크립트 제공할 수 있을것임

- `1.` 이나 `2.` 방법 모두 `문서` 작성과 제공, 가이드는 필요할 수 밖에 없으므로 상황에 맞게 진행해야 할 것임.

  

  
