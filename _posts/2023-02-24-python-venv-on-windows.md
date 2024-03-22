---
title: install python virtual env on windows
date: 2023-02-24 15:27:00 +0900
categories: [PYTHON, NLP, ML]
tags: [python, venv, nlp, enviroment, ml]     # TAG names should always be lowercase
authors: [gonnichiwa]
---
## 0. 왜하나?
- 1pc 에서 서로 연관 없는 `프로젝트` 나 `예제공부` 작업들을 python으로 수행할때
- 여러가지 라이브러리들을 `pip3`로 인스톨 하게 되는데
- 로컬pc에서 글로벌하게 설치 하다보면
- `프로젝트`쪽 의존 라이브러리 버전과
- `다른 프로젝트` 혹은 `내 공부` 작업 환경에서 쓰는 라이브러리 의존 (버전) 충돌 발생할 수 있음.
- 작업들마다 `virtualenv` 환경을 갖추고 각 작업환경마다 `격리된 라이브러리 의존환경`을 구성하여

- __작업들간의 `의존 (버전) 충돌`로 인한 시간낭비를 예방함.__


## 1. 파이썬3 설치
- `cmd`에서 `python3` , windows store에서 다운받은 뒤 실행
```
\> python3 -version 
Python 3.12.2
```

## 2. virtualenv 설치
```
\> python3 -m pip install --user -U virtualenv
```
- install 후 `WARNING` 사인 내용 확인 : `virtualenv.exe` 경로 환경변수 path 추가

### 2-1. virtualenv 설치 확인
```
\> virtualenv
usage: virtualenv [--version] ...
SystemExit: 2
```

## 3. 가상환경 생성하기 (create)
__작업할 디렉토리 생성__ 후 콘솔 명령 : `virtualenv env`
```
\> virtualenv env
C:\dev\python\venvtest>virtualenv env
created virtual environment CPython3.12.2.final.0-64 in 710ms
...
PythonActivator

C:\dev\python\venvtest>dir
 ...
2024-03-22  오후 08:40    <DIR>          .
2024-03-22  오후 08:40    <DIR>          ..
2024-03-22  오후 08:40    <DIR>          env
 ...
 ```

## 4. 가상환경 시작하기, 나가기 (activate, deactivate)

- 3.에서 생성한 가상환경 경로 내에서 명령 `env/Scripts/activate`
```
C:\dev\python\venvtest\>cd env/scripts
C:\dev\python\venvtest\env\scripts\>activate
```
- 콘솔 경로 앞에 `(env)` 가 붙는다. 파이썬 가상환경 진입완료.
```
(env) C:\dev\python\venvtest\env\Scripts>
```
- 가상환경 나가기 : `deactivate`
```
(env) C:\dev\python\venvtest\env\Scripts>deactivate
C:\dev\python\venvtest\env\Scripts>
```

## 가상환경 안에서 패키지 설치하기

4. 수행한 경로에서 `pip3 install numpy pandas scipy`
```
(env) C:\dev\python\MathForProgrammers\env\Scripts>pip3 install numpy pandas scipy scikit-learn matplotlib
Collecting numpy
  Using cached numpy-1.26.4-cp312-cp312-win_amd64.whl.metadata (61 kB)
...
Successfully installed...
```
- 본인 작업환경에 따라 python 라이브러리 의존충돌 없이 작업이 가능하다.

## 사족
- python3.3 이상 버전이라면 설치하면 내장된 `venv`가 있음.
- `venv` 와 `virtualenv`는 서로 다른 툴임.
- `venv`는 pip로 업그레이드 할 수 없다 소문으로 `virtualenv`선택함


### 작성 참고, 자료출처
[링크](https://jaemunbro.medium.com/python-virtualenv-venv-%EC%84%A4%EC%A0%95-aaf0e7c2d24e)