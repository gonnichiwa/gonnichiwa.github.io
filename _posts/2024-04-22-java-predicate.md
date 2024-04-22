---
title: java modifiers - protected, default
date: 2024-04-22 17:34:00 +0900
categories: [JAVA, SPRING]
tags: [predicate, lambda, spring cloud gateway]  # TAG names should always be lowercase
authors: [gonnichiwa]
---

## Predicate (구글번역)
1. 
the part of a sentence or clause containing a verb and stating something about the subject (e.g., went home in John went home ).  
동사를 포함하고 주제에 관해 무엇인가를 기술하는 문장이나 절의 일부(예: John goes home 에서 goes home ).

2. 
state, affirm, or assert (something) about the subject of a sentence or an argument of a proposition.
문장의 주어나 명제의 논증에 관해 진술하고, 단언하고, 단언합니다.  
a word that predicates something about its subject  
주제에 관해 무엇인가를 서술하는 단어

3. 
found or base something on.  
무언가를 발견하거나 기반으로 삼습니다.  
the theory of structure on which later chemistry was predicated  
후기 화학의 기초가 된 구조 이론  

...

- (코드)에서의 적용례를 보면 2. 번의 번역이 적절해 보인다.
- 주제(변수)에 관해 참거짓을 서술(리턴)한다는 뜻에서 


## 예제
- simple 1
```java
@Test
public void predicate(){
    // Creating predicate
    Predicate<Integer> lesserthan = i -> (i < 18);

    // Calling Predicate method
    System.out.println(lesserthan.test(18)); // false
    System.out.println(lesserthan.test(16)); // true

}
```
- lesserthan.test(int) 로 참거짓 구분함.

- simple 2 (chaining)
```java
@Test
public void predicateChaining() {
    Predicate<Integer> greaterThanTen = (i) -> i > 10;

    // Creating predicate
    Predicate<Integer> lowerThanTwenty = (i) -> i < 20;
    boolean result = greaterThanTen.and(lowerThanTwenty).test(15);
    System.out.println(result); // true

    // Calling Predicate method
    boolean result2 = greaterThanTen.and(lowerThanTwenty).negate().test(15);
    System.out.println(result2); // false
    boolean result2_1 = greaterThanTen.and(lowerThanTwenty).negate().test(21);
    System.out.println(result2_1); // true

    Predicate<String> isHangulIncluded = str -> {
        return str.matches(".*[ㄱ-ㅎㅏ-ㅣ가-힣]+.*");
    };
    boolean result3 = isHangulIncluded.test("nameabc안");
    boolean result3_1 = isHangulIncluded.negate().test("nameabc안");
    System.out.println(result3); // true
    System.out.println(result3_1); // false
}
```
- `result1` : and(predicate) 조건의 표현에 test(value) 넣음.
- `result2` : negate()로 연산 결과를 뒤집는다.  
`result2_1`을 보면 21은 `lowerThenTwenty` 조건에 false지만 negate()로 결과를 뒤집는 것을 볼 수 있음.
- `result3` : 한글 포함 여부와 구현부`{}`를 블록처리 해봤다, 한글 집어넣어보니 결과는 true.
- `result3_1` : negate() 확인을 위해 true인 결과가 반대로 처리됨을 볼 수 있음.


### 조건 로직
```java
@Test(expected = IllegalArgumentException.class)
    public void existNumber(){
        pred(3, i -> i < 7); // Number : 3
        pred(15, i -> i < 7); // IllegalArgumentException
    }

    private void pred(int number, Predicate<Integer> pred){
        if(pred.test(number)){
            System.out.println("Number : " + number);
        } else {
            throw new IllegalArgumentException("not matched number condition");
        }
    }
```
- Predicate<T>를 인자로 조건 false일 시 예외 발생하는 테스트


### collection으로 filterling 예시

```java
import static org.hamcrest.core.Is.is;
import static org.junit.Assert.assertThat;

@Test
public void predicateCollection(){
    // User의 role이 ADMIN인 것들..

    // when
    List<User> users = new ArrayList<>();
    users.add(new User("a","ADMIN"));
    users.add(new User("b","USER"));
    users.add(new User("c","USER"));

    // then
    List<User> admins = filtering(users, (User u) -> u.getRole().equals("ADMIN"));

    assertThat(admins.size(), is(1)); // true
}

private List<User> filtering(List<User> users, Predicate<User> pred){
    List<User> l = new ArrayList<>();
    for(User u : users){
        if(pred.test(u)){
            l.add(u);
        }
    }
    return l;
}

class User {
    String name;
    String role;

    public User(String name, String role) {
        this.name = name;
        this.role = role;
    }

    public String getRole() {
        return role;
    }
}
```
- filterling 조건을 client에서 명시하여 쓸 수 있음.
- 어디서 많이 본거 같은데? 람다식의 filterling 예제가 Predicate를 쓴것.
```java
List<User> us = users.stream()
                .filter((User u) -> u.getRole().equals("USER"))
                .collect(Collectors.toList());
assertThat(us.size(), is(2)); // true
```


### 참고
https://docs.oracle.com/javase/8/docs/api/java/util/function/Predicate.html
https://www.geeksforgeeks.org/java-8-predicate-with-examples/