---
title: java lambda(람다), doublecolon(더블콜론)
date: 2024-04-10 20:07:00 +0900
categories: [JAVA]
tags: [lambda, doublecolon]  # TAG names should always be lowercase
authors: [gonnichiwa]
---

### 작성목적
`java lambda` 예제로 사용법 알 수 있음  
`java doublecolon` 사용 원리

### `lambda`

```java
@Test
public void lambdaeqTest() {
    Printer p = new Printer() {
        @Override
        public String print(String str, String str2) {
            return str + str2 + " good";
        }
    };
    assertThat(p.print("pc", "manner"), is("pcmanner good"));

    Printer p2 = (str, str2) -> str + str2 + " good";
    assertThat(p2.print("pc", "manner"), is("pcmanner good"));

    Printer p3 = (str, str2) -> {
        str += "123";
        return str + str2 + " good";
    };
    assertThat(p3.print("pc","manner"),
            is("pc123manner good"));

    Printer p4 = (String s, String r) -> s + r + " good";
    assertThat(p4.print("pc","manner"), is("pcmanner good"));
}
```
+ `Printer p` : 클라이언트 객체에서 인터페이스의 구현과 바로 사용 
  - java 8 버전 이전 익명 클래스로 사용하는 예시
+ `Printer p2` : `p`의 람다식 적용 버전
  - 복수의 매개변수를 `()`로 묶어 전달하여 사용함.
+ `Printer p3` : 람다식 구현에 멀티라인을 쓸 경우
  - 구현 블록에 `{}`로 감싸 코드 작성
+ `Printer p4` : 람다식 파라미터에 타입 지정함.
  - 가독성이 좀더 향상됨. 
  - 파라미터 타입이 뭔지 Printer 객체를 안들어가봐도 되니까

- `Printer`는 인터페이스 (아래 코드 참조)
```java
//@FunctionalInterface // 람다식 구현되어 쓰인 인터페이스 명시
interface Printer {
    String print(String str, String str2);
    // @FunctionalInterface 는 추상메소드가 하나여야만 한다.
    // 클라이언트 객체에서 본 인터페이스 이용하여 익명클래스 람다식 구현되어 있다면 메소드 추가할 수 없다.
      // 바로 아래줄 주석 풀면 테스트 코드 컴파일 안됨.
//    void print2();
    // 더쓰고 싶다면 이런식 (java 8 이상)
    default String print(String a){
        // ...
        return a;
    }
}
```
+ `@FunctionalInterface` : 람다식 적용할 수 있는 객체 지정 어노테이션
  - 객체 멤버 메소드 하나만 가지고 있다면 굳어 안넣어도 되지만
  - 이 인터페이스 구현한 객체가 람다식을 사용하고 있음을 명시적으로 개발자에게 알려줌.
  - 이는 곧 `이 객체 쓰는 클라이언트로 람다식 익명클래스 쓰면 메소드 한개만 가지고 있을 수 있소` 라는 뜻으로 읽을 수 있음.
  ![](https://blog.kakaocdn.net/dn/dzHGuE/btsGvtfuJv9/GXOkqfEvk1KbHQIVuPptJ0/img.png)
  - 람다 자체가 한개의 메소드를 표현하는 방식이므로 컴파일러는 어떤 메소드를 람다식에 적용해야 할지 알 수 없음 (위처럼 객체의 익명클래스로 써버리면)
  - 그래서 `default method`를 써서 구현하기도 함


<br/>

---

### DoubleColon (`::`)

```java
@Test
public void doubleColon() {
    List<String> list = Arrays.asList("aa","bb","cc","dd");
    list.forEach((item) -> System.out.println(item));
    
    System.out.println("-----");

    List<String> list2 = Arrays.asList("aa","bb","cc","dd");
    list2.forEach(System.out::println);

    // "---" 기준으로 output 결과는 같다.

    // 더블콜론의 특징 : forEach()에서 넘어가는 파라미터 가짓수가 하나(item)임이 보장된다.
    // 메소드 레퍼런스
}
```

```java
class Food {
    private String name;
    public Food(String name){
        this.name = name;
    }
    public String getName() {
        return name;
    }
}

@Test
public void doubleColonConstruct() {
    Function<String, Food> function1 = (String a) -> new Food(a);
    Food food = function1.apply("pizza");
    System.out.println(food.getName());

    Function<String, Food> function2 = Food::new;
    food = function2.apply("apple");
    System.out.println(food.getName());
}
```
- 



<br/>

---


#### import 포함한 전체 예제

##### lambda
```java
import org.junit.Test;

import static org.hamcrest.core.Is.is;
import static org.junit.Assert.assertThat;

public class LambdaTest {

    @Test
    public void lambdaeqTest() {
        Printer p = new Printer() {
            @Override
            public String print(String str, String str2) {
                return str + str2 + " good";
            }
        };
        assertThat(p.print("pc", "manner"), is("pcmanner good"));

        Printer p2 = (str, str2) -> str + str2 + " good";
        assertThat(p2.print("pc", "manner"), is("pcmanner good"));

        Printer p3 = (str, str2) -> {
            str += "123";
            return str + str2 + " good";
        };
        assertThat(p3.print("pc","manner"),
                is("pc123manner good"));

        Printer p4 = (String s, String r) -> s + r + " good";
        assertThat(p4.print("pc","manner"), is("pcmanner good"));
    }

}

//@FunctionalInterface // 람다식을 적용할 수 있는 인터페이스
interface Printer {
    String print(String str, String str2);
    // @FunctionalInterface 는 추상메소드가 하나여야만 한다.
    // 본 인터페이스로 람다식 구현되어 있다면 메소드 추가할 수 없다.
      // 바로 아래줄 주석 풀면 테스트 코드 컴파일 안됨.
//    void print2();
}
```

##### doubleColon(`::`)
```java
import org.junit.Test;

import java.util.Arrays;
import java.util.List;
import java.util.function.Function;

public class LambdaDoubleColonTest {
    @Test
    public void doubleColon() {
        List<String> list = Arrays.asList("aa","bb","cc","dd");
        list.forEach((item) -> System.out.println(item));
        
        System.out.println("-----");

        List<String> list2 = Arrays.asList("aa","bb","cc","dd");
        list2.forEach(System.out::println);

        // "---" 기준으로 output 결과는 같다.

        // 더블콜론의 특징 : forEach()에서 넘어가는 파라미터 가짓수가 하나(item)임이 보장된다.
        // 메소드 레퍼런스
    }

    @Test
    public void doubleColonConstruct() {
        Function<String, Food> function1 = (String a) -> new Food(a);
        Food food = function1.apply("pizza");
        System.out.println(food.getName());

        Function<String, Food> function2 = Food::new;
        food = function2.apply("apple");
        System.out.println(food.getName());
    }
}

class Food {
    private String name;
    private String name2;
    public Food(String name){
        this.name = name;
    }
    public Food(){

    }
    public String getName() {
        return name;
    }

    public String getName2() {
        return name2;
    }
}
```