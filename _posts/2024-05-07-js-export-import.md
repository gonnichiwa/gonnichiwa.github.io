---
title: js export import (feat. commonJS, module)
date: 2024-05-07 17:15:43 +0900
categories: [JS]
tags: [nodejs, module, commonjs]  # TAG names should always be lowercase
authors: [gonnichiwa]
---

## PROBLEM

* 아래와 같은 import 관련 런타임 에러를 만날 수 있음.
  - Cannot use import statement outside a module
  - Unexpected token 'export'  
  - Exception during run: ReferenceError: require is not defined in ES module scope, you can use import instead
This file is being treated as an ES module because it has a '.js' file extension and 'C:\...\package.json' contains "type": "module". To treat it as a CommonJS script, rename it to use the '.cjs' file extension.

- 프로젝트 실행환경이 `commonjs` | `module` 인지에 따라 객체 `import, export` 써야하는 키워드가 다름.


## 작성목적

- `packages.json`의 `type: commonjs` 혹은 `type: module` 에 따라 파일 `import, export 가이드` 기술함.


## module : export, default, import, as

- 아래와 같이 `package.json` 의 `type` 속성이 `module` 인 경우

```js
{
  ...
  "type": "module"
}
```
_package.json_

- keyword : `export`, `export default`

### 1모듈 1객체 내보내기 : export default

- export

```js
export default class User { // default 하나의 모듈(some.js) 에서 하나의 객체(User)만 내보낸다.
    constructor(name) {
      this.name = name;
    }
    sayHello() {
      console.log(this.name);
    }
  }
```

- import

```js
import User from './some.js';

const user = new User('John');
user.sayHello(); // John
```

<br/>

### export 할 모듈(.js) 내에서 선택적으로 내보내기

- `export default`

```js
// currency-functions.js
const exchangeRate = 0.91;

// 안 내보냄
function roundTwoDecimals(amount) {
  return Math.round(amount * 100) / 100;
}

// 내보내기
export default {
  canadianToUs(canadian) {
    return roundTwoDecimals(canadian * exchangeRate);
  },

  usToCanadian: function (us) {
    return roundTwoDecimals(us / exchangeRate);
  },
};
```

- import

```js
import User from './some.js';
import * as currency from './currency-functions.js';

const user = new User('John');
user.sayHello();
console.log(currency.canadianToUs(30000)); // 27300
```

<br/>


- 클래스도 아래 꼴로 가능
- export

```js
export function sampleProvinceData(){
    ...
}

export class Province {
    ...
}

export class Producer {
    ...
}
```

- import

```js
import { sampleProvinceData, Province, Producer } from '../source/sample.js';
import assert from 'assert'; // node_modules에 installed

describe('sample.spec.js', function() {
    it('shortfall', function() {
        const asia = new Province(sampleProvinceData());
        assert.equal(asia.shortfall, 5);
    });
});
```



<br/>


## **commonjs : require, exports, module.exports**

- 아래와 같이 `package.json` 의 `type` 속성이 `commonjs` 인 경우

```js
{
  ...
  "type": "commonjs"
}
```

- **export**

```js
const exchangeRate = 0.91;

function roundTwoDecimals(amount) {
  return Math.round(amount * 100) / 100;
}

const canadianToUs = function (canadian) {
  return roundTwoDecimals(canadian * exchangeRate);
};

function usToCanadian(us) {
  return roundTwoDecimals(us / exchangeRate);
}

exports.canadianToUs = canadianToUs; // 내보내기 1
exports.usToCanadian = usToCanadian; // 내보내기 2
```

- **선택하여 export**

```js
const exchangeRate = 0.91;

// 안 내보냄
function roundTwoDecimals(amount) {
  return Math.round(amount * 100) / 100;
}

// 내보내기
const obj = {};
obj.canadianToUs = function (canadian) {
  return roundTwoDecimals(canadian * exchangeRate);
};
obj.usToCanadian = function (us) {
  return roundTwoDecimals(us / exchangeRate);
};
module.exports = obj;
```

- **import**

```js
const currency = require("./currency-functions");

console.log("50 Canadian dollars equals this amount of US dollars:");
console.log(currency.canadianToUs(50));

console.log("30 US dollars equals this amount of Canadian dollars:");
console.log(currency.usToCanadian(30));
```

### module.exports

- node 실행환경 콘솔에서 `this.module` 치고 들어가면 아래처럼 module객체 속성들이 나옴.
- 그안에 `exports`와 `paths` 있고 여기에 객체와 모듈 위치(경로) 추가

```sh
PS C:\dev\js\exportimport> node
Welcome to Node.js v20.11.1.
Type ".help" for more information.
> this.module
{
  id: '<repl>',
  path: '.',
  exports: {}, // exports 객체들이 여기 들어가겠지...
  filename: null,
  loaded: false,
  children: [],
  paths: [
    ...
    'C:\\node_modules',
    ...
  ]
}
```

## 참고 

https://www.daleseo.com/js-module-require/  
https://www.daleseo.com/js-module-import/  
