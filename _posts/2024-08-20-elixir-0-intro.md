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

```
C:\Users\keept>iex
Erlang/OTP 27 [erts-15.0.1] [source] [64-bit] [smp:16:16] [ds:16:16:10] [async-threads:1] [jit:ns]

Interactive Elixir (1.17.2) - press Ctrl+C to exit (type h() ENTER for help)
iex(1)>
```

- helloworld는 vscode에서 아래처럼 돌려볼 수 있음

![](https://blog.kakaocdn.net/dn/xFw2M/btsJazSC2UU/w9W2t9tX5A8MXABi6buyZ1/img.png)

### 2-0. 아래 내용 요약

- 가장 기초적인 `type`, `연산`은 언어 공식 doc에 있음
- https://hexdocs.pm/elixir/basic-types.html


- https://joyofelixir.com/7-part-2-recap/
- 위 페이지 언제까지 떠있을지 몰라 아래 요약 함께 넣음

```
iex> 2 + 4
6
iex> 3 - 6
-3
iex> 4 * 12345
49380
iex> 1234 / 4
308.5
```

```
iex> place = "World"
iex> "Hello #{place}!"
"Hello, World!"
```

```
iex> ["chicken", "beef", "and so on"]
["chicken", "beef", "and so on"]
```

```
iex> person = %{name: "Izzy", age: "30ish", gender: "Female"}
```

```
iex> greeting = fn (name) -> "Hello, #{name}!" end
#Function<6.52032458/1 in :erl_eval.expr/5>
```

```
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
```
iex(107)> a = {:ok, "hihi"}
{:ok, "hihi"}

iex(108)> tuple_size(a)
2
```

- 접근
```
iex(109)> elem(a, 1)
"hihi"
iex(110)> elem(a, 0)
:ok
```

### map
---
<br/>

```
iex(10)> person = %{"name" => "Roberto", "age" => 56, "gender" => "Male"}
%{"age" => 56, "gender" => "Male", "name" => "Roberto"}
iex(11)> person["name"]
"Roberto"
iex(12)> person["age"]
56
```

### pattern matching (`=`)

- 좌우항 같지 않으면 컴파일 에러
- `=`가 단순한 변수 할당이 아님

```
iex(4)> 5 = 2 + 2
** (MatchError) no match of right hand side value: 4
    (stdlib 6.0.1) erl_eval.erl:652: :erl_eval.expr/6
    iex:4: (file)
```

- 같게 맞춰야 함.  
```
iex(4)> 4 = 2 + 2
4
```

<br/>

------

<br/>

- 아래와 같은 맵이 있을 때

```
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

```
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
```
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

```
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

```
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

```
iex(65)> person = %{name: "Izzy", age: "30ish"}
%{name: "Izzy", age: "30ish"}

iex(66)> person.name
"Izzy"

iex(67)> person.age
"30ish"
```

---
<br/>

```
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

```
[first_person = %{name: first_name} | others] = those_who_are_assembled
```

- `first_person` 은 `those_who_are_assembled` 의 0번 엘리먼트 객체 담음.
- `first_person` 객체안의 키 `name` 값을 그대로 `first_name` 변수에 할당해버림

- 변수할당의 전역화아닌가? 이래도됨?? 싶지만 어차피 함수 파이프라인 관점으로 접근할거니까 상관없이 설계된듯 싶음


### pattern matcing inside functions 함수내 패턴매칭

- 간단한 함수 선언

```
iex(72)> road = fn
...(72)> "high" -> "You take the high road!"
...(72)> "low" -> "I'll take the low road! (and I'll get there before you)"
...(72)> end
#Function<42.39164016/1 in :erl_eval.expr/6>

```
- `road` fn은 `high` 와 `low` 두개의 `arguments`를 가짐
- 아래처럼 선언한 함수에 접근 (access)

```
iex(73)> road.("high")
"You take the high road!"

iex(74)> road.("low")
"I'll take the low road! (and I'll get there before you)"
```

- 없는 `argument` 호출하면 당연 에러

```
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

```
iex(75)> greeting = fn
...(75)> %{name: name} -> "Hello, #{name}!"
...(75)> %{} -> "Hello, Anonymous Stranger!"
...(75)> end
```

- 이렇게 호출

```
iex(79)> greeting.(%{name: "Izzy"})
"Hello, Izzy!"

iex(80)> greeting.(%{name: first_name})
"Hello, Izzy!"
```

```
iex(76)> greeting.(%{})
"Hello, Anonymous Stranger!"
```

---

- `default` 파이프라인은 `_`으로 처리

```
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




## 참고

+ Intro
  - [liftIO 2022 : 웹 개발의 모순과 Elixir가 특효약인 이유 - 한국축산데이터 CTO Max(이재철](https://www.youtube.com/watch?v=lAaD-6OQSHE)

+ 요구사항 사례별 예시
  - [그럼에도 불구하고 Elixir: 카카오워크 서버팀의 Elixir 실용주의 프로그래밍 / if(kakao)2022](https://www.youtube.com/watch?v=NotXpRovDoA)

+ fff
  - [[OKKY 3월 세미나] Elixir: 대용량 분산 웹개발의 혁명 (부제: Java / C++ / Python이 OOP 언어가 아닌 이유)](https://www.youtube.com/watch?v=Rjyf_dELAeg)
