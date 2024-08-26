---
title: spring cloud gateway security 용어 정리
date: 2024-04-29 10:15:43 +0900
categories: [JAVA, SPRING]
tags: [spring cloud, gateway, spring security]  # TAG names should always be lowercase
authors: [gonnichiwa]
---

## 작성목적 

spring cloud, security 배우면서 나오는 용어의 요약 해석  


## 용어들

### `gateway`  

- API 를 거치는 포탈. (all Inbound, outbound)
- configuration : URI 요청 predicate 해서 어느 서비스로 요청 보내줄건지?
  예) spring.cloud.gateway.routes: 에서 predicate에 따라 처리됨  
- `routes`는 `논리 이름`을 사용함. : 같은 api 서비스끼리 `로드밸런싱`를 용이하게 함.

```yaml
spring:
  application:
    name: gateway  
  cloud:
    gateway:
      routes:
      - id: service
        uri: lb://service
        predicates:
        - Path=/service/**
        filters:
        - StripPrefix=1
      - id: registry
        uri: lb://registry
        predicates:
        - Path=/registry/**
        filters:
        - StripPrefix=1
      - id: eureka
        uri: lb://registry
        predicates:
        - Path=/eureka/**
```


### `registry`

- `service-discovery`역할 (서비스 ip, port 전화번호부)
- `gateway` 가 가지고 있는 api routes에 가지고 있는 `논리이름` 에 해당하는 서버
- `gateway`와 loadbalancing(lb)하는 복수개의 `service` 서버들이 `scale out` 될 때, 자동으로 연결될 수 있도록 함


### `service`

- REST API 서비스
- `registry`는 `client`로 받음.

```yml
eureka:
  client:
    registerWithEureka: true
    serviceUrl:  
      defaultZone: ${EUREKA_SERVER:http://localhost:8761/eureka} 
      # EUREKA_SERVER : docker-compose.yml 의 env
    healthcheck:
      enabled: true
```


### `service-discovery`

- 클라우드 MSA 환경에서 `service api`가 동적으로 scale out (서버댓수 늘어남) 될 때
- 늘어난 서버들의 실제 사용을 위해서는 __어딘가엔__ ip port정보를 업데이트 해야 하니까
- API Gateway나, loadbalancer에서 늘어난 서버들의 IP, port정보를 수동 업데이트 해 줘야 했음.
- scale out 빈도가 증가하면서 이를 자동화 함
- 어쩌다가? 각종 쿠폰이벤트 등으로 사용자가 몰리는 타이밍이 자주 생김

<br/>

- 구현 방식은 `server-side`와 `client-side` 로 나뉨.
+ `기준`
  - `api`가 타 `api` 호출할 때
  - 네트웍 내 물리 loadbalancer로 호출하는지? (`server side`)
  + registry에 직접 질의한뒤 얻은 접속정보가지고 접속하는 지 하는지? (`client side`)
    - eureka

<br/>

## 참고

https://spring.io/blog/2019/07/01/hiding-services-runtime-discovery-with-spring-cloud-gateway  
https://spring.io/blog/2019/08/16/securing-services-with-spring-cloud-gateway  