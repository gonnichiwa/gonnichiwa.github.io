---
title: git windows crlf -> lf 로 변경 (delete 'cr' prettier)
date: 2024-06-11 19:22:03 +0900
categories: [GIT]
tags: [git, eol]  # TAG names should always be lowercase
authors: [gonnichiwa]
---

## 문제

- `mac`에서 작성한 `nestjs` 코드들 `windows`에서 `clone` 하여 `vscode` 열자마자 prettier관련 에러 발생.
- `delete 'cr' (prettier/...)` 하며 코드 라인 빨개짐..

## 원인

- 작성중인 `nestjs` app은 prettier 세팅이 되어있음

- `windows`에서 본 app 코드를 `clone` 받아오면서 코드라인을 `crlf` 처리하여 저장함.

- 머신에서 개발환경 함께 갖출 때, 코드 라인마다 들어가는 개행이 windows, mac/unix가 다름.

- `windows는 \r\n(CRLF)` 이고 `mac/unix는 \n(lf)`임

- windows 머신에서도 개발하므로, 머신간 `컨벤션 통일`을 위해

- production 환경은 windows 서버 쓰지 않는 이상 코드 개행은 `lf` 통일이 필요 할것임.


## 조치방법 검토

### vscode 에서 CRLF -> LF 변경
- `vscode`에서 아래 그림처럼 CRLF를 LF로 변경하면 prettier에서 감지 해제 되긴 함
![](https://blog.kakaocdn.net/dn/deanML/btsHU4sImY4/NLIHJ8sJvfCBRdN6GxFSz0/img.png)

+ `그러나` 전체 파일 다시 저장 후 커밋 필요함. 
  - 팀단위 작업 시 불필요한 혼란 발생 가능성 생길 수 있음.

### vscode settings.json

- 검색해보니 프로젝트 루트에 `settings.json` 넣으면 `vscode`로딩 중 본 파일 읽어들여 디폴트 세팅한다는데, 제대로 못본건지 반응없음.

- 그리고 프로젝트 루트에 파일 `settings.json` 파일이 추가되어야 하는게 맘에안듬.

### git crlf -> lf 변경

- 로컬 windows 머신의 git config 세팅만 바꾸고, 형상 변경 여지가 없으므로 시행함.

### 조치

#### clone default autocrlf false

```cmd
\> git config --global core.autocrlf false
\>
```

- 기존 `clone` 받은 `js` app 삭제 후 `다시 clone 받음`

#### vscode new file set lf 처리

- file -> settings -> preference
![](https://blog.kakaocdn.net/dn/bBRwvZ/btsHUp5t7QJ/NsVOkfhiqiYUjTIBrKejw0/img.png)

- `eol` 검색, `\n` 처리
![](https://blog.kakaocdn.net/dn/b4RCO1/btsHWd3jIXH/d2csktwqsdbwVjkW1CcfEk/img.png)


## 결과

- prettier 관련 에러 사라짐.

- 새 파일 열었을 때 파일 개행 세팅 자동 `LF` 처리됨 <span style="color:red;">(우하단 참조)</span>
![](https://blog.kakaocdn.net/dn/bVJVrO/btsHVICKQFy/ureRwnDxTfStAA4twsyjd0/img.png)


- 운영 환경 리눅스 | Unix에 올릴거면 에디터 개행 셋을 `lf`로 맞출 필요가 있음.

