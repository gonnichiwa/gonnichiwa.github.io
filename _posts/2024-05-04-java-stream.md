---
title: java stream
date: 2024-05-04 10:15:43 +0900
categories: [JAVA]
tags: [lambda, stream, predicate, optional]  # TAG names should always be lowercase
authors: [gonnichiwa]
---

## 작성 목적

stream 데이터 생성 가공 표현 관련 여러 예제를 작성함.


## 작업 분류

1. 생성하기    : 스트림 인스턴스 생성
2. 가공하기    : filtering, mapping
3. 결과 만들기 : 가공한 결과 `어떻게` 처리할건지?

### 0. data given

```java
import org.junit.Before;
import org.junit.Test;

import java.util.Arrays;
import java.util.HashMap;
import java.util.stream.Stream;

public class StreamTest {
    String[] arr;
    HashMap<Integer, String> data;
    @Before
    public void given(){
        arr = new String[]{"Java", "Scala", "Groovy", "Python", "Go", "Swift"};
        data = new HashMap<>();
        data.put(1, "Java");
        data.put(2, "Scala");
        data.put(3, "Groovy");
        data.put(4, "Python");
        data.put(5, "Go");
        data.put(6, "Swift");
    }
}

```

### 가공과 표현


#### stream 생성 .stream()

```java
@Test
public void arrayStream(){
    Stream<String> stream = Arrays.stream(arr);
    // 가공, 표현(forEach)
    stream.map(String::toUpperCase)
            .forEach(x -> System.out.print(x + " ")); // JAVA SCALA GROOVY PYTHON GO SWIFT SCALA GROOVY

    Arrays.stream(arr, 1, 3) // arr[1], arr[2]
            .map(String::toUpperCase)
            .forEach(x -> System.out.print(x + " ")); //SCALA[1] GROOVY[2]
}
```

#### 스트림 값|데이터 추가 : add()

```java
@Test
public void 스트림_값추가(){
    Stream<String> builderStream =
            Stream.<String>builder()
                    .add("Eric").add("Elena").add("Java")
                    .build();
    // print
    builderStream.forEach(System.out::println); // [Eric, Elena, Java]
}
```

#### limit stream 생성 : generate(lambda).limit(maxSize);

```java
@Test
public void limit으로_stream_생성(){
    Stream<String> generatedStream =
            Stream.generate(() -> "gen").limit(5);

    // print
    generatedStream.forEach(System.out::println); // [gen, gen, gen, gen, gen]
}
```

#### 스트림 연결 : concat(stream, stream)

```java
@Test
public void 스트림_연결하기() {
    Stream<String> stream1 = Stream.of("Java", "Scala", "Groovy");
    Stream<String> stream2 = Stream.of("Python", "Go", "Swift");
    Stream<String> concat = Stream.concat(stream1, stream2);
    // print
    concat.forEach(System.out::println); // [Java, Scala, Groovy, Python, Go, Swift]
}
```

#### collect(Collectors.joining())

```java
@Test
public void collectors(){
    String[] str = {"a","b","c"};
    String result = Arrays.stream(str).collect(Collectors.joining());
    String result2 = String.join("1", str);
    assertThat(result, is("abc"));
    assertThat(result2, is("a1b1c"));
}
```

#### iterate : iterate(초기값, 식).limit(long 최대크기);

```java
@Test
public void iterate_stream(){
    Stream<Integer> iteratedStream =
            Stream.iterate(30, n -> n + 2).limit(5);
    // print
    iteratedStream.forEach(System.out::println); // [30, 32, 34, 36, 38]
}
```

#### 기본타입형 : IntStream, LongStream

- range(), 종료값 포함 rangeClose()
- Stream\<Wrapper\>로 boxing 가능.

```java
@Test
public void primitiveWrapperStream(){
    IntStream intStream = IntStream.range(1,5);
    LongStream longStream = LongStream.rangeClosed(1L,5L);
    DoubleStream doubleStream = new Random().doubles(3);
    // print
    intStream.forEach(System.out::println); // 1,2,3,4
    longStream.forEach(System.out::println); // 1L,2L,3L,4L,5L
    doubleStream.forEach(System.out::println);

    Stream<Integer> integerStream = IntStream.range(1,5).boxed();
    integerStream.forEach(System.out::println);
}
```

- 위 `intStream` 변수 그대로 `boxed()` 할 경우 error 발생함.
> java.lang.IllegalStateException: stream has already been operated upon or closed

```java
Stream<Integer> integStream = intStream.boxed(); // java.lang.IllegalStateException: stream has already been operated upon or closed
```
- 이미 수행됐던것에는 더 이상 다른 뭔가를 할 수 없도록 함.

- 그래서 스트림 재사용 방법을 아래처럼 구현함

```java
@Test
public void stream_재사용(){
    // List로 데이터 가지고 있다가
    List<String> list = Stream.of("Eric", "Elena", "Java")
            .filter(name -> name.contains("a"))
            .collect(Collectors.toList());
    // stream 생성해서 바로 씀.
    Optional<String> firstElement = list.stream().findFirst();
    Optional<String> anyElement = list.stream().findAny();

    firstElement.ifPresent(name -> System.out.printf("firstElement=%s \n", name)); // firstElement=Elena 
    anyElement.ifPresent(name -> System.out.printf("anyElement=%s", name)); // anyElement=Elena
}
```

<br/>

#### 정규식으로 문자열 자르고 요소별 스트림 생성

```java
@Test
public void 정규식_문자열_split_and_stream(){
    Stream<String> strStream = Pattern.compile(", ")
                        .splitAsStream("Eric, Elena, Java");
    // print
    strStream.forEach(System.out::println);
}
```

#### **reduce**

+ 인자(param) 갯수에 따라 (1,2,3순, `2`와 `1`은 언어마다 순서 바뀔 수 있음)
  1. accumulator(누산로직)
  2. identity(초기값)
  3. combiner(패러랠계산결과합치기)

> 1개일때 (accumulator)

```java
@Test
public void reduceTestOne(){
    int sum = Stream.of(1, 2, 3, 4, 5)
            .reduce((a, b) -> a + b + 1)
            .get();
    /*
    * 4 ==  (a=1  + b=2) + 1
    * 8 ==  (a=4  + b=3) + 1
    * 13 == (a=8  + b=4) + 1
    * 19 == (a=13 + b=5) + 1
    * */
    System.out.println(sum); // 19 (4->8->13->19)
}
```

> 2개일때 (identity, accumulator)

```java
@Test
public void reduceTestTwo(){
    int sum = Stream.of(1, 2, 3, 4, 5)
            .reduce(10, (a, b) -> a + b + 1);

    /*
    * 12 == (a=10 + b=1) + 1
    * 15 == (a=12 + b=2) + 1
    * 19 == (a=15 + b=3) + 1
    * 24 == (a=19 + b=4) + 1
    * 30 == (a=24 + b=5) + 1
    * */
    System.out.println(sum); // 30
}
```

> 3개일때 (identity, accumulator, combiner)

```java
@Test
public void reduceTestThree(){
    int sum = Stream.of(1, 2, 3, 4, 5)
            .parallel()
            .reduce(0
                    , (a, b) -> a + b
                    , (a, b) -> {
                        System.out.printf("combiner %d, %d was called: %d %n", a, b, (a+b));
                        return a + b;
                    });
    // 초기값 0 : 15 (0+1) 1, (1+2) 3, (3+3) 6, (6+4) 10, (10+5) 15
    System.out.println(sum);
    /*
    combiner 1, 2 was called: 3 
    combiner 4, 5 was called: 9 
    combiner 3, 9 was called: 12 
    combiner 3, 12 was called: 15 
    15
    */
}
```

- `parallel()` 에서 `combiner`가 호출됨. (안넣으면 `combiner` 호출없음)
+ 결과 로그 찍히는걸로 미루어 아래와 같이 병렬계산됨을 보임.
  - `Stream.of(1, 2, 3, 4, 5)` 에서
  1. 1+2 계산    3
  2. 4+5 계산    9
  3. 3 + `2.(9)` 계산 12
  4. `1.(3)` + `3.(12)` 계산 15
- 병렬연산을 가능케함

<br/>

#### LazyInvocation

- `stream.collect(), forEach()`와 같이 정의된 스트림이 `동작수행 해야 할 때` 정의된 스트림 실행됨.

```java
@Test
public void lazyInvocationTest() {
    List<String> list = Arrays.asList("Eric", "Elena", "Java");
    // 스트림 정의만 했을 때.
    Stream<String> stream = list.stream()
            .filter(el -> {
                Counter.wasCalled();
                return el.contains("a");
            });
    // .filter() 수행없음
    System.out.println(Counter.counter); // 0

    // collect()로 정의한 stream 동작
    List<String> data = stream.collect(Collectors.toList());
    // .filter() 동작확인
    System.out.println(Counter.counter); // 3
}
```

## Collection

- Collections.swap

```java
int[] arr = {4,3,5,2,3,6};
List<Integer> nums = Arrays.stream(arr).boxed().collect(Collectors.toList());
Collections.swap(nums, 1,2);
System.out.print(nums); // [4, 5, 3, 2, 3, 6]
```



## 감상

+ 배열과 컬렉션의 함수형 처리 가능하게 함으로써  
  - 데이터 `처리`와 `표현`이 특화 분리된 기능 설계를 가능하게 함.  
  - 여기서 `처리`란 데이터 그 자체의 `filtering`, `sorting`, `mapping` 등 있겠음  

  <br/>

+ 실무에서의 복잡한 ~~여러개 테이블로 조인된 db 한방쿼리와 난무하는 프로시저~~ 쿼리로직과 modify 어려운 설계구조를 개선할 좋은 재료
  - application단의 `persistance`계층에서 에서 호출, 생성하는 db쿼리들을 최대한 단순화
  - 이후 `service`단에서 함수형으로 데이터 stream 생성 조합후 딜리버리
  - `serviceImpl` 레이어의 비즈로직과 
  - **대용량 데이터로딩 시점 app단의 memory usage 측정 필요하겠음**
  - Collectors.summarizingInt(), Collectors.averageInt(), Matching(anyMatch(), allMatch()), 외 그룹핑(Collectors.groupingBy()) 등으로 쿼리 보조가 가능하겠음.


## 관련 용어

불변성 : 함수 외부에서 데이터 수정이 없음을 보장함.


## 참고

[Java 스트림 Stream (1) 총정리](https://futurecreator.github.io/2018/08/26/java-8-streams/)  
[Java 스트림 Stream (2) 고급](https://futurecreator.github.io/2018/08/26/java-8-streams-advanced/)
[함수형 프로그래밍 특장점](https://yoondii.tistory.com/124)
