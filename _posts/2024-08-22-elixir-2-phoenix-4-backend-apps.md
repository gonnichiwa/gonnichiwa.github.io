---
title: elixir-2-phoenix-4-backend-apps
date: 2024-08-23 12:11:00 +0900
categories: [Elixir]
tags: [phoenix]  # TAG names should always be lowercase
authors: [gonnichiwa]
---

## 0. 작성목적

 - elixir umbrella 프로젝트로 `app` 프로젝트 생성
 - db 연동하여 jwt 인증 서버 생성 목적 계속 진행

## REQUIRE

- erlang/OTP `27`
- elixir `1.17.2`

+ Phoenix db연동해야하니 돌려볼려면 `PostgresSQL` 이 필요함
  - https://www.postgresql.org/download/windows/
  - 여기서는 `15.8` 버전 사용함.
  - id `postgres` 비번 `1234` port `5432`
  - Locale `Korean, korea` set
  - `stackbuilder`로 따로 더 설치할거 없으니 체크해제 후 완료처리.

- windows 수행 기준 작성

## 수행 요약

+ `umbrella` project 생성
  - \\> `mix new jbackend --umbrella`

- \\> `cd jbackend`
- \jbackend> `cd apps`
- \jbackend\apps> `mix phx.new users_api`
- \jbackend\apps> `cd users_api`
- \jbackend\apps\users_api> `mix phx.gen.auth Accounts User users`

+ db 접속정보 수정 필요
  - `\jbackend\config\dev.exs`
  - `\jbackend\config\test.exs`

- \jbackend\apps\users_api> `mix ecto.migrate`

- \jbackend\apps\users_api> `mix deps.get`

- \jbackend\apps\users_api> `mix test`

+ routes 확인
  - \\jbackend\apps\users_api\\> `mix phx.routes`

+ 서버 시작
  - \\jbackend\apps> `mix phx.server`

+ 브라우저 켜고 접속 `localhost:4000/users/register` 
  - id : 맘대로 이메일주소
  + pw : `iwannabeelixirdeveloper`  
    - 외 아무거나 12자 이상



## 수행상세

+ `umbrella` project 생성
  - \\> `mix new jbackend --umbrella`
```elixir
\> mix new jbackend --umbrella
* creating README.md
* creating .formatter.exs
* creating .gitignore
* creating mix.exs
* creating apps
* creating config
* creating config/config.exs

Your umbrella project was created successfully.
Inside your project, you will find an apps/ directory
where you can create and host many apps:

    cd jbackend
    cd apps
    mix new my_app

Commands like "mix compile" and "mix test" when executed
in the umbrella project root will automatically run
for each application in the apps/ directory.
```

- \\> `cd jbackend`
- \jbackend\\> `cd apps`

```
C:\dev\elixir\phoenixplayground>cd jbackend
C:\dev\elixir\phoenixplayground\jbackend>cd apps
```

- \jbackend\apps\\> `mix phx.new users_api`
```
C:\dev\elixir\phoenixplayground\jbackend\apps>mix phx.new users_api

* creating users_api/lib/users_api/application.ex
...
* creating users_api/assets/tailwind.config.js

Fetch and install dependencies? [Yn] y

* running mix deps.get
* running mix assets.setup
* running mix deps.compile

We are almost there! The following steps are missing:

    $ cd users_api

Then configure your database in config/dev.exs and run:

    $ mix ecto.create

Start your Phoenix app with:

    $ mix phx.server

You can also run your app inside IEx (Interactive Elixir) as:

    $ iex -S mix phx.server
```

- \jbackend\apps\\> `cd users_api`
- \jbackend\apps\users_api\> `mix phx.gen.auth Accounts User users`
```
C:\dev\elixir\phoenixplayground\jbackend\apps\users_api>mix phx.gen.auth Accounts User users
An authentication system can be created in two different ways:
- Using Phoenix.LiveView (default)
- Using Phoenix.Controller only
Do you want to create a LiveView based authentication system? [Yn] Y
Compiling 15 files (.ex)
...
Please re-fetch your dependencies with the following command:

    $ mix deps.get

Remember to update your repository by running migrations:

    $ mix ecto.migrate

Once you are ready, visit "/users/register"
to create your account and then access "/dev/mailbox" to
see the account confirmation email.
```

+ \jbackend\apps\users_api\> `mix ecto.migrate` 하면 에러 뜰것임.
  - db 접속정보 수정 필요
  - `\jbackend\config\dev.exs`
  - `\jbackend\config\test.exs`

본 포스트 최상단 `REQUIRE` 설치내용 기준 아래처럼 수정

![](https://blog.kakaocdn.net/dn/lTJOY/btsJfS4z2rS/OOaevgsJmLk5sEpUijAjWk/img.png)


- 다시 \\> `mix ecto.migrate`
```
C:...\users_api>mix ecto.migrate
Compiling 28 files (.ex)
...
Generated users_api app

18:10:28.013 [info] == Running 20240825090614 UsersApi.Repo.Migrations.CreateUsersAuthTables.change/0 forward
18:10:28.015 [info] execute "CREATE EXTENSION IF NOT EXISTS citext"
18:10:28.051 [info] create table users
...
18:10:28.079 [info] == Migrated 20240825090614 in 0.0s
```

- **db 테이블 생성 확인 (dbeaver tool로 봄)**

![](https://blog.kakaocdn.net/dn/cK1Utb/btsJfQTemUG/7xkpghj3AWN6kSLAU47IcK/img.png)

  - \\> `mix deps.get`
  - 의존 빌드 역할 수행
  - `mix ecto.migrate` 전에 하는게 안전함
```
C:...\users_api>mix deps.get
Resolving Hex dependencies...
Resolution completed in 0.17s
Unchanged:
  bandit 1.5.7
  castore 1.0.8
...
All dependencies are up to date
```

- \\> `mix test`

```
주절주절 성공
```

- 아래 같은 warning 메세지 거슬리니 처리해줌
```
warning: defining a Gettext backend by calling
    use Gettext, otp_app: ...
is deprecated. To define a backend, call:
    use Gettext.Backend, otp_app: :my_app
Then, instead of importing your backend, call this in your module:
    use Gettext, backend: MyApp.Gettext
  lib/users_api_web/gettext.ex:23: UsersApiWeb.Gettext (module)
```

![](https://blog.kakaocdn.net/dn/ClTFo/btsJgcogErM/WRnzh2WU5WTQCsR0fDIcBk/img.png)


## 돌려보자

+ routes 확인
  - \\jbackend\apps\users_api\\> `mix phx.routes`

```
  GET     /                                      UsersApiWeb.PageController :home
  GET     /dev/dashboard/css-:md5                Phoenix.LiveDashboard.Assets :css
  GET     /dev/dashboard/js-:md5                 Phoenix.LiveDashboard.Assets :js
  GET     /dev/dashboard                         Phoenix.LiveDashboard.PageLive :home
  GET     /dev/dashboard/:page                   Phoenix.LiveDashboard.PageLive :page
  GET     /dev/dashboard/:node/:page             Phoenix.LiveDashboard.PageLive :page
  *       /dev/mailbox                           Plug.Swoosh.MailboxPreview []
  GET     /users/register                        UsersApiWeb.UserRegistrationLive :new
  GET     /users/log_in                          UsersApiWeb.UserLoginLive :new
  GET     /users/reset_password                  UsersApiWeb.UserForgotPasswordLive :new
  GET     /users/reset_password/:token           UsersApiWeb.UserResetPasswordLive :edit
  POST    /users/log_in                          UsersApiWeb.UserSessionController :create
  GET     /users/settings                        UsersApiWeb.UserSettingsLive :edit  GET     /users/settings/confirm_email/:token   UsersApiWeb.UserSettingsLive :confirm_email
  DELETE  /users/log_out                         UsersApiWeb.UserSessionController :delete
  GET     /users/confirm/:token                  UsersApiWeb.UserConfirmationLive :edit
  GET     /users/confirm                         UsersApiWeb.UserConfirmationInstructionsLive :new
  WS      /live/websocket                        Phoenix.LiveView.Socket
  GET     /live/longpoll                         Phoenix.LiveView.Socket
  POST    /live/longpoll                         Phoenix.LiveView.Socket
```

+ 서버 시작
  - \\> `mix phx.server`
  - 아래 `warning`은 관리자모드 실행 필요 메세지 (권장)
  - 루트인 `jbackend` 에서 커맨드 실행시 모든 app 동작
```
[info] Running UsersApiWeb.Endpoint with Bandit 1.5.7 at 127.0.0.1:4000 (http)
[info] Access UsersApiWeb.Endpoint at http://localhost:4000
[watch] build finished, watching for changes...
```

+ 브라우저 켜고 접속 `localhost:4000/users/register` 
  - id : 맘대로 이메일주소
  - pw : 나는 `iwannabeelixirdeveloper`  

![](https://blog.kakaocdn.net/dn/bnYSpx/btsJdJaI7Kc/dvdTF5h8hfxKGs62UNPrl1/img.png)

- 잘뜬다

![](https://blog.kakaocdn.net/dn/8aMu8/btsJeDOnFKl/m9FPbKcLKkwpNNJNH7ryvk/img.png)


## 참고

https://elixirschool.com/ko/lessons/advanced/umbrella_projects
https://hexdocs.pm/phoenix/mix_phx_gen_auth.html
https://hexdocs.pm/phoenix/api_authentication.html