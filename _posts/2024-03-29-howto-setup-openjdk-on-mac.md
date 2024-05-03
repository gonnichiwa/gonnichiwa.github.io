---
title: gradlew build spring boot proj on mac
date: 2024-03-29 19:15:00 +0900
categories: [SPRING, JAVA, MAC, GRADLE]
tags: [gradle, spring, java, mac]  # TAG names should always be lowercase
authors: [gonnichiwa]
---

## 작성 목적
`macos` 환경에서 `openjdk21` 설치 가이드를 제공한다.

## OpenJDK 설치과정
  1. [openjdk21 다운로드: macOS / x64(설치할 macbook은 인텔CPU니까, M1박은 CPU라면 AArch64 다운받을것)](https://jdk.java.net/21/)
  - 본인은 openjdk-21.0.1_macos-x64_bin.tar.gz 다운받음

  <br/>
    
  2. 체크섬 확인
  - 파일이 올바른 것인지?
  - `올바르다` : 파일이 '특정 이유'로 `변조` 된것이 아닌지? 확인
  ```bash
  $ shasum -a 256 openjdk-21.0.2_macos-x64_bin.tar.gz
  ```
  ![](https://blog.kakaocdn.net/dn/bk9sZ8/btsGeaOzB1z/iwvSfukhW3YkmhgBnL0iD0/img.png)
 _local: 다운받은 파일의 shasum, downl:웹사이트에 게시된 설치파일의 shasum_

  - local이 명령 실행하여 리턴된 것.
  - downl이 다운받을 (공식) 사이트의 sha256 게시값.
    
    <br/>
    
  3. `openjdk-21.0.1_macos-x64_bin.tar.gz` 파일 압축 해제
  - `tar`는 파일/디렉토리 여러개 한개의 `.tar` 파일로 묶은것. (용량을 압축시키는게 아님)
  - `gz` 압축 알고리즘 써서 `용량 압축` 시킨 파일
  ```bash
  $ mkdir $HOME/OpenJDK
  $ sudo tar -xvf openjdk-21.0.2_macos-x64_bin.tar.gz -C .$HOME/OpenJDK
    ./
    x ./jdk-21.0.2.jdk/
    x ./jdk-21.0.2.jdk/Contents/
    x ./jdk-21.0.2.jdk/Contents/CodeResources
    ...
    x ./jdk-21.0.2.jdk/Contents/Home/bin/jimage
    x ./jdk-21.0.2.jdk/Contents/_CodeSignature/CodeResources
  ```
  - 명령 실행 경로에(`echo $HOME`하면 경로나옴) OpenJDK 디렉토리가 없으면
  - tar: could not chdir to './OpenJDK' 뜰 수 있음.
  - 압축해제한 디렉토리에서 환경변수 잡을것이므로 $HOME/OpenJDK로 잡음.
  
  <br/>
  
  4. 환경변수 연결 (zshell)
  - 본인은 zshell(~~이뻐서~~) 씀
  ```bash
  $ cat >> .zshrc
  ```
  - 커맨드 입력 (bash라면 `cat >> .bash_profile` 쓰셈)
  - 아래 한줄씩 복붙
  ```bash
  export JAVA_HOME=$HOME/OpenJDK/jdk-21.0.2.jdk/Contents/Home
  export PATH=$JAVA_HOME/bin:$PATH
  ```
  - `Ctrl` + `D` 하여 저장

  <br/>

  5. 확인
  - 열려있는 터미널 끄고 다시 열어봄.  
  ```bash
  $ java -version
    openjdk version "21.0.2" 2024-01-16
    OpenJDK Runtime Environment (build 21.0.2+13-58)
    OpenJDK 64-Bit Server VM (build 21.0.2+13-58, mixed mode, sharing)
  $ javac -version
  javac 21.0.2
  ```

