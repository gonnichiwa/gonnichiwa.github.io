---
title: macos 환경변수 제어
date: 2024-07-10 23:30:00 +0900
categories: [MACOS]
tags: [macos, path]  # TAG names should always be lowercase
authors: [gonnichiwa]
---

## macos 환경변수 추가하기

### 기본 셸 확인

```sh
 $ echo $SHELL
/usr/local/bin/zsh
```
- `zsh`이다.

<br/>

- `zsh`이면
```
$ vi ~/.zshrc
```

- `bash`이면
```
$ vi ~/.bashrc
```

+ 환경변수 `.rc`에 업데이트 (`zshrc`, `bashrc` 동일)
```
...
export TEST_PATH=gonnichiwa!!!!!
...
```
  - 이후 위 `export` 후 `:wq` (저장후 나가기)

<br/>

- 변경된 ~/.zshrc 환경변수 적용

```
$ source ~/.zshrc (~/.bashrc)
```

.. 실행중인 콘솔 프로세스 종료 후 재시작 하는게 안전하다.

- 적용 확인

```
 $ echo $TEST_PATH
gonnichiwa!!!!!
 $ $TEST_PATH 
 zsh: command not found: gonnichiwa!!!!!
```


#### 참고  
https://itvillage.tistory.com/47