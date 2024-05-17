---
title: react debugger on vscode 에서 리액트 디버깅
date: 2024-05-17 16:33:33 +0900
categories: [REACT]
tags: [js, react, debugging]  # TAG names should always be lowercase
authors: [gonnichiwa]
---

## 작성목적

`yarn start`로 구동시킨 리액트 로컬소스의 디버깅 가이드

## 순서

1. react app 실행(`yarn start`, `npm start` 등)

2. 디버거 set  
![](https://blog.kakaocdn.net/dn/dgHG34/btsHsYmgK7Q/oTATmN8uQpOLHQmY5APQGK/img.gif)

3. breakpoint 찍고 동작시켜보기  
![](https://blog.kakaocdn.net/dn/Ju04B/btsHsm9ePi7/IBkQW7XeQo3zGeQ2peUhpk/img.png)

- 좌중단 조사식으로 코드 찍으면 console.log효과로 결과 알 수 있음.

## 조건부 breakpoint

- 조사식 true 걸리면 breakpoint 동작하도록 할 수 있음, 중단점 오른쪽 클릭하여 `중단점 편집...`  
![](https://blog.kakaocdn.net/dn/2k15m/btsHtJIVBBS/SzP54TqwlmpKUaiZ9QKlK1/img.png)  

- `inStockOnly`는 bool변수임, true일때 동작함.  
- **식 적고 엔터로 조건부 breakpoint 적용됨**  
![](https://blog.kakaocdn.net/dn/7P8Yj/btsHtrPknUk/KiCLYhXKKAYPKmczgpG1G0/img.png)


- inStockOnly = true 일 때 breakpoint 동작함을 알 수 있음.  
![](https://blog.kakaocdn.net/dn/cL95gH/btsHt3GUpRu/0SfdAN3h0uMldwZOSkXVA1/img.png)

## 참고

https://profy.dev/article/debug-react-vscode  
https://itchallenger.tistory.com/904  
