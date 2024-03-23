---
title: how to update .gitIgnore but it is not applied after it committed (.gitIgnore 추가적용)
date: 2023-03-20 16:30:00 +0900
categories: [GIT]
tags: [git, gitignore, cache]     # TAG names should always be lowercase
authors: [gonnichiwa]
---

## 0. 현상
- `text.txt` 파일은 참고용으로 `push`했던 파일인데 내 로컬에서만 봐도 괜찮다.
- 커밋할 필요가 없음
- 그래서 `.gitIgnore`에 추가해서 업데이트 했으나
- 이후 `text.txt`를 업데이트 하니 `UnCommitted changes`에 계속 나타남.
- 업데이트한 `.gitIgnore` 사항대로 `UnCommitted changes`에도 안나타나게 하고싶음.

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FlhmOt%2FbtsF16rYHtA%2FZ93X12c9WyKP4zU7K6HDYk%2Fimg.png) 
_1. 미리 커밋(해버린) `text.txt` 파일_

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbMWA42%2FbtsF0sbWjvM%2Fav6XTyN66K7kTuaUPDwNGK%2Fimg.png)
_2. `.gitIgnore` 업데이트_

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FcuRgmg%2FbtsF1tgW7KX%2FnvXpW3A7PgkUnMJJWK2vCK%2Fimg.png)
_3. ...했으나 `text.txt` 수정해도 `uncommitted changes`에 나타남_


## 1. 원인

- (알다시피) `.gitIgnore`는 커밋 무시하는 파일 목록 (__커밋할 필요가 없는 파일__)
- 이미 커밋한 파일이므로 `.gitIgnore`는 무시됨.

## 2. 조치

### 2-1. (소견) 약간 위험한 방법
- 검색 결과 나온 방법 중 하나
```
$ git rm -r --cached .
$ git add .
$ git commit -m "[fixed 되었다는 메시지 작성]"
$ git push origin [branch]
```
1. `git rm -r --cached .` 명령에서 본 작업공간상 추가했던 __모든 파일을 삭제처리__ 하는 `uncommitted changes`가 생성됨.
1. 생성된 `uncommitted changes`에서 삭제 시키면 안되는 파일들을 일일이 골라내야함.
1. 골라내서 삭제시켰는데, `운영|개발` 서버에서 참조하는 파일이었다면?
1. 실수 방지 우선 관점상 안티패턴같이 느껴짐.

### 2-2. 조치 & 결과
- `text.txt` 내 로컬 `다른경로에 백업`해두고 `삭제 커밋&push`
- 백업해둔 `text.txt` 다시 작업디렉토리에 둠.

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fbsxn7K%2FbtsF1RaHCPX%2FGEyxyd6mikViydU02ViwoK%2Fimg.png)
_1.삭제 커밋 올리고(파일은 다른경로에 백업해둠)_

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fcs2RgO%2FbtsF3lWdN62%2FibJS0Lp5oGWwXpwPtXGIKK%2Fimg.png)
_2.다시 작업공간에 갖다놓으니_

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FNSSNz%2FbtsF2FARVDt%2FMFxMByJMZoThxlbYx8E4lk%2Fimg.png)
_3. uncommitted changes에 걸리지 않는다. `text.txt`_


## 3. 적용 예

- 자신의 github.io(github pages) 에 블로그 템플릿 적용할 때 
- `Gemfile.lock` 같은 파일을 `함께 커밋&push`해버려서
- 로컬에서는 동작하나 `github action`으로 배포 할때 빌드 깨지는 경우가 생김.
