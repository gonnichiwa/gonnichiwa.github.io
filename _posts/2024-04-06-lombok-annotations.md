---
title: lombok annotations-lombok 어노테이션들
date: 2019-06-26 00:01:00 +0900
categories: [JAVA]
tags: [lombok, java]  # TAG names should always be lowercase
authors: [gonnichiwa]
---

### @Data
@Data = @ToString + @EqualsAndHashCode + @Getter + @Setter + @RequiredArgsConstructor  
@Data는 위의 5개 어노테이션 효과 합친 매우 강력한 아이임.  

### @ToString
@ToString 은 멤버들 toString오버라이드.
@ToString(exclude = "멤버변수이름")으로 특정 멤버를 제외하고 toString()오버라이드 할 수 있음.  

### @Getter, @Setter
@Getter, @Setter는 클래스레벨 뿐만 아니라 **필드레벨에서 생성가능** 하다.  

### @...ArgsContructor
@NoArgsContructor : 파라미터 없는 디폴트 생성자.  
@AllArgsConstructor : 멤버를 모두 포함하는 생성자.  
@RequiredArgsConstuctor : 필수 멤버(final || @NonNull 붙은 멤버들) 넣는 생성자.  

### @UtilityClass
@UtilityClass : 유틸클래스에 붙어줄수있음. 기본생성자 private 처리됨.  
// 리플렉션, 내부 생성자 호출 시 Exception 발생  
// UnsupportedOperationException("This is a utility class and cannot be instantiated");  

```java
@UtilityClass
public class UtilityClass {
	public static int add(int a, int b){
    	return a+b;
    }
}
 
@EqualsAndHashCode
class Domain{
	private int id;
}

@EqualsAndHashCode(callSuper = true)
class User extends Domain {
	private String userName;
    private String password;
}

public class Main{
	public static void main(String[] args){
    	User user1 = new User();
        user1.setId(1L); // setId(int)는 위 Domain 클래스 에서 왔음.
        user1.setUserName("user");
        user1.setPassword("1111");
        
        User user2 = new User();
        user2.setId(2L);
        user2.setUserName("user");
        user2.setPassword("1111");
        
        user1.equals(user2); ////////
        // User클래스에서 @EqualsAndHashCode 어노테이션의 callSuper 인자가 true이면
        // User의 부모클래스인 Domain의 멤버(id)까지 값비교하므로 결과는 'false'
        
        // callSuper 인자가 false이면
        // User클래스의 멤버만 비교하므로
        // 결과는 'true'
    }
}
```

equals는 비교할려는 두 객체의 내용이 동일한지? (동등성)  
  - 동등한 기준으로 비즈니스 로직 처리를 어떻게 수행할건지?
hashcode는 비교할려는 두 객체가 같은 객체인지? (동일성)  
  - 동일한 기준으로 객체들의 모음(Collection) 중복처리 등을 어떻게 수행할건지?

[hashcode에 대해서는 해야 할 얘기가 좀 있음](https://gonnichiwa.github.io/posts/java-hashcode/)  

