---
title: elixir 0 - 튜토리얼
date: 2024-08-20 18:50:00 +0900
categories: [Elixir]
tags: [env]  # TAG names should always be lowercase
authors: [gonnichiwa]
---

## 0. 사용 이유
 - 최하단 `참고` 영상들 볼것

## 0-1. 작성목적

 - elixir와 웹프레임워크 phoenix로 사이트 맹글기 찍먹

## 1. 개발환경 설정 (windows)

+ erlang 설치 (`windows`,`mac`):  
  - https://elixir-lang.org/install.html
  - erlang/OTP `27`버전 설치함.

+ elixir `1.17.2` 설치
  - https://elixir-lang.org/install.html#windows

- `erlang`과 `elixir` 모두 환경변수 setting.

```
C:\Users\keept>erl
Erlang/OTP 27 [erts-15.0.1] [source] [64-bit] [smp:16:16] [ds:16:16:10] [async-threads:1] [jit:ns]

Eshell V15.0.1 (press Ctrl+G to abort, type help(). for help)
1>
BREAK: (a)bort (A)bort with dump (c)ontinue (p)roc info (i)nfo
       (l)oaded (v)ersion (k)ill (D)b-tables (d)istribution

C:\Users\keept>

C:\Users\keept>elixir -v
Erlang/OTP 27 [erts-15.0.1] [source] [64-bit] [smp:16:16] [ds:16:16:10] [async-threads:1] [jit:ns]

Elixir 1.17.2 (compiled with Erlang/OTP 27)
```

+ 튜토리얼 돌려볼려면 `PostgresSQL` 이 필요함
  - https://www.postgresql.org/download/windows/
  - 여기서는 `15.8` 다운받음.
  - 비번 `1234` port `5432`
  - Locale `Korean, korea` set
  - `stackbuilder`로 따로 더 설치할거 없으니 체크해제 후 완료처리.

## 2. 일단 쳐보기

- https://joyofelixir.com/1-appeasing-the-masses/
- https://joyofelixir.com/2-where-did-i-put-that-value/

+ `cmd` 에서 `iex` 치면 아래처럼 나온다.
```
C:\Users\keept>iex
Erlang/OTP 27 [erts-15.0.1] [source] [64-bit] [smp:16:16] [ds:16:16:10] [async-threads:1] [jit:ns]

Interactive Elixir (1.17.2) - press Ctrl+C to exit (type h() ENTER for help)
iex(1)>
```

### string 할당 "Hello #{var}"
---
<br/>

```
iex(1)> place = "world"
"world"
iex(2)> "hello #{place}"
"hello world"
```

### list
---

**index로 접근할 수 없게 만들어놨다.**

```
iex(10)> shopping_list = ["bread","egg","ham"]
["bread","egg","ham"]
iex(17)> shopping_list[1]
** (ArgumentError) the Access module does not support accessing lists by index, got: 1

Accessing a list by index is typically discouraged in Elixir, instead we prefer to use the Enum module to manipulate lists as a whole. If you really must access a list element by index, you can Enum.at/1 or the functions in the List module
    (elixir 1.17.2) lib/access.ex:347: Access.get/3
    iex:17: (file)
```

- 책에는 어떻게 접근하는지 바로 알려주지 않는데, 이유가 있는 듯..


### map
---
<br/>

```
iex(10)> person = %{"name" => "Roberto", "age" => 56, "gender" => "Male"}
%{"age" => 56, "gender" => "Male", "name" => "Roberto"}
iex(11)> person["name"]
"Roberto"
iex(12)> person["age"]
56
```







## 참고

- [liftIO 2022 : 웹 개발의 모순과 Elixir가 특효약인 이유 - 한국축산데이터 CTO Max(이재철](https://www.youtube.com/watch?v=lAaD-6OQSHE)

+ 요구사항 사례별 예시
  - [그럼에도 불구하고 Elixir: 카카오워크 서버팀의 Elixir 실용주의 프로그래밍 / if(kakao)2022](https://www.youtube.com/watch?v=NotXpRovDoA)

+ fff
  - [[OKKY 3월 세미나] Elixir: 대용량 분산 웹개발의 혁명 (부제: Java / C++ / Python이 OOP 언어가 아닌 이유)](https://www.youtube.com/watch?v=Rjyf_dELAeg)
