---
title: Install This blog on windows
date: 2023-03-22 17:09:00 +0900
categories: [BLOG]
tags: [jekyll, Chirpy]     # TAG names should always be lowercase
authors: [gonnichiwa]
---

# Install This Blog on windows

## 사전환경구성
1. setup git
1. source clone

## download ruby
1. Ruby+Devkit 3.3.1-1 (x64) [download](https://github.com/oneclick/rubyinstaller2/releases/download/RubyInstaller-3.3.1-1/rubyinstaller-devkit-3.3.1-1-x64.exe)

## install file & setup on cmd
1. __setup ruby + mingw devkit__
  - __다운로드 파일 setup : 1 선택__
![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FBxyQF%2FbtsF015IF1r%2F1gMY62LVo2pzoNFv4J2Gs1%2Fimg.png)

1. __setup jekyll env__
> - jekyll env setup doc : [__jekyll on windows__](https://jekyllrb.com/docs/installation/windows/)
> - ```clonedpath\> gem install jekyll bundler ```
> - ```clonedpath\> bundle install ```

## start blog on local env
1. open new `cmd`
2. ``` \> bundle exec jekyll serve ```
> 구동 확인
![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FciANyq%2FbtsF2xbjBfA%2Fz1nCdUEvjPCqPh83vVJvB1%2Fimg.png)