---
title: oracle 19c install 오라클 19c 설치
date: 2024-04-01 11:30:00 +0900
categories: [ORACLE, TUNING]
tags: [oracle, db, tuning]  # TAG names should always be lowercase
authors: [gonnichiwa]
---

## install 환경
windows 10 pro x64  

## oracle 19c 다운로드
- https://www.oracle.com/kr/database/technologies/oracle19c-windows-downloads.html  
- 계정 로그인 or 생성하여 진행(무료)  
![](https://blog.kakaocdn.net/dn/c0VUVd/btsGeu7TfDI/hje2ju1AaoJzJL5yXzZlj0/img.png)

## 설치 실행
Users/downloads/ 다운받은 zip 압축풀고  
C:\로 옮겨서 실행  
![](https://blog.kakaocdn.net/dn/l7hax/btsGesCexMv/seBjdnuQxkjixalNHEtbk1/img.png)  

## 설치 수행 과정
![](https://blog.kakaocdn.net/dn/cSAM9z/btsGfwRqPls/rF1N9TcPuroEEgdHslMh10/img.png)
단일 인스턴스 데이터베이스 생성 및 구성  

![](https://blog.kakaocdn.net/dn/65BeD/btsGeSgdOeq/8Doqeu0xnIhk4Lk2EPxGHK/img.png)
서버 클래스  
- 네트워크(wifi내부망 포함) 


![](https://blog.kakaocdn.net/dn/dkAGYC/btsGe8wwGeK/Rdqs85yRV654OjoW6m1Kpk/img.png)
표준 설치  

![](https://blog.kakaocdn.net/dn/botOCA/btsGfgBdrz8/rjcNQWCkHbHGWVeFEuvwwk/img.png)
가상 계정 사용  

![](https://blog.kakaocdn.net/dn/bqxS4w/btsGgbsLOYH/KG7ZWDlCrfjhKqs9HxmT0k/img.png)
비번설정 
- `INS-30011` : 대문자&소문자&숫자 포함 구성 필.  
- 보통 테스트/개발용으로 쓰는 db는 비번 까먹기 쉬워 메모함  

![](https://blog.kakaocdn.net/dn/bXCihr/btsGgbfd1Vr/obKZvFOJa3L18fJXC9Bj4k/img.png)
[INS-32014] 지정된 Oracle 기본 위치(C:\)이(가) 부적합합니다.
[INS-32056] 지정한 Oracle 기본 위치에 기존 중앙 인벤토리 위치 C:\Program Files\Oracle\Inventory이(가) 포함되어 있습니다.
- virtualbox 같은 oracle 제품 미리 설치되어 있는 경우 뜰 수 있음.  

![](https://blog.kakaocdn.net/dn/t6GF6/btsGfJC6isc/A1taYdmI0KiPLUQUqUW1Xk/img.png)
- C:\oracle19c로 디렉토리 생성함.  

![](https://blog.kakaocdn.net/dn/bFxCqc/btsGewLlQKV/HlgZ42ARhnSZxrg8eqcBEk/img.png)
다음 눌러 설치 시작  

![](https://blog.kakaocdn.net/dn/bDGfxQ/btsGe7dg3So/u4IQdVKT5TjdwEDzwPi3TK/img.png)
진행 중  

![](https://blog.kakaocdn.net/dn/miZm7/btsGdS9pFcd/i25AUkxDbEeMnp9eqLfVKK/img.png)
엑세스 허용  

![](https://blog.kakaocdn.net/dn/qAmah/btsGeK3JiaV/sLqsvHF25nb2cOjtvpp0ok/img.png)
시간 좀 걸림.......
i7-10700K, 32G 기준
30분정도 걸린듯.

![](https://blog.kakaocdn.net/dn/bzWbDP/btsGhnl3TLo/BOeLAB1y3gF30b5b36CDm0/img.png)
성공

## 접속 및 수행 테스트  
몇가지 db client 툴로 접속 시험한다.

### cmd 접속 시험
---
cmd 로 접속
```
\>sqlplus
```
![](https://blog.kakaocdn.net/dn/EwQeP/btsGfwDUzm3/OOGcAQ8NMTmG2vTSZzkT6k/img.png)
사용자 : system 
비번 : 메모해둔비번 (타이핑해도 입력안보이므로 그대로 치고 들어간다.)

<br/>
<br/>

### oracle sql developer 접속 시험
---
[sql developer download](https://www.oracle.com/database/sqldeveloper/technologies/download/) 받아 그대로 실행  
<br/>
![](https://blog.kakaocdn.net/dn/Q0dnN/btsGfxQobfs/7KFKTY3nCps0vAKaV3z8m1/img.png)  
주로 쓰고 있는 JDK 버전이 따로 있을 수 있으므로(21버전 주로 쓰는 spring boot 라던지..) JDK Included 버전을 다운 받음.


![](https://blog.kakaocdn.net/dn/NVyAQ/btsGex4vSAI/MzUfmgcVmzBFvHEt0DvPx1/img.png)  
새접속 눌러서

![](https://blog.kakaocdn.net/dn/uxRMN/btsGhmtVC0B/rMP3Fhu781DBaabIEE6Vr0/img.png)
위 설치과정대로 수행했을때  
SID 기본값 orcl  
사용자 이름은 system
하단 `테스트` 눌러서 좌하단 `상태:성공` 확인


### dbeaver 접속 시험
---
![](https://blog.kakaocdn.net/dn/yd1GT/btsGeQJxLpc/DLPziu0vrzGonCyxiA80l0/img.png)

![](https://blog.kakaocdn.net/dn/boeUEv/btsGeht6AkC/6zeitX4J6sxMWtkKLIqLw0/img.png)
dbeaver 로 오라클 접속 처음 생성하는 경우 관련 driver 다운로드 필요함  
회사 내부망이거나 해서 다운받기 어려운 환경이면  
Driver 구해서 따로 경로 세팅 해줄 수 있음. Driver Settings..

![](https://blog.kakaocdn.net/dn/bi9uA2/btsGfKaYX4B/fulMV1zsLwCObFj6VMGduK/img.png)
커넥션 성공


#### 참고
https://javacoding.tistory.com/113  