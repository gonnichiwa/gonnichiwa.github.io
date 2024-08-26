---
title: mocha 설치와 사용법
date: 2024-05-06 20:15:43 +0900
categories: [JS, TEST]
tags: [nodejs, mocha, test]  # TAG names should always be lowercase
authors: [gonnichiwa]
---

## 작성목적

`mocha` 모듈 설치와 nodejs 환경에서 작성한 코드의 테스트 방법을 기록함.  
windows powershell 환경에서 작성 및 run 가이드임.


## 설치

### install mocha

empty 디렉토리에서 생성함

```sh
$ npm install mocha --save-dev
```

- `--save-dev` : 프로젝트 디렉토리/node_modules 넣고 `devDependancies` 설정
+ 반대(글로벌 설치) : `npm install mocha -P` or `npm install mocha`
  - Win10 기준으로는 (사용자명)\AppData\Roaming\npm\node_modules에 들어감



![](https://blog.kakaocdn.net/dn/cXi1Rc/btsHcJCjysG/mBSw4m7g9A50uhDiJynBgk/img.png)

- 의존 모듈들과 package.json 생성됨.


### package.json

```json
{
  "devDependencies": {
    "mocha": "^10.4.0"
  },
  "scripts": {
    "test": "mocha"
  }
}
```

- `devDependencies` : 프로덕션 안올리고 개발 시에만 쓰는 의존
+ `scripts` 넣어서 mocha 테스트 돌릴 수 있도록 추가해준다
  - `npm run test`

```shell
$ npm run test
Debugger attached.

> test 
> mocha

Debugger attached.
Error: No test files found: "test"
Waiting for the debugger to disconnect...
Waiting for the debugger to disconnect...
```

- 현재 경로상 `/test` 디렉토리가 없음
- mocha 설치 완료

<br/>

## RUN TEST

### 간단한 실행 파일 생성

#### /app.js

```js
module.exports = {
  sayHello: function () {
    return 'hello';
  }
}
```

#### /test/app.spec.js

```js
const sayHello = require('../app').sayHello;

describe('App test!', function () {
    it('sayHello should return hello', function (done) {
      if (sayHello() === 'hello') {
        done();
      }
    });
  });
```

- 아래처럼 다시 test 돌려봄

#### RUN TEST

```sh
$ npm run test

> test 
> mocha

Debugger attached.


  App test!
    ✔ sayHello should return hello


  1 passing (4ms)
```


## mocha 사용법

추가예정


## 참고

https://heropy.blog/2018/03/16/mocha/  
- node
https://c17an.netlify.app/blog/node.js/npm-install-%EC%A0%95%EB%A6%AC/article/  