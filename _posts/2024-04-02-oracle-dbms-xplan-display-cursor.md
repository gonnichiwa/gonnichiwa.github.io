---
title: dbms_xplan.display_cursor 실행해보기
date: 2024-04-02 17:00:00 +0900
categories: [ORACLE, TUNING]
tags: [oracle, db, tuning, dbms_xpan]  # TAG names should always be lowercase
authors: [gonnichiwa]
---

## 대체할 방법은 없나?
- `10046 event trace` 있으나
- 사용하기 위해선 `FTP`권한 가진 OS 계정이 필요함.
- DBA아닌 이상 현장에서 계정 권한을 줄까? 안전상 어렵다.
  

## 왜 써야 하나?
+ `내부 힌트` 제공
  - 내부 힌트란 오라클과 사용자가 부여한 힌트를 알 수 있음
  - Predicate Information : 조인조건, 엑세스 조건, 필터 조건의 구분
  - 각자의 출력 포맷 설정 가능.


## 사용하기 위해선
#### 대상 테이블 `읽기 권한` 확인
+ 아래 `읽기 권한` 필요.
```sql
select * from V$SQL;
select * from V$SESSION;
select * from V$SQL_PLAN;
select * from V$SQL_PLAN_STATISTICS_ALL;
```
시노님(SYNONYM) 으로 뷰와 연결되어 있음.  
V$SQL -> V_$SQL  
V$SESSION -> V_$SESSION  ...  

#### `19c` 기준 뷰테이블로 정의되어 있음.
```sql
SELECT * 
FROM USER_VIEWS 
WHERE VIEW_NAME 
IN ('V_$SQL','V_$SESSION','V_$SQL_PLAN','V_$SQL_PLAN_STATISTICS_ALL');
```

#### 뷰 권한 부여  (SYS계정필요)
<!--[유저생성과 권한주기]()-->
```sql
grant select on V_$SQL to username;
grant select on V_$SESSION to username;
grant select on V_$SQL_PLAN to username;
grant select on V_$SQL_PLAN_STATISTICS_ALL to username;
```
<br/>

## 사용법

### sql developer 사용한다.

### 쿼리 실행 전 SET
```sql
ALTER SESSION SET STATISTICS_LEVEL = ALL;
```
세션 유지하는 동안 실행한 쿼리의 `실제 실행계획`을 볼 수 있음.  

OR  

_위 SESSION SET 쿼리 쓰지 않고_
실행계획 뜨고 싶은 쿼리에 아래 힌트 추가 해서 쿼리 실행.  
`/*+ gather_plan_statistics */`  
예시)
```sql
SELECT /*+ gather_plan_statistics */
    *
FROM (
	SELECT *
	FROM EMP
	WHERE DEPTNO = 30
	ORDER BY ENAME
)
WHERE ROWNUM <= 3;
```
[emp 데이터 쿼리(ddl,dml)는 여기](https://gonnichiwa.github.io/posts/oracle-emp-dept-datas/)

>**주의 : 쿼리 수행결과 데이터를 모두 FETCH 해야 한다.**  
>**쿼리 결과 전체 데이터count는 1000인데 DB client tool 에서 초기 50건만 가져오게 셋 되어있으면 XPLAN 뜨기 불가할 수 있음**

>**Dbeaver Community 수행결과 cannot fetch plan for ... 메세지 띄우며 수행 불가할 수 있음**  
>**Dbeaver pro버전 필요한 기능으로 보이므로 Oracle SQL Developer 사용해야함.**  
>**DBEAVER PRO 유료이나 .ac.kr 이메일 도메인 있으면 라이센스 발급이 가능함.**

```sql
SELECT * FROM 
TABLE(DBMS_XPLAN.DISPLAY_CURSOR(SQL_ID,CHILD_NUMBER,'ADVANCED ALLSTATS LAST'));
```
- `SQL_ID` : 실행 쿼리의 SQL_ID, 
- `CHILD_NUMBER` : 실행 쿼리 분석하여 옵티마이저가 부여하는 값으로 보임  

  
---
  
#### _가장 최근 쿼리 실행_

```sql
SELECT * FROM 
TABLE(DBMS_XPLAN.DISPLAY_CURSOR(NULL,NULL,'ADVANCED ALLSTATS LAST'));
```
  
---
  
#### _실행했던 쿼리의 SQL_ID, CHILD_NUMBER 찾기 (수행 쿼리 기준 like 검색)_

```sql
select 
  SQL_TEXT, SQL_ID, CHILD_NUMBER 
from V$SQL 
where SQL_TEXT like '%WHERE DEPTNO = 30%';
```

- 결과 예시  

|SQL_TEXT                                                                                   |SQL_ID       |CHILD_NUMBER|
|-------------------------------------------------------------------------------------------|-------------|------------|
|SELECT * FROM (  SELECT *  FROM EMP  WHERE DEPTNO = 30  ORDER BY ENAME ) WHERE ROWNUM <= 4 |cwtr8bacds25h|           0|
SELECT * FROM (  SELECT *  FROM EMP  WHERE DEPTNO = 30  ORDER BY ENAME ) WHERE ROWNUM <= 3  |56782qauc6jyt|           1|
|SELECT  * FROM (  SELECT *  FROM EMP  WHERE DEPTNO = 30  ORDER BY ENAME ) WHERE ROWNUM <= 3|43shzdff4njyg|           0|

<br/>

---

<br/>








