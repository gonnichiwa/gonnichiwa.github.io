---
title: 이 시스템에서 스크립트를 실행할 수 없으므로 ...\yarn.ps1 파일을 로드할 수 없습니다.
date: 2024-05-27 10:10:33 +0900
categories: [POWERSHELL]
tags: [powershell, yarn]  # TAG names should always be lowercase
authors: [gonnichiwa]
---

## problem

- windows에서 `npm install --global yarn` 설치 후 powershell에서 버전 확인 하니 아래 메세지와 같이 실행불가 발생

```cmd
PS \> yarn --version
yarn : 이 시스템에서 스크립트를 실행할 수 없으므로 C:\Users\...\AppData\Roaming\npm\yarn.ps1 파일을 로드할 수 없습니다. 자세한 내용은 about_Execution_Policies(https://go.microsoft.com/fwlink/ 
+ yarn --version
+ ~~~~
+ CategoryInfo          : 보안 오류: (:) [], PSSecurityException
+ FullyQualifiedErrorId : UnauthorizedAccess
```


## 원인

- `powershell` `ExecutionPolicy`의 권한문제였음

```cmd
PS C:\> Get-ExecutionPolicy
Restricted
```

```cmd
PS C:\> Get-ExecutionPolicy -List

        Scope ExecutionPolicy
        ----- ---------------
   UserPolicy       Undefined
      Process       Undefined
  CurrentUser       Undefined
 LocalMachine       Undefined
```

- 현재 OS유저에 대해서 RemoteSigned 권한 줘서 실행 가능하도록 처리함.

```cmd
PS C:\> Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser
PS C:\> Get-ExecutionPolicy -List

        Scope ExecutionPolicy
        ----- ---------------
MachinePolicy       Undefined
   UserPolicy       Undefined
      Process       Undefined
  CurrentUser    RemoteSigned
 LocalMachine       Undefined
```

- 확인

```cmd
PS C:\dev\js\react-playground> yarn --version
1.22.22
```

## 참고

https://learn.microsoft.com/ko-kr/powershell/module/microsoft.powershell.core/about/about_execution_policies?view=powershell-7.4
