---
title: elixir-2-phoenix-1-upandrunning
date: 2024-08-22 12:11:00 +0900
categories: [Elixir]
tags: [phoenix]  # TAG names should always be lowercase
authors: [gonnichiwa]
---

## 0. 작성목적

 - elixir phoenix 프레임워크로 웹 요청 생성 플젝 편다
 - db 연동하여 jwt 인증 서버 생성 목적 계속 진행

## 1. 시작전 필요사항

- erlang/OTP `27`
- elixir `1.17.2`

+ Phoenix db연동해야하니 돌려볼려면 `PostgresSQL` 이 필요함
  - https://www.postgresql.org/download/windows/
  - 여기서는 `15.8` 다운받음.
  - 비번 `1234` port `5432`
  - Locale `Korean, korea` set
  - `stackbuilder`로 따로 더 설치할거 없으니 체크해제 후 완료처리.

+ `Docker` 위에서 시험하고 올리는게 정신건강에 좋겠다.
  - `phoenix` 프레임워크의 deprecated 사항 업데이트 등의 시험이 필수임
  - 윈도 디렉토리 경로만 바꿔도 서버 start 안됨


## 수행 요약

- https://hexdocs.pm/phoenix/up_and_running.html
+ 시작 전 setting
  - `플젝생성` : `$ mix phx.new hello`
  - `db 연결셋` : `config/dev.exs`
  - `db repo 생성` : `$ mix ecto.create`
  - deprecated 메세지 뜰 시 처리
+ 서버구동
  -  `$ mix phx.server`
+ web browser 결과 확인
+ shutdown
  - `Ctrl` + `c` 두번

---

### 플젝 생성
```
$ mix phx.new hello

...
We are almost there! The following steps are missing:

    $ cd hello

Then configure your database in config/dev.exs and run:

    $ mix ecto.create

Start your Phoenix app with:

    $ mix phx.server

You can also run your app inside IEx (Interactive Elixir) as:

    $ iex -S mix phx.server
```

### db 연결셋

- `config/dev.exs` 에서 db연결관련 정보 config 수정


### `Repo` 생성 

- `ecto`는 orm 역할 수행
- `$ mix ecto.create`

```
$ mix ecto.create
Compiling 13 files (.ex)
Generated hello app
The database for Hello.Repo has been created
```

+ 실패 메세지는 대부분 DB 권한 패스워드 문제
  - role, password, permission 등은 아래 `ecto.create` 가이드 참조
  - `db설정` : `config/dev.exs`
  - https://hexdocs.pm/phoenix/mix_tasks.html#mix-ecto-create



+ 아래와 같은 deprecated 메세지 뜰 수 있음.
```
warning: defining a Gettext backend by calling

    use Gettext, otp_app: ...

is deprecated. To define a backend, call:

    use Gettext.Backend, otp_app: :my_app

Then, instead of importing your backend, call this in your module:

    use Gettext, backend: MyApp.Gettext

  lib/jwtdb_web/gettext.ex:23: JwtdbWeb.Gettext (module)
```

- 아래 코드처럼 고친 뒤 서버 구동  

![](https://blog.kakaocdn.net/dn/bpwHSi/btsJbglxNhu/YrIX1rcLU6UKuNEPzRJLcK/img.png)



### 서버구동

```
$ mix phx.server

Compiling 2 files (.ex)
Compiling 4 files (.ex)
[info] Running JwtdbWeb.Endpoint with Bandit 1.5.7 at 127.0.0.1:4000 (http)
[info] Access JwtdbWeb.Endpoint at http://localhost:4000
[watch] build finished, watching for changes...

Rebuilding...

Done in 379ms.
```

+ `windows에서 구동 시` : 관리자 권한으로 cmd 띄워서 돌려야 함, phoenix가 `symlink 생성` 안된다고 warning 띄우고 이때문에 empty asset response 날릴 수 있다고 경고.
```
  C:phoenixplayground\hello>mix phx.server
[warning] Phoenix is unable to create symlinks. Phoenix' code reloader will run considerably faster if symlinks are allowed. On Windows, the lack of symlinks may even cause empty assets to be served. Luckily, you can address this issue by starting your Windows terminal at least once with "Run as Administrator" and then running your Phoenix application.
  ```

## web browser 결과

![](https://blog.kakaocdn.net/dn/bzLI1M/btsJcPti8Pq/4mTcsg92vkRkki0aGUMLK1/img.png)

  
### shutdown

- `ctrl+c` 두번

## 참고

- https://hexdocs.pm/phoenix/up_and_running.html