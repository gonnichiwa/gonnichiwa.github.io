---
title: tar-gz-xz-handling
date: 2024-04-06 20:07:00 +0900
categories: [LINUX]
tags: [linux, tar, gz, xz]  # TAG names should always be lowercase
authors: [gonnichiwa]
---

예시는 /var/log/* 안의 파일들을 예시로 하였다.


### 요약
#### tar.xz 압축하기
```bash
$ tar Jcvf [압축파일명] [압축할대상]
$ tar Jcvf aa.tar.xz /var/log/*
```
#### tar.xz 압축풀기
```bash
$ tar Jxvf [압축파일명]
$ tar Jxvf var_log.tar.xz 
```

### tar.gz 압축하기
```bash
$ tar zcvf [압축파일명] [대상]
$ tar zcvf var_log.tar.gz /var/log/* 
```

#### tar.gz 압축풀기
```bash
$ tar zxvf [압축된 tar.gz 혹은 tgz 파일]
$ tar zxvf var_log.tar.gz
$ tar zxvf var_log.tgz
```

<br/>

---

<br/>


### .tar 아카이브 파일 생성
```bash
$ tar cvf [압축될 파일 이름] [압축할 대상] ( tar Compress,Verbose, File)
$ tar cvf archieve.tar /var/log/*
``` 

### .tar 아카이브 파일 해제
```bash
$ tar xvf [tar 파일] (tar eXtract Verbose File)
$ tar xvf archieve.tar
```
 
### .gzip 으로 압축하기
```bash
$ gzip [압축할 대상] 
$ gzip archieve.tar
```

### .gzip 압축 풀기
```bash
$ gzip [압축된 gz] 
$ gzip -d archieve.tar.gz 
$ gunzip [압축된 gz] 
$ gunzip archieve.tar.gz
```

### .xz 압축 하기
```bash
$ xz [압축할 파일] 
$ xz archieve.tar
```

### .xz 압축 풀기
```bash
$ xz -d [xz로 압축된 파일] 
$ xz -d archieve.tar.xz 
```

```bash
$ unxz [xz로 압축된 파일]
$ unxz archieve.tar.xz
```

> 이때 tar에서 아카이브+압축 과정을 한번에 할 수 있다.

### .tar.gz 혹은 .tgz 파일로 압축하기
```bash
$ tar zcvf [압축할 파일 이름] [대상]
$ tar zcvf var_log.tar.gz /var/log/* 
$ tar zcvf var_log.tgz /var/log/*
```
.tar.gz 혹은 .tgz 로 확장자를 한번에 주고 뺄 수 있다.
<br/>

### .tar.gz 압축 풀기
```bash
$ tar zxvf [압축된 tar.gz 혹은 tgz 파일]
$ tar zxvf var_log.tar.gz
$ tar zxvf var_log.tgz
```
 
### .tar.xz 혹은 .txz 로 압축하기
```
$ tar Jcvf [압축할 파일 이름] [대상] 
$ tar Jcvf var_log.tar.xz /var/log/* 
$ tar Jcvf var_log.txz /var/log/*
```
 
### .tar.xz 혹은 .txz 압축 풀기
```
$ tar Jxvf [압축된 tar.xz 혹은 txz 파일] 
$ tar Jxvf var_log.tar.xz 
$ tar Jxvf var_log.txz
```
 
### tar 주요 옵션 정리
z or J : z는 gzip 사용, J는 xz 사용  
c or x : 압축(c) 또는 해제(x). 둘 중 하나만 사용할 수 있다.  
v : verbose, 화면에 과정을 출력  
f : 파일 이름 지정 옵션
p : 권한을 지정해서 아카이브한다. 권한 지정하지 않을 시 실행자의 소유권으로 생성  

