---
title: oracle tuning essential 실습환경 set
date: 2024-04-01 07:00:00 +0900
categories: [ORACLE, TUNING]
tags: [oracle, db, tuning]  # TAG names should always be lowercase
authors: [gonnichiwa]
---

## 작성 목적
- [오라클 튜닝 에센셜](https://www.yes24.com/Product/Goods/85117060) 의 실습환경 셋을 기록함.
- 14p ~ 27p, 1장 실습환경.
- [실습 관련 실행 파일 다운로드](https://cafe.naver.com/dbstudydapsqlp)

## 사전 준비
- [oracle 19c 설치](https://gonnichiwa.github.io/posts/oracle-19c-install/)
- [oracle sql developer](https://gonnichiwa.github.io/posts/oracle-19c-install/#oracle-sql-developer-%EC%A0%91%EC%86%8D-%EC%8B%9C%ED%97%98)
- 접속 가능한 `sys` 계정

## 스크립트 실행
0. `sys`계정으로 로그인

### 1.sql
스크립트 주석 풀고 수행한다.  
안하면 차후 계정생성할때 `ORA-65096` 만남
```
-- 12c 이후 버전
ALTER SESSION SET "_ORACLE_SCRIPT"=TRUE;            -- 기존 방식으로 계정생성(12c 이상 버전에서 주석 풀고 실행)
```
아니면 계정 생성할때 아래처럼 계정명prefix로 `C##` 붙여서 쿼리수행
```
CREATE USER "C##asdf" IDENTIFIED BY "1234";
```

.DBF OS 파일시스템에서 삭제했다면 ORACLE이 참조하는 파일 읽어들일 수 없어  
`socket failed..` err메세지나 `ORA-01157`, `ORA-01110`, `database not open` 등의 에러 뿜는다.  

```sql
alter database open;
alter database datafile 'C:\DEV\ORACEL-TUNING-ESSENTIAL\ORADATA\MY_DATA01.DBF' offline drop;
```
![](https://blog.kakaocdn.net/dn/JfwDU/btsGfguHwuc/wQ0VEbBH1kPT8jnrXTPtyK/img.png)  
...
하고 서비스 재시작
[참고](https://blog.naver.com/v_lovepooh_v/20208505601)  

테이블스페이스(참조파일정보포함) 삭제하고 1.sql대로 다시 생성.
```sql
drop tablespace MY_DATA including contents and datafiles;
```

```sql
-- 테이블 스페이스 생성
CREATE TABLESPACE MY_DATA DATAFILE 'D:\ORADATA\MY_DATA01.dbf' SIZE 30G AUTOEXTEND ON;
ALTER TABLESPACE MY_DATA ADD DATAFILE 'D:\ORADATA\MY_DATA02.dbf' SIZE 30G AUTOEXTEND ON; --  NEXT 50M MAXSIZE 2048M;
ALTER TABLESPACE MY_DATA ADD DATAFILE 'D:\ORADATA\MY_DATA03.dbf' SIZE 30G AUTOEXTEND ON; --  NEXT 50M MAXSIZE 2048M;
```


### 2.sql

insert문들에 한글 깨져서 나오는 현상이 있는데  
아래처럼 sql developer 환경설정 `UTF-8`로 고친 후  
sql developer 재시작 하여 파일 다시 띄운다.
![](https://blog.kakaocdn.net/dn/bwfsQ3/btsGh5FxnN9/kyxYiuqYMKu5ATJ7YLo930/img.png)  
![](https://blog.kakaocdn.net/dn/GxzeX/btsGffo5Mod/ZoIkIJDvS1NRXaXvDdIzzk/img.png)  

### 3.sql

프로시저 생성부 (랜덤데이터 생성로직)  
쿼리 수행 결과 이상 없음.

### 4.sql  

1500만건 데이터 생성 하므로 시간좀 걸린다.  
이외는 쿼리 실행시 특이사항 없었음.

