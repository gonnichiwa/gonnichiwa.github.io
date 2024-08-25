---
title: elixir-0-intro-2-iex -- how to use iex
date: 2024-08-20 18:50:00 +0900
categories: [Elixir]
tags: [iex]  # TAG names should always be lowercase
authors: [gonnichiwa]
---

## 0. 작성목적

 - elixir iex 사용법 예시
 - `.ex` 파일 `iex` 에서 사용하기

## 1. iex 사용법 예시

+ `.ex` 파일 `iex` 에서 사용하기
  - 아래처럼 `.ex` 파일 작성함

![](https://blog.kakaocdn.net/dn/bb308k/btsJe8mJMPR/ePLt4w9xdPilmu2lmY3UkK/img.png)

`use.ex` 위치에서 `iex` 구동
```cmd
C:\dev\elixir\playground>iex
Erlang/OTP 27 [erts-15.0.1] [source] [64-bit] [smp:16:16] [ds:16:16:10] [async-threads:1] [jit:ns]

Interactive Elixir (1.17.2) - press Ctrl+C to exit (type h() ENTER for help)
```

- `iex` 에서 돌려보고싶은 파일 컴파일 한 뒤 함수 호출
```
iex(1)> c("use.ex")
[Example, Hello]
iex(2)> Example.hello("myname")
"Hi, myname"
iex(3)>
```

### 1-2. 다른 방법

```elixir
defmodule Math do
  def sum(a,b) do
    a + b
  end
end
```

- `.ex` 파일을 컴파일해서 쓸 수 있음. (아래처럼)
```
working-dir $ elixirc Math.ex
working-dir $ iex
iex(1)> Math.sum(1,2)
3
```

- 컴파일 하면 `Elixir.Math.beam` 바이트코드 파일 생성되어 이게 메모리 올라가서 실행됨
  
![](https://blog.kakaocdn.net/dn/q1l5J/btsJdGRrO0T/jrk5FxiI3ifBG4YjcjNmnk/img.png)

<br/>

- 고로 `.beam` 파일 만든 경로외에서는 안됨
```
C:\>iex
Erlang/OTP 27 [erts-15.0.1] [source] [64-bit] [smp:16:16] [ds:16:16:10] [async-threads:1] [jit:ns]

Interactive Elixir (1.17.2) - press Ctrl+C to exit (type h() ENTER for help)
iex(1)> Math.sum(1,2)

** (UndefinedFunctionError) function Math.sum/2 is undefined (module Math is not available)
    Math.sum(1, 2)
    iex:1: (file)
iex(1)>
```

- `.exs` 는 scripting 모드로 실행됨 (.beam 안만듬)

![](https://blog.kakaocdn.net/dn/b5ewA1/btsJcRTZrnh/6FU4shMEfnKkd2pCHu58rk/img.png)

- `defp`는 `private`접근. (`defmodule 안에서`)

![](https://blog.kakaocdn.net/dn/bpOOuQ/btsJeqf1Qum/Lk4ACUwwzJr2RffebY1hy1/img.png)