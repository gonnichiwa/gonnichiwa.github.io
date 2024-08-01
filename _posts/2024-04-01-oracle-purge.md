---
title: oracle - purge
date: 2024-04-01 07:00:00 +0900
categories: [ORACLE]
tags: [oracle, keyword]  # TAG names should always be lowercase
authors: [gonnichiwa]
---

## [oracle] PURGE

* 테이블 삭제 시  
drop table 테이블명;

* 테이블 완전 삭제하기 (휴지통에 저장되지 않음, **FLASHBACK으로 복구 불가**)  
DROP TABLE 테이블명 purge; 

* 휴지통 비우기  
purge recyclebin;

* 휴지통에 있는 테이블 복원  
FLASHBACK TABLE 테이블명 TO BEFORE DROP

출처: https://sfeg.tistory.com/342
