---
title: elixir-2-phoenix-2-route
date: 2024-08-22 12:11:00 +0900
categories: [Elixir]
tags: [phoenix]  # TAG names should always be lowercase
authors: [gonnichiwa]
---

## 0. 작성목적

 - elixir phoenix 프레임워크로 라우팅 가이드 작성
 - db 연동하여 jwt 인증 서버 생성 목적 계속 진행

## 1. 시작전 필요사항

- erlang/OTP `27`
- elixir `1.17.2`



## 수행 요약

- https://hexdocs.pm/phoenix/request_lifecycle.html

+ `localhost:4000/hello`

![](https://blog.kakaocdn.net/dn/wkEsH/btsJaASsOiI/oZAySxpajYS0BxXFvZmPO1/img.png)

![](https://blog.kakaocdn.net/dn/t2QBC/btsJb7ayB7O/rElDtc2x1kA4L6KmVab0q1/img.png)

* 결과

![](https://blog.kakaocdn.net/dn/d5IX6a/btsJaRsVTzC/8U34ACqndPkevZ5lUhk5Rk/img.png)

<br/>

- `localhost:4000/hello/:pathvariable`

![](https://blog.kakaocdn.net/dn/74wMx/btsJb45XadC/SBUNdpEwiVZkMcGoXoqH8K/img.png)

* 결과

![](https://blog.kakaocdn.net/dn/p4CCv/btsJcQlCIbV/zEGxusCDQkHnUSpaJH1Isk/img.png)





  
## 참고

- https://hexdocs.pm/phoenix/request_lifecycle.html#a-new-route