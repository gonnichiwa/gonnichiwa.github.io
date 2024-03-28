---
title: aidaboat-1-openproject
date: 2024-03-28 19:13:00 +0900
categories: [SPRING, JAVA]
tags: [spring, java, gradle]  # TAG names should always be lowercase
authors: [gonnichiwa]
---

## 작성 목적
- rest api 개발 운영 서비스 신규 플젝
- 수행 중 만나는 결정사항들을 기록함.

## 요구사항
### 기술적
- spring boot, cloud인프라 운영환경 (~~뭘쓸까~~)
- cicd
- msa(스럽게, 모듈은 분리하여 DDD지향 하나 서버는 한대만~~사용자수가 초반에 크지 않으니~~)
- gitlab
- jdk 21

### 업무적
- 별도 작성

## 개발환경
+ openjdk 21
  - [oracle jdk 21 EOSL 2031](https://www.java.com/releases/matrix/#note2)
- intellij idea (>= 2024.3 )
- spring boot (>= 3.2)
- gradle (>= 8.0)
- 로컬 : windows,mac, 개발,운영: linux
- gitlab remote repository : private. (커맨드 툴: sourcetree, git bash)
- cicd : 구성 하는대로 업뎃 (jenkins standalone? gitlabCI?)
- mariadb
+ 모듈방식
  - 작은서비스겠으나 DDD지향함
  - 모듈별 기능 배포 용이하게끔
  + 예)
    - 유저:8081
    - 기계정보(제품):8082
    - 주문:...
    - 결제:...
    - 계약:...
    - admin 등등
+ jira free plan
  - 100이메일 noti/일 제한
  - site url(xxxx.atlassian.net/... )변경 안됨
  - 그외 여러가지 기능제한 있겠으나, 써보면서 느껴보는걸로
  + 120일 액티베이션 모니터링한다함, 안그러면 deactivate 됨. 맞는번역인지 확실치 않음, ~~100일간 마늘먹듯이 매일 들어가야하나?~~
    - 모니터링 기준은 로그인 완료, 페이지 클릭 로그들

## 개발&운영 서버 환경
- docker (니까 linux겠지)
- 그외 tbd

## 이 장에서의 수행 범위
- new project (intellij idea)
- create remote repo (gitlab)
+ jira 프로젝트 생성 (aidaboat.atlassian.net)
  - 본 문서 쓰면서 업뎃
- application.yml 로컬개발운영config 나눔

### new project
---
#### intellij
- new project
![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fb1pe8r%2FbtsGbRgD2Oj%2FRITyoaVkfsvvl1JNAADdHK%2Fimg.png)
![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fcn8zKK%2FbtsF8wZMp74%2FqY7u9pT4RRfWtEkgxtNTw1%2Fimg.png)


#### gitlab
- create project만 해줌

### push, clone, gradle 시험
---
#### gitlab push
```bash
$ git init
Initialized empty Git repository in C:/.../aidaboat24/.git/
$ git remote add origin https://gitlab.com/gonnichiwa/aidaboat.git
$ git add .
$ git commit -m "initial commit"
```

#### sourcetree add
- sourcetree 새탭(+)에서 Add
- push

+ .gitIgnore 다른곳에 흩어져있어도 함께 적용됨
  - ![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FrvrgP%2FbtsF9CrGVWH%2Fr5GFTYOAtwpxVfrPq4dIfK%2Fimg.png)


#### 전체 날리고 clone, gradlew 시험
```bash
$ git clone https://gitlab.com/gonnichiwa/aidaboat24.git
$ cd aidaboat24
$ gradlew build
```


### user 모듈 생성
  - 로그인 api를 담당
  - (intellij)new - module 
  - spring-starter-web만 추가
  + 그외 더 필요한것들 `ctrl`+`shift`+`a`, `edit starters` 띄워 추가
    - ![](https://blog.kakaocdn.net/dn/m9oRF/btsGcgm3GHl/kcNEPoPV382A0lGPQtHXI0/img.png)
  - core모듈 따로 빼서 통합 고민중
  - 좀더 진행하고 해도 무방해보임


### config setting
  - application.yml, application-local.yml, application-dev.yml, application-prod.yml
  + 운영 config정보는 운영서버에 숨겨놓고 외부에서 쓰고 싶다

---
```java
package com.aidaboat.user;

import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.boot.builder.SpringApplicationBuilder;

@SpringBootApplication
public class UserApplication {
    private static final String APP_CONFIG_LOCATION = "spring.config.location="
            + "classpath:application.yml"
            + ",optional:file:./config/application-prod.yml"; // TODO: CD 작업시 배포 테스트

    public static void main(String[] args){
        new SpringApplicationBuilder(UserApplication.class)
                .properties(APP_CONFIG_LOCATION)
                .build()
                .run(args);
    }
}
```
- UserApplication.java
- 메인을 application.yml로 둠
- 운영 config파일은 운영서버 내 특정 경로 config/application.yml 로 읽어들이도록
+ `optional:` 줘서 로컬에서 빌드 시 해당 (운영config) 파일 없어도 구동되도록
  - `application.yml`에 active profile을 `prod`로 놓고 돌리면 당연히 안돌겠지...

---
```yml
server:
  port: 8081

spring:
  profiles:
    active: local
# 본 파일 경로에 application-common.yml 만들어서 프로파일마다 그룹으로 함께 참조하도록 지정할 수 있음.
#    group:
#      local:
#        - common
#      dev:
#        - common
#      prod:
#        - common
```
- application.yml
- local,dev,prod 포함하는 common config 역할
- 위 `spring.profiles.active: local` 설정으로 놔도 intellij 설정이 우선됨, 아래 그림 참조
- intellij 설정 : `ctrl`+`shift`+`a`, `edit configuration`, `active profiles`
![](https://blog.kakaocdn.net/dn/v2Cc5/btsGcOKJeZg/amDjFzz6wva7GJ4MEsEBX0/img.png)
![](https://blog.kakaocdn.net/dn/uF6CA/btsF86NsWj0/O7SHlRfBKqnGkIEe71ksT1/img.png)
![](https://blog.kakaocdn.net/dn/RFABq/btsF9HNgCFY/24B6wk9XbXQzk7akhFHttk/img.png)


---
```yml
#내 PC
#server:
#  port: 8082

spring:
  datasource:
    driver-class-name: org.mariadb.jdbc.Driver
    url: jdbc:mariadb://localhost:3306/dbname
    username: ....
    password: ....
```
- application-local.yml
- 이외 `application-dev.yml`, `application-prod.yml`도 함께 생성해줌.


### 참고
- [중계와 중개: https://www.joongang.co.kr/article/2811365#home](https://www.joongang.co.kr/article/2811365#home)
- https://support.atlassian.com/jira-cloud-administration/docs/what-is-the-free-jira-cloud-plan/
- https://www.java.com/releases/matrix/#note2
- https://jojoldu.tistory.com/267