---
title: nestjs mac 환경설정
date: 2024-06-09 14:54:03 +0900
categories: [JS]
tags: [nestjs]  # TAG names should always be lowercase
authors: [gonnichiwa]
---

## 작성 목적

- https://docs.nestjs.com/first-steps 참조하여
- `nestjs` 설치와 구동 해본다

## 구동 수행

- `nestjs` 설치
```shell
$ npm -i -g @nestjs/cli 
npm WARN deprecated inflight@1.0.6: This module is not supported, and leaks memory. Do not use it. Check out lru-cache if you want a good and tested way to coalesce async requests by a key value, which is much more comprehensive and powerful.
npm WARN deprecated glob@7.2.3: Glob versions prior to v9 are no longer supported

added 280 packages in 34s

63 packages are looking for funding
  run `npm fund` for details
```

- `yarn`으로 nest 프로젝트 생성함.

```shell
 nest new nest-playground 
 ⚡  We will scaffold your app in a few seconds..
? Which package manager would you ❤️  to use? 
  npm 
❯ yarn 
  pnpm 
```

- `package.json` 여니 아래 커맨드들로 구동 할 수 있다

```json
"scripts": {
    "build": "nest build",
    "format": "prettier --write \"src/**/*.ts\" \"test/**/*.ts\"",
    "start": "nest start",
    "start:dev": "nest start --watch",
    "start:debug": "nest start --debug --watch",
    "start:prod": "node dist/main",
    "lint": "eslint \"{src,apps,libs,test}/**/*.ts\" --fix",
    "test": "jest",
    "test:watch": "jest --watch",
    "test:cov": "jest --coverage",
    "test:debug": "node --inspect-brk -r tsconfig-paths/register -r ts-node/register node_modules/.bin/jest --runInBand",
    "test:e2e": "jest --config ./test/jest-e2e.json"
  },
```

- `nest start` 하던 `npm run start` 하던 서버구동
```shell
$ npm run start
> nest-playground@0.0.1 start
> nest start
[Nest] 14946  - 2024. 06. 09. 오후 3:09:37     LOG [NestFactory] Starting Nest application...
[Nest] 14946  - 2024. 06. 09. 오후 3:09:37     LOG [InstanceLoader] AppModule dependencies initialized +10ms
[Nest] 14946  - 2024. 06. 09. 오후 3:09:37     LOG [RoutesResolver] AppController {/}: +4ms
[Nest] 14946  - 2024. 06. 09. 오후 3:09:37     LOG [RouterExplorer] Mapped {/, GET} route +3ms
[Nest] 14946  - 2024. 06. 09. 오후 3:09:37     LOG [NestApplication] Nest application successfully started +1ms

```

- 3000번 포트로 날려봄
```shell
$ curl localhost:3000
Hello World!%                             
```

## TEST


