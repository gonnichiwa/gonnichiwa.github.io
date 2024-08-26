---
title: postgres-0-install-and-run-mac
date: 2024-08-26 12:11:00 +0900
categories: [Postgres]
tags: [postgres]  # TAG names should always be lowercase
authors: [gonnichiwa]
---

## 작성목적

- postgres install과 제어 메모


### 설치 

https://wiki.postgresql.org/wiki/Homebrew

```sh
$ brew install postgresql
```


### 서비스 시작

```sh
$ brew services start postgresql
```


### 서비스 확인

```sh
$ brew services list

postgresql@14 started jae-hunjeong ~/Library/LaunchAgents/homebrew.mxcl.postgresql@14.plist
```

### superuser 생성

```sh
$ psql postgres

postgres=# create role postgres with login password '1234';
CREATE ROLE

postgres=# alter user postgres with superuser;
ALTER ROLE

postgres=# \q

$
```

#### 확인

- dbeaver에서 conn test함



### 서비스 종료

- (`brew services list`로 확인한 `postgresql@14`)
```sh
$ brew services stop postgresql@14
postgresql@14 none

$ brew services list
postgresql@14 none
```


### 서비스 삭제 on mac

```sh
$ brew services stop postgresql@15
$ brew uninstall --force postgresql@15
$ rm -rf $(brew --prefix)/var/postgresql@15
```

