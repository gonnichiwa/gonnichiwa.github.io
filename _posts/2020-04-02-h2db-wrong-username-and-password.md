---
title: h2 db의 삭제 후 찌꺼기들 ':' 재설치 후 실행 시 wrong username and password
date: 2020-04-02 10:44:00 +0900
categories: [DB]
tags: [db,h2,error,90146-199,wrong-username-and-password]     # TAG names should always be lowercase
authors: [gonnichiwa]
---

## 1 문제
- h2 설치 후 `sa`(root) 계정에서 `test` 계정 만든 뒤

  `test`로 로그인 시도 했으나 아래 에러메세지로 로그인 실패함.

- `Database "C:/Users/USER/test" not found, and IFEXISTS=true, so we cant auto-create it [90146-199]`


## 2 원인
- h2는 파일db 이다.
- USER/.h2.server.properties 파일 : h2 접속시 설정 정보들 저장
- 아래 test.mv.db, test.trace.db 파일은 test 스키마 자체에 속한 객체(테이블, 데이터..) 저장파일.

![1](https://onedrive.live.com/embed?resid=89DC3AD954E2A8C2%2116388&authkey=%21AN094XMBMzLWoC0&width=591&height=30 "h2.server.properties")

![2](https://onedrive.live.com/embed?resid=89DC3AD954E2A8C2%2116387&authkey=%21ACqpxZjkK0JBx7s&width=602&height=73)

h2 설치했다가 언인스톨 해도 위 파일들은 그대로 남아있어서
**같이 지워줘야** 말끔하게 삭제된다.


## 3 조치

`2 원인` 항목 이미지에서 선택된 파일들 모두 삭제.  
재기동

## 결과

로그인 성공.