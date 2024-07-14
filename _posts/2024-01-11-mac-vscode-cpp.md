---
title: C++ debugging 환경 ON VSCODE, MAC
date: 2024-01-11 16:53:00 +0900
categories: [CPP, MAC, ENV]
tags: [cpp, mac, env, debugging]     # TAG names should always be lowercase
authors: [gonnichiwa]
---

## 0 목적

Mac 환경의 vscode에서 c++ 코딩 환경을 셋업한다.

## 1. Xcode 버전 최신화 (업데이트)
> `appstore`에서 xcode 버전 업데이트 OR xcode 설치   
>> `목적` : 2. 에서 확인할 lldb, clang을 함께 설치하기 위함.

## 2. terminal에서 lldb, clang 설치확인

[mac에서 lldb, clang 설치하기](링크)

```sh
$ lldb --version
$ clang --version
```

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2F8clGB%2FbtsDmqNgPql%2FkfkdWLXj4xK4JbRV1fKBZ0%2Fimg.png)

## 3. vscode에서 c++ 컴파일러 버전 셋업  
> (출처 참고 : https://areumdawoon.tistory.com/20)

### 3.1 extention 설치

#### 3.1.1 c++ (microsoft)

#### 3.1.2 code runner 설치 (컴파일, 디버깅 편의 제공 툴)

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbQWZrO%2FbtsDnxZrpYS%2FEtNN6IuhbGBq97dS0tGk9k%2Fimg.png)

- 확장 설정 (`1`OR `2` 수행)
> 1
![1](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FdKo6vn%2FbtsDjXyzlIl%2FgMyKbt4uBORW1kqKkjUcZk%2Fimg.png)
> 2
![2](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FdZklWG%2FbtsDlnXCR2A%2F9TWKdeoJSA1Euvuk7ENy51%2Fimg.png)

### 3.2 디버깅 환경 설정


#### 3.2.1 task.json 셋업

### 3.3 코드 실행
#### 3.3.1 바로 코드 실행
#### 3.3.2 브레이크포인트와 디버거 실행