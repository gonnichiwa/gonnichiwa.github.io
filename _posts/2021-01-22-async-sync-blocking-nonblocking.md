---
title: async-sync-blocking-nonblocking
date: 2023-01-22 22:13:00 +0900
categories: [CONCEPT]
tags: [async,sync,blocking,nonblocking]  # TAG names should always be lowercase
authors: [gonnichiwa]
---

## 질문
```
동기와 비동기, 블로킹과 논블로킹의 차이를 설명하고 각 개념을 적용할 수 있는 사례가 뭐가 있을까?
```

## 설명 전에..
### 사전적 정의
- 정보통신분야에서는 두 신호의 일치화에 쓰이는듯

>동기, 同期, synchronization
주기적인 운동을 하는 개체들이 서로 영향을 주고받거나 받게 됨으로써, 동일한 주기를 갖게되는 것, 그러한 현상을 동기 현상(同期現狀)이라 하고, 동기된 상태를 동기화(同期化) 되었다고 한다, 통신에서는 주로 서로 다른 시스템이나 네트워크에서 클럭 주파수나 비트, 프레임, 워드 등을 일치시키는데 사용된다.

>비동기, asynchronous
연관된 메시지가 연대순 및 절차적으로 분리되는 경우 비동기적인 상호 작용이라고 한다. 요청-응답의 상호 작용을 예로 들면, 클라이언트 에이전트는 미래의 불확실한 시점에서 응답을 처리할 수 있다. 이렇게 하기 위한 메커니즘에는 폴링, 다른 메시지 수신의 알림 등이 있다.
출처 : 단체표준 TTAS.KO-10.0179 웹서비스 용어

>블로킹, blocking
데이터 신호의 전송이 끝나기 전에는 다음 동작을 진행할 수 없는 경우.
출처 : 단체표준 TTAK.OT-10.0288 시스템 수준 반도체 IP 인터페이스 모델링

>논블로킹, non-blocking
데이터 신호의 전송이 끝나기 전에도 다음 동작을 진행할 수 있는 경우. 본 표준에서는 블로킹으로 별도 표기되지 않은 경우에는 모두 논블로킹으로 간주한다.
출처 : 단체표준 TTAK.OT-10.0288 시스템 수준 반도체 IP 인터페이스 모델링


### 본 포스트에서 설명하고자 하는 개념의 사전설정
- 호출하는함수(`caller`)
- 호출당하는함수(`callee`, 이하 `..ee`로 작성)가 있음.

- `caller`가 `..ee`를 호출했을 때,  (`..ee`는 별도의 스레드(같은걸로)에서 실행 된다고 가정)
- 즉 **함수 호출관점**에서 **동기와 비동기 && 블로킹과 논블로킹을 구분**할 필요가 있음.


## 개념 분류 기준

### 블로킹(Blocking) | 논블로킹(Nonblocking)
- `..ee`를 호출한 함수`caller`의 다음 행동을 막는다(blocking) ? Y(블로킹) : N(논블로킹)
- 즉, `..ee`호출시점에 `제어권`을 `..ee`가 가지고 간다? Y(블로킹) : N(논블로킹)

- `제어권` : 쉽게 얘기해서 내가 돌리는 앱의 다음 코드라인을 뭘 실행할지? 핸들링하는 주체 권한


### 동기(Sync) | 비동기(ASync)
- `caller`가 `..ee`의 `완료 여부`를 체크하는지? Y(sync) : N(async)


## CASE
- 4가지 케이스가 있겠다.

### 동기(Sync) - 블로킹(Blocking)
- `Sync`니까, `caller`가 `..ee`에게 완료여부 체크 해야 하지만,
- `제어권`이 `..ee`에게 있으므로 완료여부 체크를 할 수 없음.
- java main()으로 돌리는 여러 알고리즘 문제 코드들

### 동기(Sync) - 논블로킹(Nonblocking)
- `caller`가 `..ee`의 `완료여부체크`(완료했니? 반복) 하면서 `caller`의 작업 돌고있음.
+ 게임 로딩화면
  - 화면 하나 띄워주고, 논블로킹으로 맵정보 불러오면서 완료여부 체크
  - 완료정도 % 형태로 화면에 갱신해줌
+ ...외 논블로킹 방식의 여러 로그인 처리 로직들

### 비동기(ASync) - 논블로킹(Nonblocking)
- `caller`가 `..ee` 의 완료여부 체크 안함.
- `..ee`는 끝나면 `콜백`으로 알려줄 뿐
+ 웹사이트들 
  - 여러 asset파일들(img,js,...) 읽는작업 완료되면 콜백 실행(화면띄워줌) 하는 구조

### 비동기(ASync) - 블로킹(Blocking)
- `..ee` 가 제어권을 가지고 있음.
- `caller`는 완료여부도 신경안씀, 콜백만 전달받아 실행할뿐
- 동기-블로킹과 별 차이가 없다
- 그래서 4가지 케이스 중 가장 끝에 놨음.


### 콜백
---
- 적용 전
```javascript
function findUser(id) {
  let user;
  setTimeout(function () {
    console.log("waited 0.1 sec.");
    user = {
      id: id,
      name: "User" + id,
      email: id + "@test.com",
    };
  }, 100);
  return user;
}
console.log('start');
const user = findUser(1);
console.log("user:", user);
console.log('end');
```
{: file='findUser.js'}

- 실행결과
```
$ node findUser.js
start
undefined
end
waited 0.1 sec.
```
- findUser()를 리턴하기도 전에 메인의 `console.log("user:", user)`를 실행해버림.
- 호출된 함수 `setTimeout()`은 별도로 빠져서 실행됨 `nonblocking`

---

- 적용 후
```js
function findUser(id, callback) {
  let user;
  setTimeout(function () {
    console.log("waited 0.1 sec.");
    user = {
      id: id,
      name: "User" + id,
      email: id + "@test.com",
    };
    callback(user);
  }, 100);
  return user;
}
console.log('start');
const user = findUser(1, function(user) {
    console.log(user);
});
console.log('end, user:' + user);
```
{: file='findUser.js'}

- 실행결과
```
$ node findUser.js
start
end, user:undefined
waited 0.1 sec.
{ id: 1, name: 'User1', email: '1@test.com' }
```
- 완료여부를 체크하지 않고, 완료되면 `callback`으로 알려줌

#### 사족
- 실행결과상 undefined가 신경쓰여 promise로 적용
```js
function findUser(id) {
  let user;
  return new Promise((resolve, _) => {
    console.log("waited 0.1 sec.");
    setTimeout(() => {
      user = {
        id: id,
        name: "User" + id,
        email: id + "@test.com",
      };
      resolve(user)
    }, 1000)
  });
}
function userMapping(user) {
  return Object.entries(user).map(([k,v]) => (
    `${k}:${v}`
  ));
}
console.log('start');
findUser(1).then((user) => {
  console.log('end, user= ' + userMapping(user));
});
```

- 실행결과
```
start
waited 0.1 sec.
end, user= id:1,name:User1,email:1@test.com
```



### 참고자료
- [TTA 정보통신 용어사전](https://terms.tta.or.kr/dictionary/dictionaryView.do?word_seq=086653-1)
- [자바 네트워크 소녀 Netty](https://www.aladin.co.kr/m/mproduct.aspx?ItemId=67191832)
- [Callback함수란?? 뭔데??](https://velog.io/@ko1586/Callback%ED%95%A8%EC%88%98%EB%9E%80-%EB%AD%94%EB%8D%B0)
- [개발자 기술면접 노트](https://product.kyobobook.co.kr/detail/S000212738756)
- [완벽히 이해하는 동기/비동기 & 블로킹/논블로킹](https://inpa.tistory.com/entry/%F0%9F%91%A9%E2%80%8D%F0%9F%92%BB-%EB%8F%99%EA%B8%B0%EB%B9%84%EB%8F%99%EA%B8%B0-%EB%B8%94%EB%A1%9C%ED%82%B9%EB%85%BC%EB%B8%94%EB%A1%9C%ED%82%B9-%EA%B0%9C%EB%85%90-%EC%A0%95%EB%A6%AC#%ED%99%9C%EC%9A%A9_%EC%98%88%EC%8B%9C_%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%A8)