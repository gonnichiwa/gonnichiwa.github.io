---
title: mariadb set user privileges (mariadb 유저 권한 핸들링)
date: 2024-03-29 23:52:00 +0900
categories: [MAC, MYSQL, MARIADB]
tags: [mysql, mariadb, mac]  # TAG names should always be lowercase
authors: [gonnichiwa]
---


>**root계정초기생성**  
>생성전 비번은 [blank]임  
>set password for 'root'@'localhost'=password('newpassword');

>버전확인  
>select VERSION();  

>**유저들 보기**  
>use mysql;  
>select * from USER;

>**유저생성**  
>CREATE USER username@'%' IDENTIFIED BY 'password';  

>**유저 에게 모든 권한 주기**  
>GRANT ALL PRIVILEGES ON dbname.* to username@'%';  
> \'%\'는 localhost 이외 ip 에서 접근 시  
> \'localhost\'는 localhost에서 접근 시  

>**유저 에게 특정 권한 주기**  
>GRANT SELECT, INSERT ON dbname.* to username@'%';  
>  
>**특정 테이블 권한**  
>GRANT UPDATE(col1,col2) ON dbname.tablename to username@'%';  

>(mysql한정)  
>nodejs, java등 was app의 db driver plugin caching_sha2_password 문제 시  
>유저 테이블 plugin `caching_sha2_password`을  `mysql_native_password`로 바꿔줌.  
>ALTER USER 'username'@'localhost' IDENTIFIED WITH mysql_native_password BY 'password';

>**유저 권한 보기**  
SHOW GRANTS FOR username;  

>**권한 변경사항 적용**  
FLUSH PRIVILEGES;  

>**유저삭제**  
DROP USER aidaboat@'%';  
DROP USER aidaboat@'localhost';

>**transaction_isolation 확인**  
>`mariadb` : `transaction_isolation`, `tx_isolation` 두개 있음.  
>`mysql` : `transaction_isolation`만 있음.  
> 그래서? jdbc driver 연결 시 구분됨.  
>show global variables like 't%isolation';  
>show global variables like 'tx%';  

