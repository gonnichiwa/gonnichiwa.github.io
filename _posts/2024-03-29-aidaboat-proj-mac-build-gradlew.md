---
title: gradlew build spring boot proj on mac
date: 2024-03-28 18:15:00 +0900
categories: [SPRING, JAVA, MAC, GRADLE]
tags: [gradle, spring, java, mac]  # TAG names should always be lowercase
authors: [gonnichiwa]
---

spring boot proj 소스 클론 이후 `gradlew` 빌드 과정을 기록함.

## gradle build와 실패 메세지 처리

### command not found
- 말그대로 커맨드 잘못입력했거나
- 실행 권한 없거나

```
$ sudo ./gradlew build
sudo: ./gradlew: command not found

$ ls -al | grep gradlew
-rw-r--r--   1 jae-hunjeong  staff  8692  3 29 18:10 gradlew
```
실행권한이 없음.

```
$ sudo chmod 755 ./gradlew
$ ls -al | grep gradlew
-rwxr-xr-x   1 jae-hunjeong  staff  8692  3 29 18:10 gradlew

```
r(읽기,4)w(쓰기,2)x(실행,1) 이므로 `owner/group/other` 별로 권한 부여 가능함.

실행권한 주려고 `755` 줌

### build 실패

```
$ ./gradlew build
............10%...100%

Welcome to Gradle 8.6!
...
FAILURE: Build failed with an exception.
Incompatible because this component declares a component for use during compile-time, compatible with Java 17 and the consumer needed a component for use during runtime, compatible with Java 10

```
gradle 플러그인 컴파일러 자바 버전과 내 mac 로컬의 자바 버전 안맞단다.

![](https://blog.kakaocdn.net/dn/FtHUW/btsGbRW0x4A/cC0J1cBzkox2YkkXqhO0S0/img.png)
intellij 로 오픈시키면 jdk 버전 맞춰 다운받을줄 알았더니 동작안함.<br/>
intelilj project close (`File` - `Close project`) 해주고
  
  
[이전 포스트](https://gonnichiwa.github.io/posts/aidaboat-1-openproject/#%EA%B8%B0%EC%88%A0%EC%A0%81)에서 개발환경 java 21로 맞추기로 했으니 mac 환경설정도 이에 따라 맞춰준다.

~~로컬에서 docker 돌려야하나? [여기처럼](https://dev.gmarket.com/72)~~
  
## OpenJDK 설치과정
  1. [openjdk21 다운로드: macOS / x64(내껀 인텔CPU니까, M1박은 CPU라면 AArch64 다운받을것)](https://jdk.java.net/21/)
  - 본인은 openjdk-21.0.1_macos-aarch64_bin.tar.gz 다운받음
    
  2. 체크섬 확인
  - 파일이 올바른 것인지?
  - `올바르다` : 파일이 '특정 이유'로 `변조` 된것이 아닌지? 확인
  ```
  $ shasum -a 256 openjdk-21.0.2_macos-x64_bin.tar.gz
  ```
  ![](https://blog.kakaocdn.net/dn/bk9sZ8/btsGeaOzB1z/iwvSfukhW3YkmhgBnL0iD0/img.png)
  - local이 명령 실행하여 리턴된 것.
  - downl이 다운받을 (공식) 사이트의 sha256 게시값.
    
  3. `openjdk-21.0.1_macos-x64_bin.tar.gz` 파일 압축 해제
  - `tar`는 파일/디렉토리 여러개 한개의 `.tar` 파일로 묶은것. (용량을 압축시키는게 아님)
  - `gz` 특정 압축 알고리즘 써서 `용량 압축` 시킨 파일
  ```
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
  
  4. 환경변수 연결 (zshell)
  - 본인은 zshell(~~이뻐서~~) 씀
  ```
  $ cat >> .zshrc
  ```
  - 커맨드 입력 (bash라면 `cat >> .bash_profile` 쓰셈)
  - 아래 한줄씩 복붙
  ```
  export JAVA_HOME=$HOME/OpenJDK/jdk-21.0.2.jdk/Contents/Home
  export PATH=$JAVA_HOME/bin:$PATH
  ```
  - `Ctrl` + `D` 하여 저장

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
  끝났으니 다시 gradlew 돌려보자

```
$ ./gradlew build
...
BUILD SUCCESSFUL in 10s
```
  
