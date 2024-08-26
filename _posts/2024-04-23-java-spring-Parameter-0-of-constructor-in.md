---
title: Parameter 0 of constructor in ..controller required a bean of type '...Service' that could not be found. (junit)
date: 2024-04-13 20:05:00 +0900
categories: [JAVA, SPRING, JUNIT]
tags: [spring, junit]  # TAG names should always be lowercase
authors: [gonnichiwa]
---

### msg

![](https://blog.kakaocdn.net/dn/boVSfr/btsGARWrof1/MtFoTMukXGV2m2Kjhxe5PK/img.png)
![](https://blog.kakaocdn.net/dn/RZ7hB/btsGBRH9pd4/Y19tqXakGBsRM4NxWV5u9K/img.png)
```
Parameter 0 of constructor in ..controller required a bean of type '...Service' that could not be found. (junit)
```

- junit 유닛테스트 작성을 위해 `MockMvc` 호출 후 작업 중 발생함.
- 로컬 구동 후 postman 요청과 응답은 정상동작.

### 원인

- `@WebMvcTest` 시 Controller 불러와도 Controller 객체가 DI받는 `@Service` `@Component`, AOP 어드바이스 등은 불러오지 않는단다.  

> Using this annotation will disable full auto-configuration and instead apply only configuration relevant to MVC tests (i.e. @Controller, @ControllerAdvice, @JsonComponent, Converter/GenericConverter, Filter, WebMvcConfigurer and HandlerMethodArgumentResolver beans but not @Component, @Service or @Repository beans).

- see also  
https://docs.spring.io/spring-boot/docs/current/api/org/springframework/boot/test/autoconfigure/web/servlet/WebMvcTest.html

### 조치, 해결  

@SpringBootTest  
@AutoConfigureMockMvc  
대체하여 테스트 구동 성공.  

![](https://blog.kakaocdn.net/dn/bwye8P/btsGCX8p60F/1vJPIfJERAuWJhIwMnaDd0/img.png)


#### 참고  
https://stackoverflow.com/questions/75709203/spring-boot-controller-test-throws-no-qualifying-bean-of-type-service-name