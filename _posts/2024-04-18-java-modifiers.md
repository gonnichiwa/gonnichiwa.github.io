---
title: java modifiers - protected, default
date: 2024-04-18 21:12:00 +0900
categories: [JAVA]
tags: [protected, default, modifier]  # TAG names should always be lowercase
authors: [gonnichiwa]
---

## protected와 default 기준

- 상속구조에서 자식클래스 인가? protected
+ 같은 패키지 내인가? default
  - 자식 패키지 안됨. 오직 같은 패키지 siblings class 여야함.

### test

- (아래) 구조  
![](https://blog.kakaocdn.net/dn/boFfuf/btsGIXoajSU/7mM2uBHM54ja2Sj5FK7Wl1/img.png)
![](https://blog.kakaocdn.net/dn/biXTun/btsGHtgV9an/cWuyX5NyBmN1b6pK14yLfk/img.png)

<br/>

- (아래) 같은 패키지 내라면 `protected`, `default` 상관없이 참조가능함  
![](https://blog.kakaocdn.net/dn/1Y0OG/btsGKXmUChS/tvaGC4vFnq2ky3sC9DA6YK/img.png)

- (아래) 다른 패키지다? 상속받았을때만 `protected` 가능
- 다른 패키지니까 `default`는 쓸 수 없음.
![](https://blog.kakaocdn.net/dn/GFnk7/btsGJmuiomB/BzcKH7DCxQWsooXKufJOyK/img.png)

- (아래) `protected` 있는 패키지의 하위 패키지라도 `default`와 `protected`는 쑬 수 없음.
![](https://blog.kakaocdn.net/dn/c3Vrja/btsGHa2TgOn/EkxHiMnyae8GaT0UtOnDVK/img.png)


### 정리

- 같은 패키지라면 `default`던 `protected`던 **상관없이 쓸 수 있음.**  

- `default` : 오직 같은 패키지 내에서만 쓸 수 있음
- `protected` : 다른 패키지라면 상속 받아야만 쓸 수 있음.

