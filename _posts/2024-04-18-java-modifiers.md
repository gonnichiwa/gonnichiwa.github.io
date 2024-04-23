---
title: java modifiers - protected, default
date: 2024-04-18 21:12:00 +0900
categories: [JAVA]
tags: [java, protected, default, modifier]  # TAG names should always be lowercase
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


- `mypackage`.`Test` 객체를 만들어놈.
+ `Test`객체를 갖다쓰는 `client` 들의 package 위치에 따라 사용 가능 여부를 따짐.
  - 외부 패키지 (`otherpackage`.`client`)
  - 같은 패키지이나 하위 패키지 (`childpackage`.`Client`)
  - 같은 패키지 (`mypackage`.`client`)

<br/>

+ 외부 패키지다? 상속받았을때만 `protected` 가능
  - 외부 패키지니까 `default`는 쓸 수 없음.
![](https://blog.kakaocdn.net/dn/GFnk7/btsGJmuiomB/BzcKH7DCxQWsooXKufJOyK/img.png)

+ 같은 패키지이나 하위 패키지
  - (아래) `protected` 있는 패키지의 하위 패키지라도 `default`와 `protected`는 쑬 수 없음.
![](https://blog.kakaocdn.net/dn/c3Vrja/btsGHa2TgOn/EkxHiMnyae8GaT0UtOnDVK/img.png)

+ 같은 패키지
  - (아래) 같은 패키지 내라면 ~~당연히~~ `protected`, `default` 상관없이 참조가능함  
![](https://blog.kakaocdn.net/dn/1Y0OG/btsGKXmUChS/tvaGC4vFnq2ky3sC9DA6YK/img.png)


### 정리

- `default` : **오직 같은 패키지 depth 경로 내** 에서만 쓸 수 있음 (하위 패키지 안됨)
- `protected` : 패키지 경로 다르다면, __상속 받아야만 쓸 수 있음__.
- 같은 패키지라면 `default`던 `protected`던 **상관없이 쓸 수 있음.**  

