---
title: elixir 0 - 튜토리얼
date: 2024-08-20 18:50:00 +0900
categories: [Elixir]
tags: [env]  # TAG names should always be lowercase
authors: [gonnichiwa]
---

## 0. 사용 이유
 - 최하단 `참고` 영상들 볼것

## 0-1. 작성목적

 - elixir실행환경과 언어 기본사항 숙지용

## 1. elixir 실행환경 설정

+ erlang 설치 (`windows`,`mac`):  
  - https://elixir-lang.org/install.html
  - erlang/OTP `27`버전 설치함.

+ elixir `1.17.2` 설치
  - https://elixir-lang.org/install.html#windows

- `erlang`과 `elixir` 모두 환경변수 setting.

```
C:\Users\keept>erl
Erlang/OTP 27 [erts-15.0.1] [source] [64-bit] [smp:16:16] [ds:16:16:10] [async-threads:1] [jit:ns]

Eshell V15.0.1 (press Ctrl+G to abort, type help(). for help)
1>
BREAK: (a)bort (A)bort with dump (c)ontinue (p)roc info (i)nfo
       (l)oaded (v)ersion (k)ill (D)b-tables (d)istribution

C:\Users\keept>

C:\Users\keept>elixir -v
Erlang/OTP 27 [erts-15.0.1] [source] [64-bit] [smp:16:16] [ds:16:16:10] [async-threads:1] [jit:ns]

Elixir 1.17.2 (compiled with Erlang/OTP 27)
```

## 2. 쳐보면서 찍먹 해보자

- https://joyofelixir.com/1-appeasing-the-masses/
- https://joyofelixir.com/2-where-did-i-put-that-value/

+ `콘솔` 에서 `iex` 치면 아래처럼 나온다.

### `iex` 에서 실행

```
C:\Users\keept>iex
Erlang/OTP 27 [erts-15.0.1] [source] [64-bit] [smp:16:16] [ds:16:16:10] [async-threads:1] [jit:ns]

Interactive Elixir (1.17.2) - press Ctrl+C to exit (type h() ENTER for help)
iex(1)>
```

### vscode에서 실행

+ vscode extension 설치
  - extension ElixirLS: Elixir support and debugger 0.23.1
  - Dash v2.4.0
  
![](https://blog.kakaocdn.net/dn/bG9sHk/btsJcjpBf64/Sq7RV3Qr87yiod7E9dZmh0/img.png)

- 이렇게도 가능

![](https://blog.kakaocdn.net/dn/xFw2M/btsJazSC2UU/w9W2t9tX5A8MXABi6buyZ1/img.png)


### 2-0. 아래 내용 요약

- 가장 기초적인 `type`, `연산`은 언어 공식 doc에 있음
- https://hexdocs.pm/elixir/basic-types.html

- https://joyofelixir.com/7-part-2-recap/
- 위 페이지 언제까지 떠있을지 몰라 아래 요약 함께 넣음

#### 사칙연산 + mod

- 나눗셈 div(), mod연산 rem()

```elixir
iex>div(10,5)
2
iex>rem(10,3)
1
```


#### 비교연산

- `===` 타입까지 비교
```
iex> 2 == 2.0
true
iex> 2 === 2.0
false
```

- `||` `&&` `!` 

```elixir
iex> -20 || true
-20
iex> false || 42
42

iex> 42 && true
true
iex> 42 && nil
nil

iex> !42
false
iex> !false
true
```

---

#### `애텀` (상수)

```elixir
iex> :foo == :bar
false
```

- 모듈 이름도 Atom

```elixir
iex> is_atom(MyApp.MyModule)
true
```

---

#### 정수

```elixir
iex> 2 + 4
6
iex> 3 - 6
-3
iex> 4 * 12345
49380
iex> 1234 / 4
308.5
```

#### string 합치기

```elixir
iex> "aaa" <> "bbb"
aaabbb
```

#### string 전개

```elixir
iex> place = "World"
iex> "Hello #{place}!"
"Hello, World!"
```

#### list (연결리스트임)

```elixir
iex> ["chicken", "beef", "and so on"]
["chicken", "beef", "and so on"]
```

- 앞에 추가하는게 뒤에 추가하는것보다 `빠름`

#### `cons` 연산자 `|`

```elixir
iex> [head | tail] = [3.14, :pie, "Apple"]
[3.14, :pie, "Apple"]
iex> head
3.14
iex> tail
[:pie, "Apple"]
```

#### `tuple`

- 길이 구하기는 빠름 (메모리 연속적으로 저장되므로 )
- 수정은 비쌈 (메모리에 새로 복사해서 씀)




#### 

```elixir
iex> person = %{name: "Izzy", age: "30ish", gender: "Female"}
```

```elixir
iex> greeting = fn (name) -> "Hello, #{name}!" end
#Function<6.52032458/1 in :erl_eval.expr/5>
```

```elixir
iex> %{name: name, age: age} = %{name: "Izzy", age: "30ish"}
%{name: "Izzy", age: "30ish"}

iex> name
"Izzy"

iex> age
"30ish"
```

<br/>

---

<br/>


### integer double 이런 byte연산까지 신경쓸 필요가 없어짐
---
<br/>

```
iex(87)> 2934587239058723085720395871039581039581203985120397529837519387510239571023985 + 1
2934587239058723085720395871039581039581203985120397529837519387510239571023986
```

### string 할당 "Hello #{var}"
---
<br/>

```
iex(1)> place = "world"
"world"

iex(2)> "hello #{place}"
"hello world"
```

### list
---

```
iex(10)> shopping_list = ["bread","egg","ham"]
["bread","egg","ham"]

iex(17)> shopping_list[1]
** (ArgumentError) the Access module does not support accessing lists by index, got: 1

Accessing a list by index is typically discouraged in Elixir, instead we prefer to use the Enum module to manipulate lists as a whole. If you really must access a list element by index, you can Enum.at/1 or the functions in the List module
    (elixir 1.17.2) lib/access.ex:347: Access.get/3
    iex:17: (file)
```

- **index로 접근할 수 없게 만들어놨다.**
- 변수할당 해서 접근할 수 있음.

```
iex(23)> [a|others] = shopping_list
["bread", "egg", "ham"]

iex(24)> a
"bread"

iex(25)> others
["egg", "ham"]
```

- elixir의 구조분해 할당 방법인데, 본 포스트 아래에 계속 기록함.


- insert

```
iex(112)> list = ["a","b","c"]
["a", "b", "c"]

iex(113)> ["d"] ++ list
["d", "a", "b", "c"]
```

```
iex(114)> ["gg","dd","ee"] ++ list
["gg", "dd", "ee", "a", "b", "c"]

iex(115)> list ++ [333]
["a", "b", "c", 333]
```


### tuple

- 선언과 size
```elixir
iex(107)> a = {:ok, "hihi"}
{:ok, "hihi"}

iex(108)> tuple_size(a)
2
```

- 접근
```elixir
iex(109)> elem(a, 1)
"hihi"
iex(110)> elem(a, 0)
:ok
```

<br/>  

---

### map

<br/>

```elixir
iex(10)> person = %{"name" => "Roberto", "age" => 56, "gender" => "Male"}
%{"age" => 56, "gender" => "Male", "name" => "Roberto"}
iex(11)> person["name"]
"Roberto"
iex(12)> person["age"]
56
```


- map 갱신 (**기존 map에 키가 존재할 때만**)
- 갱신할 때 맵은 **복사되어 생성됨**.

```elixir
iex(1)> map = %{one: 1, two: 2}
%{one: 1, two: 2}
iex(2)> map.one
1
# 수정하고싶으면 map 재할당
# | : map 데이터 중 'one' 키의 값은 111임
iex(3)> map = %{map | one: 111} 
%{one: 111, two: 2}
iex(4)> map.one
111
```

- 키 추가는 Map.put/3

```elixir
iex(2)> map = Map.put(map, :hi, "gonnichiwa") # :hi 는 atom
%{foo: "bar", hello: "world", hi: "gonnichiwa"}
```

- 삭제는 Map.delete/2

```elixir
iex(22)> map = Map.delete(map, :a)
%{c: "c", b: "b", d: "d"}
iex(23)> map.a
** (KeyError) key :a not found in: %{c: "c", b: "b", d: "d"}
    iex:23: (file)
```

- 그외 Map 관련 문서는 여기 
- https://hexdocs.pm/elixir/1.12/Map.html



### Enum 모듈

- 열거형은 함수형 프로그래밍의 핵심
- 지연 열거 (Lazy enumeration) 은 `Stream` 모듈임

+ `reduce/3`
  - /1 : 돌릴 list
  - /2 : 초기시작값 (optional)
  - /3 : 계산식

<br/>

- 10 부터 1,2,3 더함 (3 + 2 + 1 + 10)  
```elixir
iex> Enum.reduce([1,2,3], 10, fn(x, acc) -> x + acc end)
16
```

- 1,2,3 더함
```elixir
iex> Enum.reduce([1,2,3], fn(x, acc) -> x + acc end)
6
```

- 문자열 ["a","b","c"] + "1" 붙임
```elixir
iex> Enum.reduce(["a","b","c"], "1", fn(x, acc) -> x <> acc end)
"cba1"
```

+ `min/2`, `max/2`
  - /1 : 돌릴 리스트
  - /2 : 리스트 비었을 경우 리턴할 값 (**익명함수여야함**)

```elixir
iex> Enum.min([], fn -> :baz end)
:baz
```

+ `filter/2`
  - /1 : 돌릴 리스트
  - /2 : `true`로 평가될 요소들

- 짝수인 수
```elixir
iex> Enum.filter([1,2,3,4], fn(x) -> rem(x,2) == 0 end)
[2, 4]
```

+ `all?/2`
  - /1 : 돌릴 list
  - /2 : 반환할 boolean

- ["foo", "bar", "hello"] 에서 길이가 1 이상이면 `true`

```elixir
iex(1)> Enum.all?(["foo","bar","hello"], fn(s) -> String.length(s) > 1 end)
true
```


+ `any?/2`
  - /1 에서 하나라도 조건 맞으면 true

```elixir
iex> Enum.any?(["foo","bar","hello"], fn(s) -> String.length(s) == 5 end)
true
```

+ `chunk_every/2`
  - /1에 주어진 list를
  - /2 수만큼 list 안의 list로 

```elixir
iex> Enum.chunk_every([1,2,3,4,5,6], 2)
[[1, 2], [3, 4], [5, 6]]
```




---

<br/>

### pattern matching (`=`)

- 좌우항 같지 않으면 컴파일 에러
- `=`가 단순한 변수 할당이 아님

```elixir
iex(4)> 5 = 2 + 2
** (MatchError) no match of right hand side value: 4
    (stdlib 6.0.1) erl_eval.erl:652: :erl_eval.expr/6
    iex:4: (file)
```

- 같게 맞춰야 함.  
```elixir
iex(4)> 4 = 2 + 2
4
```

- `tuple`도 마찬가지

```elixir
iex> {:ok, value} = {:ok, "success"}
{:ok, "success"}
iex(2)> value
"success"
iex(4)> {:ok, value} = {:error}
** (MatchError) no match of right hand side value: {:error}
    (stdlib 6.0.1) erl_eval.erl:652: :erl_eval.expr/6
    iex:4: (file)
```

- 패턴매칭은 아래처럼 맵의 인자만 골라낸 패턴 매칭으로 쓸 수 있음.

```elixir
person = %{name: "jack", age: 34, country: "ko"}
defmodule Greeter1 do
  def hello(%{name: person_name} = person) do
    IO.puts "Hello, " <> person_name
    IO.inspect(person)
  end
end

Greeter1.hello(person)
# Hello, jack
# %{name: "jack", age: 34, country: "ko"}
```


### pattern matcing - inside functions 함수내 패턴매칭

- 간단한 함수 선언

```elixir
iex(72)> road = fn
...(72)> "high" -> "You take the high road!"
...(72)> "low" -> "I'll take the low road! (and I'll get there before you)"
...(72)> end
#Function<42.39164016/1 in :erl_eval.expr/6>

```
- `road` fn은 `high` 와 `low` 두개의 `arguments`를 가짐
- 아래처럼 선언한 함수에 접근 (access)

```elixir
iex(73)> road.("high")
"You take the high road!"

iex(74)> road.("low")
"I'll take the low road! (and I'll get there before you)"
```

- 없는 `argument` 호출하면 당연 에러

```elixir
iex(75)> road.("middle")
** (FunctionClauseError) no function clause matching in :erl_eval."-inside-an-interpreted-fun-"/1

    The following arguments were given to :erl_eval."-inside-an-interpreted-fun-"/1:

        # 1
        "middle"

    (stdlib 6.0.1) :erl_eval."-inside-an-interpreted-fun-"/1
    (stdlib 6.0.1) erl_eval.erl:1117: :erl_eval.eval_fun/8
    iex:72: (file)
```

#### map을 끼얹으면?

```elixir
iex(75)> greeting = fn
...(75)> %{name: name} -> "Hello, #{name}!"
...(75)> %{} -> "Hello, Anonymous Stranger!"
...(75)> end
```

- 이렇게 호출

```elixir
iex(79)> greeting.(%{name: "Izzy"})
"Hello, Izzy!"

iex(80)> greeting.(%{name: first_name})
"Hello, Izzy!"
```

```elixir
iex(76)> greeting.(%{})
"Hello, Anonymous Stranger!"
```

---

- `default` 파이프라인은 `_`으로 처리

```elixir
iex(81)> road = fn
...(81)> "high" -> "You take the high road!"
...(81)> "low" -> "I'll take the low road! (and I'll get there before you)"
...(81)> _ -> "Take the 'high' road or the 'low' road, thanks!"
...(81)> end
#Function<42.39164016/1 in :erl_eval.expr/6>

iex(82)> road.("middle")
"Take the 'high' road or the 'low' road, thanks!"

iex(83)> road.("Hhhhhhh")
"Take the 'high' road or the 'low' road, thanks!"

iex(84)> road.(%{})
"Take the 'high' road or the 'low' road, thanks!"

iex(85)> road.([])
"Take the 'high' road or the 'low' road, thanks!"
```


---

### pin 연산자 (`^`)

- `^변수 = 값`
- `^변수는 위 값으로 고정함.`
- 돌려보면 신기함

```elixir
# pin 연산자
greeting = "Hello"
greet = fn
  (^greeting, name) -> "HI, #{name}" #1
  (greeting, name) -> "#{greeting}.. #{name}!" #2
end

IO.puts(greet.("Hello","jjj"));    # HI, jjj #1 호출
IO.puts(greet.("Hellu","hgggg"));  # Hellu.. hgggg!  #2 호출
```
- 고정한 값대로 호출 되었으니 `^greeting` 인자 있는 함수 호출됨.

![](https://blog.kakaocdn.net/dn/06Voa/btsJe8tlSBI/BdJ4gluNM7shjAuP1FL1hk/img.png)

- greet fn 이 갖고있는 param 함수들 중 호출 뭘 할지 정할 때
- `^greeting` 이 붙으면서 `greeting` `값` 보고 어떤 fn param을 호출할 지 결정함

#### pin 연산자 다른 예

```elixir
# pin 연산자
pie = 3.14
case "cherry pie" do
  ^pie -> IO.puts("Not so tasty")
  pie -> IO.puts("I bet #{pie} is so tasty")
end
# result : I bet cherry pie is so tasty
```
- 위 `pie` 값은 3.14로 고정됨.
- `case`에서 주어진 `cherry pie`는 `^pie` 고정값인 3.14가 아니므로
- pie -> "I bet #{pie} is so tasty" 호출됨.


<br/>


### `if`와 `unless`

- if : 우리가 알고 있는 그 if, 조건 `true`
+ unless : 조건 `false`일 때 블록 실행.
  - if(!condition) 이렇게 적지 말라는 뜻, 보기 힘드니까

```elixir
iex(4)> if String.valid?("Hello") do
...(4)> "Valid String"
...(4)> else
...(4)> "Invalid string."
...(4)> end
"Valid String"

iex(5)> if "a string value" do
...(5)> "Truthjy"
...(5)> end
"Truthjy"

iex(6)> unless is_integer("hihi") do
...(6)> "Not an Int"
...(6)> end
"Not an Int"
```

### case

- default : `_`

```elixir
iex(7)> case {1,2,3} do
...(7)> {1,x,3} when x > 0 -> "will match"
...(7)> _ -> "Won't match"
...(7)> end
"will match"
```

### with

<br/>

------

<br/>

- 아래와 같은 맵이 있을 때

```elixir
those_who_are_assembled = [
%{age: "30ish", gender: "Female", name: "Izzy"},
%{age: "30ish", gender: "Male", name: "The Author"},
%{age: "56", gender: "Male", name: "Roberto"},
%{age: "38", gender: "Female", name: "Juliet"},
%{age: "21", gender: "Female", name: "Mary"},
%{age: "67", gender: "Female", name: "Bobalina"},
%{age: "54", gender: "Male", name: "Charlie"},
%{age: "10", gender: "Male", name: "Charlie (no relation)"},
]
```

### 구조분해 할당

```elixir
iex(7)> [first, second, third | others] = those_who_are_assembled

iex(8)> first
%{name: "Izzy", age: "30ish", gender: "Female"}

iex(9)> second
%{name: "The Author", age: "30ish", gender: "Male"}

iex(10)> third
%{name: "Roberto", age: "56", gender: "Male"}

iex(11)> others
[
  %{name: "Juliet", age: "38", gender: "Female"},
  %{name: "Mary", age: "21", gender: "Female"},
  %{name: "Bobalina", age: "67", gender: "Female"},
  %{name: "Charlie", age: "54", gender: "Male"},
  %{name: "Charlie (no relation)", age: "10", gender: "Male"}
]
```

- 한줄로 할당

```elixir
iex(42)> [one,two,three|others] = those_who_are_assembled
...

iex(43)> one
%{name: "Izzy", age: "30ish", gender: "Female"}

iex(44)> two
%{name: "The Author", age: "30ish", gender: "Male"}

iex(45)> three
%{name: "Roberto", age: "56", gender: "Male"}

iex(46)> others
[
  %{name: "Juliet", age: "38", gender: "Female"},
  %{name: "Mary", age: "21", gender: "Female"},
  %{name: "Bobalina", age: "67", gender: "Female"},
  %{name: "Charlie", age: "54", gender: "Male"},
  %{name: "Charlie (no relation)", age: "10", gender: "Male"}
]

```

- 위 `others`를 또 할당

```elixir
iex(47)> [one,two,three|remaining] = others
...

iex(48)> one
%{name: "Juliet", age: "38", gender: "Female"}

iex(49)> two
%{name: "Mary", age: "21", gender: "Female"}

iex(50)> three
%{name: "Bobalina", age: "67", gender: "Female"}

iex(51)> remaining
[
  %{name: "Charlie", age: "54", gender: "Male"},
  %{name: "Charlie (no relation)", age: "10", gender: "Male"}
]
```

- **안남은걸 할당하려 하면 컴파일 에러**

```
iex(52)> [one,two,three|still_remaining] = remaining

** (MatchError) no match of right hand side value: 
[%{name: "Charlie", age: "54", gender: "Male"},
%{name: "Charlie (no relation)", age: "10", gender: "Male"}]
    (stdlib 6.0.1) erl_eval.erl:652: :erl_eval.expr/6
    iex:52: (file)
```

- **map element 갯수에 맞춰서 `|`하고 나머지 할당은 괜찮음**
- still_remaining은 empty길이로 할당됨

```elixir
iex(55)> [one,two|still_remaining] = remaining
[
  %{name: "Charlie", age: "54", gender: "Male"},
  %{name: "Charlie (no relation)", age: "10", gender: "Male"}
]

iex(56)> one
%{name: "Charlie", age: "54", gender: "Male"}

iex(57)> two
%{name: "Charlie (no relation)", age: "10", gender: "Male"}

iex(58)> still_remaining
[]

```

<br/>

---

<br/>

- 이렇게 접근과 할당도 가능

```elixir
iex(65)> person = %{name: "Izzy", age: "30ish"}
%{name: "Izzy", age: "30ish"}

iex(66)> person.name
"Izzy"

iex(67)> person.age
"30ish"
```

---
<br/>

```elixir
ex(69)> [first_person = %{name: first_name} | others] = those_who_are_assembled
[
  %{name: "Izzy", age: "30ish", gender: "Female"},
  %{name: "The Author", age: "30ish", gender: "Male"},
  %{name: "Roberto", age: "56", gender: "Male"},
  %{name: "Juliet", age: "38", gender: "Female"},
  %{name: "Mary", age: "21", gender: "Female"},
  %{name: "Bobalina", age: "67", gender: "Female"},
  %{name: "Charlie", age: "54", gender: "Male"},
  %{name: "Charlie (no relation)", age: "10", gender: "Male"}
]

iex(70)> first_person
%{name: "Izzy", age: "30ish", gender: "Female"}

iex(71)> first_name
"Izzy"

```

- **좀 신기한것**

```elixir
[first_person = %{name: first_name} | others] = those_who_are_assembled
```

- `first_person` 은 `those_who_are_assembled` 의 0번 엘리먼트 객체 담음.
- `first_person` 객체안의 키 `name` 값을 그대로 `first_name` 변수에 할당해버림

- 변수할당의 전역화아닌가? 이래도됨?? 싶지만 어차피 함수 파이프라인 관점으로 접근할거니까 상관없이 설계된듯 싶음


### module, function

```elixir
defmodule Math do
  def sum(a,b) do
    a + b
  end
end
```

- `.ex` 파일을 컴파일해서 쓸 수 있음. (아래처럼)
```
working-dir $ elixirc Math.ex
working-dir $ iex
iex(1)> Math.sum(1,2)
3
```

- 컴파일 하면 `Elixir.Math.beam` 바이트코드 파일 생성되어 이게 메모리 올라가서 실행됨
  
![](https://blog.kakaocdn.net/dn/q1l5J/btsJdGRrO0T/jrk5FxiI3ifBG4YjcjNmnk/img.png)

<br/>

- 고로 `.beam` 파일 만든 경로외에서는 안됨
```
C:\>iex
Erlang/OTP 27 [erts-15.0.1] [source] [64-bit] [smp:16:16] [ds:16:16:10] [async-threads:1] [jit:ns]

Interactive Elixir (1.17.2) - press Ctrl+C to exit (type h() ENTER for help)
iex(1)> Math.sum(1,2)

** (UndefinedFunctionError) function Math.sum/2 is undefined (module Math is not available)
    Math.sum(1, 2)
    iex:1: (file)
iex(1)>
```

- `.exs` 는 scripting 모드로 실행됨 (.beam 안만듬)

![](https://blog.kakaocdn.net/dn/b5ewA1/btsJcRTZrnh/6FU4shMEfnKkd2pCHu58rk/img.png)

- `defp`는 `private`접근. (`defmodule 안에서`)

![](https://blog.kakaocdn.net/dn/bpOOuQ/btsJeqf1Qum/Lk4ACUwwzJr2RffebY1hy1/img.png)


<br/>


- `guard` 처럼 보이지만 `boolean` 형 함수임 (아래)

```elixir
defmodule Math do
  def zero?(0) do
    true
  end

  def zero?(x) when is_integer(x) do
    false
  end
end

IO.puts Math.zero?(0) # true
IO.puts Math.zero?(1) # false
IO.puts Math.zero?([1,2,3]) # ** (FunctionClauseError) no function clause matching in Math.zero?/1
```

- 함수 구문 `do`를 한줄로

```elixir
defmodule Math do
  def zero?(0), do: true
  def zero?(x) when is_integer(x), do: false
  def zero?(x) when is_list(x), do: false
end

IO.puts Math.zero?(0) # true
IO.puts Math.zero?(1) # false
IO.puts Math.zero?([1,2,3]) # false
```


#### 구조체 (defstruct)

- 아래 참조  
https://elixirschool.com/ko/lessons/basics/modules#%EA%B5%AC%EC%A1%B0%EC%B2%B4-2

```elixir
# filename: defstruct.ex
defmodule Example do
  @greeting "hihi"

  def greeting(name) do
    IO.puts ~s(#{@greeting} #{name}.)
  end
end

defmodule Example.User do
  defstruct name: "Sean", roles: []
end
```


```elixir
iex> c("defstruct.ex")
[Example, Example.User, Sayings.Farewells, Sayings.Greetings]

iex> %Example.User{}
%Example.User<name: "Sean", roles: [], ...>

iex> %Example.User{name: "Steve"}
%Example.User<name: "Steve", roles: [], ...>

iex> %Example.User{name: "Steve", roles: [:manager]}
%Example.User<name: "Steve", roles: [:manager]>
```


#### alias, import, require

+ `alias` : defmodule 풀네임 그대로 가져올 때
  - https://elixirschool.com/ko/lessons/basics/modules#alias-4
+ `import` :
  - https://elixirschool.com/ko/lessons/basics/modules#import-5
+ `require` : 컴파일 후 불러옴
  - https://elixirschool.com/ko/lessons/basics/modules#require-7


#### **use**

- 아래 참조  
https://elixirschool.com/ko/lessons/basics/modules#use-8

### 정규식

```elixir
iex> string = "100_000_000"
"100_000_000"

iex> Regex.split(~r/_/, string) # ~r/_/ 에서 ~r은 sigil
["100", "000", "000"]
```

### sigil

+ `sigil`에 대한것은 아래 참조
  - https://elixirschool.com/ko/lessons/basics/sigils
+ `sigil` 기본제공 문법은 여러가지 있음

<br/>

+ 소문자면 식 내용 계산을 수행
+ 대문자면 식 내용 그대로를 출력
+ 예시
```elixir
iex> ~s/welcome to elixir #{String.downcase "SCHOOL"}/
"welcome to elixir school"

iex> ~S/welcome to elixir #{String.downcase "SCHOOL"}/
"welcome to elixir \#{String.downcase \"SCHOOL\"}"
```

```elixir
iex> ~c/2 + 7 = #{2 + 7}/
'2 + 7 = 9'

iex> ~C/2 + 7 = #{2 + 7}/
'2 + 7 = \#{2 + 7}'
```


## 참고

- https://elixirschool.com/ko/lessons/basics/collections


+ Intro
  - [liftIO 2022 : 웹 개발의 모순과 Elixir가 특효약인 이유 - 한국축산데이터 CTO Max(이재철](https://www.youtube.com/watch?v=lAaD-6OQSHE)

+ 요구사항 사례별 예시
  - [그럼에도 불구하고 Elixir: 카카오워크 서버팀의 Elixir 실용주의 프로그래밍 / if(kakao)2022](https://www.youtube.com/watch?v=NotXpRovDoA)

+ fff
  - [[OKKY 3월 세미나] Elixir: 대용량 분산 웹개발의 혁명 (부제: Java / C++ / Python이 OOP 언어가 아닌 이유)](https://www.youtube.com/watch?v=Rjyf_dELAeg)
