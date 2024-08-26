---
title: Could not set process working directory to could not set current directory (errno 2)
date: 2024-04-23 12:31:00 +0900
categories: [INTELLIJ]
tags: [intellij]  # TAG names should always be lowercase
authors: [gonnichiwa]
---

## 현상과 메세지

module name 변경 (user -> gateway) 후 intellij idea에서 gradle sync 빌드 실패

메세지
> Could not set process working directory to could not set current directory (errno 2)


## 조치와 완료 확인

- 메인메뉴 -> file -> close project
- intellij idea 종료
+ 파일 탐색기에서 아래 디렉토리 삭제 (or 다른 경로로 백업)
  - .idea/
  - .gradle/

- intellij idea 다시 오픈
- 빌드 및 gradle sync 성공, `완료.`