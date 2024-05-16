---
title: java convention lint - checkstyle install and setting
date: 2024-05-16 07:32:33 +0900
categories: [JAVA]
tags: [java, lint, checkstyle]  # TAG names should always be lowercase
authors: [gonnichiwa]
---

## 작성목적

- java coding convention lint tool 인 `checkstyle`의 사용 시작 방법을 가이드함.
- IDE에서 convention 파일 설정과 코드 작성 중 warning check 안내
- 빌드 툴 (gradle)에 `checkstyle` 설정으로 convention 위반 시 빌드 실패하도록 함


## 독립 실행 시켜보기

- IDE나 빌드 도구 연동 없이 본인 작성 소스 대상으로 `독립 실행` 해 볼 수 있다.
+ jar download
  - https://github.com/checkstyle/checkstyle/releases/
+ convention 정의 파일(rules)와 suppressions.xml 필요함
  - `rules` : 컨벤션 정의, `suppressions` : 컨벤션 예외 지정
  - https://github.com/naver/hackday-conventions-java/blob/master/rule-config/naver-checkstyle-rules.xml  
  - https://github.com/naver/hackday-conventions-java/blob/master/rule-config/naver-checkstyle-suppressions.xml  

  - 실행 예시

```
$ java -DsuppressionFile=[suppression.xml] -jar checkstyle-10.16.0-all.jar -c [규칙파일] [소스폴더]
```
 
```sh
$ java -DsuppressionFile=naver-checkstyle-suppressions.xml -jar checkstyle-10.16.0-all.jar -c naver-checkstyle-rules.xml ./java-playground/src/main/java/springbook/user/service
Starting audit...
[WARN] C:\dev\.\java-playground\src\main\java\springbook\user\service\MockMailSender.java:1: Expected line ending for file is LF(\n), but CRLF(\r\n) is detected. [NewlineAtEndOfFile]
...
Audit done.
```

- 위 결과 에서 `[NewlineAtEndOfFile]` 은 naver-checkstyle-rules.xml 에 아래와 같이 표현되어 있음.

```xml 
<!-- [newline-eof] -->
<module name="NewlineAtEndOfFile">
    <property name="lineSeparator" value="lf"/>
</module>
```

- `..suppresions.xml` 에서 파일별(or정규식파일네임)으로 예외를 아래와 같이 설정할 수 있다.  

```xml
<?xml version="1.0"?>
<!DOCTYPE suppressions PUBLIC
"-//Puppy Crawl//DTD Suppressions 1.1//EN"
"http://www.puppycrawl.com/dtds/suppressions_1_1.dtd">

<suppressions>
    <suppress files="UserController.java" checks=".*"/>
    <suppress files="UserService.java" checks=".*"/>
</suppressions>
```

<br/>

## 빌드 수행 검사

- maven or gradle 과 같은 빌드 도구에서 컴파일 타임에 checkstyle 기준 `컨벤션 위반 시 빌드 실패 처리` 할 수 있다.  

- `독립 실행 시켜보기` 에서 사용했던 `rules`와`suppresions` xml을 프로젝트 root 디렉토리에 넣음.  
![](https://blog.kakaocdn.net/dn/bhpDZ2/btsHpNMi84m/C0E9kK3bYMTATkNRPzGKyk/img.png)
- `build.gradle` 에서 아래와 같이 checkstyle plugin 정의함.  
```groovy
plugins {
    id 'checkstyle'
}
...
checkstyle {
    maxWarnings = 0 // 규칙이 어긋나는 코드가 하나라도 있을 경우 빌드 fail을 내고 싶다면 이 선언을 추가한다.
    configFile = file("${rootDir}/convention/naver-checkstyle-rules.xml")
    configProperties = ["suppressionFile" : "${rootDir}/convention/naver-checkstyle-suppressions.xml"]
    toolVersion ="10.16.0"  // checkstyle 버전 8.24 이상 선언
}
```

### 수행 결과
---

checkstyle 플러그인 동작에 따라 warning 나타날 시 빌드 실패 처리함.  

```cmd
PS C:\dev\..\userauth> gradle build
Starting a Gradle Daemon, 1 incompatible Daemon could not be reused, use --status for details

> Task :checkstyleMain
[ant:checkstyle] [WARN] C:\dev\...\DuplicateUserException.java:1: Expected line ending for file is LF(\n), but CRLF(\r\n) is detected. [NewlineAtEndOfFile]
...
[ant:checkstyle] [WARN] C:\dev\...\SignInUp.java:34: [indentation-tab] Indent must use tab characters [RegexpSinglelineJava]
[ant:checkstyle] [WARN] C:\dev\...\SignInUp.java:35: [indentation-tab] Indent must use tab characters [RegexpSinglelineJava]

> Task :checkstyleMain FAILED
...
* What went wrong:
Execution failed for task ':checkstyleMain'.	 
...

BUILD FAILED in 20s
10 actionable tasks: 7 executed, 3 up-to-date
```

## install on intellij (편집기 설정)

- 편집기 설정하여 코딩 중 컨벤션 위반 사례가 있을 시 실시간 WARNING으로 알려준다.

- (win)file - settings - plugins (mac : intellijIDEA - settings)  
checkstyle-IDEA install, warnings는 accept하고 재시작


### editor set
---

- settings - Tools - Checkstyle
- `Treat Checkstyle errors as warnings` check
- Scan Scope : Only Java sources (including tests)
![](https://blog.kakaocdn.net/dn/ceYB0V/btsHqm1Fu2D/YxDesu03KkphpldPMw2ni0/img.png)

- +눌러서 아래처럼 set
![](https://blog.kakaocdn.net/dn/ceVj3e/btsHp4Ut6qU/ee2USqZPHQqw2UqKuPOp0K/img.png)

- 현재 `naver-checkstyle-rules.xml`을 수정 없이 사용 하고 있음.

```xml
<!--- Suppression -->
<module name="SuppressionFilter">
    <property name="file" value="${suppressionFile}"/>
    <property name="optional" value="true"/>
</module>
```
주석 처리 안했다면 아래처럼 suppresion도 지정

![](https://blog.kakaocdn.net/dn/GDkSX/btsHqpqw04m/0FQdaND8EBFeUZgzVbrMQ0/img.png)

- 준비됐으니 Finish

![](https://blog.kakaocdn.net/dn/1MPQS/btsHqu6hslP/Z9wrmS1iAKBCqiiZFb4880/img.png)

- 추가된 config check 후 settings 창 OK

![](https://blog.kakaocdn.net/dn/39YsX/btsHpmIhkSc/HnFZ6d7s9oyikUho2Qrq4k/img.png)


### editor set 결과
---


- rule에 걸린 라인들을 `노랗게` warning 띄워줌.

![](https://blog.kakaocdn.net/dn/SF5HQ/btsHqnGfPGa/YRr41xMRXoFwgMpqlsCEjk/img.png)


### 참고
---

[https://checkstyle.org/](https://checkstyle.org/)  
[https://naver.github.io/hackday-conventions-java/#_gradle](https://naver.github.io/hackday-conventions-java/#_gradle)  
