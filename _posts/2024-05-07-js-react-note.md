---
title: react note
date: 2024-05-21 14:11:33 +0900
categories: [REACT]
tags: [js, react]  # TAG names should always be lowercase
authors: [gonnichiwa]
---

## 작성목적

- react 앱 작성중 실무 수행 할만한 작업들 가이드 나열함.

## profiler

- `react developer tools`를 chrome extension으로 설치
- react app debug와 성능측정 프로파일러로 쓸 수 있다.

- https://chromewebstore.google.com/detail/react-developer-tools/fmkadmapgofadopljbjfkapdkoienihi

- 개발자 도구(f12) 에서 아래와 같이 메뉴 구성 뜸  
![](https://blog.kakaocdn.net/dn/m4UUg/btsHUa1tD7q/JieoZkNKstGcOjMnalva21/img.png)

### 성능측정 수행 example

- 아래처럼 리스트 업데이트 이벤트 발생 시 반응 속도가 느린 이슈를 profile 했음
- profiler 왼쪽 위 녹화 버튼누르고, app 이벤트 발생 시키고(클릭) 녹화버튼 누르면 그래프화 하여 수행속도 분석 보여줌.  
![](https://blog.kakaocdn.net/dn/blfTGi/btsHUqbZOY4/vlKq1kzf7KBExGZa8h6GV0/img.png)
  
- 결과 보니 `rendered at` 측정시간 608ms로 느림
- row 한개 업데이트 했을 뿐인데 전체 리스트를 리렌더링 함.

+ 컴포넌트는 아래 조건으로 리렌더링된다.
  - 전달받은 `props`의 변경
  - 자기의 `state` 변경
  - 부모 컴포넌트가 리렌더링 될 때
  - `forceUpdate` 함수 수행될 때




## JSX 마크업 태그 작성 시

- 닫는태그를 정확히 작성해야함.
- `<>, </>`
- `<img/>`, `<input/>`

```js
export default function TodoList() {
  return (
    <> /* ****************** */
    // This doesn't quite work!
    <h1>Hedy Lamarr's Todos</h1>
    <img
      src="https://i.imgur.com/yXOvdOSs.jpg"
      alt="Hedy Lamarr"
      class="photo"
    />
    <ul>
      <li>Invent new traffic lights</li>
      <li>Rehearse a movie scene</li>
      <li>Improve spectrum technology</li>
    </ul>
  </>
  );
}

```

https://ko.react.dev/learn/describing-the-ui#writing-markup-with-jsx

## React.memo

- 함수형 컴포넌트에서, 리렌더링 필요시, 미리 렌더링 된 요소 데이터들과 비교, 업데이트 필요한 요소만 업데이트 가능토록 wrapping 기능 제공
- 사용법
```js
import React from 'react';

...

export default React.memo(TodoListItem);
```

## useState 함수형 업데이트

### asis
- `useCallback()`의 두번째 파라미터로 todos 상태 변경 시 수행 되도록 구현함.
```js
const onRemove = useCallback(id => {
  setTodos(todos.filter(todo => todo.id !== id));
}, [todos]);
```

- `todos` 상태 객체 크기가 매우 클 경우 (json array element기준 2000개만 넘어도) 로딩속도에 타격


### tobe

- `setTodos()` 파라미터 주목, 함수형으로 던져서 새로운 상태를 어떻게 업데이트 할 지 지정함.

```js
const [todos, setTodos] = useState(createBulkTodos);
// prevNumber는 현재 number 값
const onIncrease = useCallback(() => setNumber  const onInsert = useCallback(text => {
    const todo = {
      id: nextId.current,
      text,
      checked: false,
    };
    setTodos(todos => todos.concat(todo));
    nextId.current += 1; // nextId 1씩 더하기
  }, []); // 2nd param [todos] : todos 변경되었을 때 수행
```

```js
  const onToggle = useCallback(id => {
    setTodos(todos =>
      todos.map(todo => todo.id === id ? {...todo, checked: !todo.checked } : todo)
    );
  }, []);
```

## 불변성의 중요성 (immer를 왜 쓰는가)

- 기존 데이터를 수정할 때
- `복사한 새 배열을 만든 다음` 필요한 업데이트 수행.
- 불변성을 못지키면 객체 내부의 값이 업데이트 되어도 바뀐 것을 감지하지 못함. (ex) React.memo

```js
const array = [1,2,3];
const nextArrayBad = array; // 복사x 똑같은 배열 가리킴
nextArrayBad[0] = 100;
console.log(array === nextArrayBad); // true
```

- 그래서 객체 복사를 아래 처럼 하는데,,,

```js
const object = {
  foo: 'bar',
  value : 1
}
const nextObject = {
  ...object,              // 객체 복사
  value: object.value + 1 // 새로운 값 덮어씀
}
console.log(object === nextObjectGood); // false, 다른 객체이므로
```

- `...object` 같은 전개 연산자는 `얕은 복사` 수행함.
- js에서 json 얕은 복사 하면 json object 바깥쪽만 복사됨.
- 아래는 예시

```js
const todos = [{id: 1, checked: true}, {id:2, checked:true}];
const nextTodos = [...todos];
console.log(todos === nextTodos); // false. 복사됐음


nextTodos[0].checked = false;
console.log(todos[0] === nextTodos[0]); // true. 복사 안됐음

// 복사 안되서 ... 연산자로 다시 복사 수행
nextTodos[0] = {
  ...nextTodos[0],
  checked:false
};
console.log(todos[0] === nextTodos[0]); // false. 복사됨
```

- ... 그래서 `객체안 객체` 라면 아래처럼 새 값 할당해줘야됨

```js
const nextComplexObj = {
  ...complexObject,
  objectInside: {
    ...complexObject.objectInside,
    enables: false
  }
};
console.log(complexObj === nextComplexObj); // false. 복사되었으니 false
console.log(complexObj.objectInside === nextComplexObj.objectInside); // false
```

- 프로덕션에서 타시스템과 주고받는 json 전문의 깊이는 3 이상으로 넘어갈 때가 많다.
- 그럼 불변성을 유지하면서 업데이트 하기가 까다로워짐 (머리아픔)

- 이럴때 `immer`를 쓴다.