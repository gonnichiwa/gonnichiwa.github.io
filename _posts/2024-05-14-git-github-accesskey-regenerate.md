---
title: github access key regenerate 재발급과 적용 on mac
date: 2024-05-14 07:32:43 +0900
categories: [GITHUB]
tags: [github, accesskey, mac]  # TAG names should always be lowercase
authors: [gonnichiwa]
---

## 작성목적

- github access token regenerate 한 뒤 mac 적용 가이드 기록

## access key 재발급 github 에서

- github.com 진입, 로그인
- 계정 _settings_ 진입
![](https://blog.kakaocdn.net/dn/b0xDwN/btsHp3GGTUK/IIMm01sfio4QvhhkNebxr0/img.png)
- developer settings-personal access tokens(classic) 진입 [link](https://github.com/settings/tokens)
![](https://blog.kakaocdn.net/dn/q943L/btsHotT88XL/5963HtVlMfdo2d19Ny99j0/img.png)
- access token generate | regenerate 한뒤 나온 access token 메모장 복붙하여 저장

## mac에 access key 적용

+ `sourcetree`와 같은 툴에서 최초 push 시 등록하는 경우 있으나 
  - 기존 push 할때 썼던 access key가 만료되었을 경우 auth error 뱉고 push 불가할 수 있음.

- `command` + `space`, `키체인 관리` 진입
- `github`로 키체인 검색
- `access key` 포함된 항목 더블클릭
![](https://blog.kakaocdn.net/dn/bCh4n5/btsHpl2e4nb/9KUqvYTfto5KE0JkP5qYAk/img.png)

- 암호보기 하여 _메모해놓은 access key_ 넣고 변경사항 저장.
![](https://blog.kakaocdn.net/dn/pOrIy/btsHnue3zyo/ZjqC0PipDAWw6OsYlOnNOK/img.png)

- push 시험
