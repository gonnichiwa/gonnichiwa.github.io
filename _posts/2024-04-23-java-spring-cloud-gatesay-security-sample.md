---
title: spring cloud gateway security sample 분석
date: 2024-04-27 19:54:21 +0900
categories: [JAVA, SPRING]
tags: [spring cloud, gateway, spring security]  # TAG names should always be lowercase
authors: [gonnichiwa]
---

## 작성목적

https://spring.io/blog/2019/08/16/securing-services-with-spring-cloud-gateway  

..에서  
spring cloud + security(jwt) + REST service 의 서버 빌드와 초기 구조 구현을 위함  
`SSO(security, jwt token privider)`, `gateway`, `registry`, `service` 조합 구현함.

## 실행

`\spring-cloud-gateway-demo\security-gateway` 에서 `build.sh` 실행

### buld.sh

windows라면 `git bash`로 위 `실행`소스 경로에서 실행 가능

```sh
$ ./build.sh
Performing a clean Maven build
[INFO] Scanning for projects...
[INFO] ------------------------------------------------------------------------
[INFO] Reactor Build Order:
[INFO]
[INFO] secured-service                                                    [jar]
[INFO] security-gateway                                                   [jar]
...
...
#7 exporting to image
#7 exporting layers 0.1s done
#7 writing image sha256:053195b1e74509918917e0665262ba330bfb93caf211094b8cecf953989d87fc done
#7 naming to docker.io/library/scg-demo-secured-service done
#7 DONE 0.1s

What's Next?
  View a summary of image vulnerabilities and recommendations → docker scout quickview
```

### docker-compose up

```sh
$ docker-compose up
time="2024-04-29T09:22:22+09:00" level=warning 
...
 Container gateway  Creating
Error response from daemon: Conflict. The container name "/gateway" is already in use by container "1def8f41aa138836a7dc48b4965b3b56cb23553a2847c763638bd5b66a1f0eb7". You have to remove (or rename) that container to be able to reuse that name.
```

### docker 구동 확인

`docker-desktop`에서 정상 구동 확인함.

![](https://blog.kakaocdn.net/dn/xbUBV/btsG2o5Av3h/51QWpSNsXzVAGchbpZERF0/img.png)  

`security-gateway` group 들어가면 각 서버별로 log 확인 가능함

![](https://blog.kakaocdn.net/dn/b1KeMU/btsG1hFLugE/Q8Vh8W261uqkqkEc7Rx8GK/img.png)


## 브라우저에서 돌려보기

`시크릿 창`에서 `http://localhost:8080/resource` 들어감  

id는 `user1` pw는 `password`  

![](https://blog.kakaocdn.net/dn/zDWov/btsG0aAmdG5/XZT9v3KxFd1TAkJOG4sI90/img.png)

<br/>

로그인 통과 시 권한 확인 창이 뜸.

![](https://blog.kakaocdn.net/dn/uARAS/btsGYQwlXuO/5ZGQTLpUgV4kqfAVMZymd1/img.png)

<br/>

권한 확인 시 `resource access` 처리됨.  
`subject Id`는 실행시 마다 다를 수 있음.

![](https://blog.kakaocdn.net/dn/b7zmzT/btsG1vDTH1G/naGi4jrY8yVoHbkNk17U30/img.png)  

<br/>

이후 `http://localhost:8080` 들어가면 아래처럼 로긴 정보 나오도록 구현되어 있음.

![](https://blog.kakaocdn.net/dn/bs7aXG/btsGY9PRUet/2yqjDSK9il6FjDPKMNaZY1/img.png)

