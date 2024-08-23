---
title: elixir-2-phoenix-3-plug
date: 2024-08-23 12:11:00 +0900
categories: [Elixir]
tags: [phoenix]  # TAG names should always be lowercase
authors: [gonnichiwa]
---

## 0. 작성목적

 - elixir phoenix 프레임워크로 `plug` 가이드 작성
 - db 연동하여 jwt 인증 서버 생성 목적 계속 진행

## 1. 시작전 필요사항

- erlang/OTP `27`
- elixir `1.17.2`

## `Plug` 요약

Plug는 Phoenix의 HTTP 레이어의 핵심에 있으며, Phoenix는 Plug를 중심에 둡니다. 우리는 요청 생명주기의 모든 단계에서 Plug와 상호작용하며, 엔드포인트, 라우터, 컨트롤러와 같은 핵심 Phoenix 구성 요소들도 내부적으로는 모두 Plug입니다. 이제 Plug가 무엇이 특별한지 알아봅시다.

Plug는 웹 애플리케이션 사이에서 조합 가능한 모듈에 대한 규격입니다. 또한, 다양한 웹 서버의 연결 어댑터에 대한 추상화 레이어이기도 합니다. Plug의 기본 개념은 우리가 작업하는 "연결"의 개념을 통합하는 것입니다. 이는 Rack과 같은 다른 HTTP 미들웨어 레이어와는 달리, `요청과 응답이 미들웨어 스택에서 분리되지 않는다`는 점에서 차이가 있습니다.

Plug는 가장 단순한 수준에서 `함수 Plug`와 `모듈 Plug`라는 두 가지 형태로 제공됩니다.

### `Function Plug, 함수 플러그`

![](https://blog.kakaocdn.net/dn/xX9J5/btsJdHC4Ia3/1zuPBVW7xCreWKxpHO193k/img.png)

- 결과

![](https://blog.kakaocdn.net/dn/ci2qGn/btsJdocItNN/ktOk9BFYGCgRUQ8UcgYYk0/img.png)

<br/>

### `Module Plug, 모듈 플러그`

모듈 플러그(Module plugs)는 모듈에서 연결 변환을 정의할 수 있게 해주는 또 다른 유형의 플러그입니다. 이 모듈은 두 가지 함수만 구현하면 됩니다:

init/1: call/2에 전달할 인수나 옵션을 초기화합니다.
call/2: 실제로 연결 변환을 수행합니다. 이 함수는 이전에 본 함수 플러그와 동일한 방식으로 작동합니다.
이 기능을 실습하기 위해, 다른 플러그, 컨트롤러 액션, 그리고 뷰에서 사용할 수 있도록 연결에 :locale 키와 값을 넣는 모듈 플러그를 작성해보겠습니다. 아래 내용을 `lib/hello_web/plugs/locale.ex` 파일에 작성하면 됩니다.

![](https://blog.kakaocdn.net/dn/cihePn/btsJeVHivs5/WBkP1Hlem4vzUz01t3BTzk/img.png)

- 결과

![](https://blog.kakaocdn.net/dn/cw77E1/btsJej2ZxqH/p96vVn9ix4PPn1IPF5vW2K/img.png)





## 참고

- https://hexdocs.pm/phoenix/plug.html