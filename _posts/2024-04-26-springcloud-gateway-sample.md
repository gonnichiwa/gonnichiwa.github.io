---
title: spring cloud gateway sample 분석
date: 2024-04-26 19:50:00 +0900
categories: [JAVA, SPRING]
tags: [spring cloud, gateway]  # TAG names should always be lowercase
authors: [gonnichiwa]
---

## 작성목적

https://spring.io/blog/2019/07/01/hiding-services-runtime-discovery-with-spring-cloud-gateway  

돌려보고 구조 분석 기록을 위함.

## 실행

https://spring.io/blog/2019/07/01/hiding-services-runtime-discovery-with-spring-cloud-gateway

### downloads

+ [source code](https://github.com/benwilcock/spring-cloud-gateway-demo)
  - 적절한 디렉토리에 clone 받음
  ![](https://blog.kakaocdn.net/dn/6lw8w/btsGZUDDPi2/vellvmnJw7e1PlxVIeQnIk/img.png)
+ [pack](https://buildpacks.io/docs/for-platform-operators/how-to/integrate-ci/pack/)
  - windows면 아래 그림대로 `.zip` 받아서 `exe` 환경변수 넣어서 실행
  - ![](https://blog.kakaocdn.net/dn/bwhDs8/btsGY5eX8iu/YPpPKfkKNFimoAdy6ToFP1/img.png)
  - ![](https://blog.kakaocdn.net/dn/7LvEm/btsGY41lZ1Q/k8dX9dZ2CjLYWuXbGKIwT1/img.png)
  - ![](https://blog.kakaocdn.net/dn/9RHCV/btsG0cKS2lP/W8R7INNGIVGjeBZsbuUrj1/img.png)
+ [docker](https://www.docker.com/products/docker-desktop/)
  - 다운받아 설치
  - ![](https://blog.kakaocdn.net/dn/cpPtnu/btsGZvxtyx8/zm87xYwhDFTllk4wKJLV2K/img.png)

### 소스 변경

오래된 버전이라 바로 실행 시 돌지 않음.
아래처럼 수정

#### pack-images.sh
```shell
#!/bin/bash

echo "Performing a clean Maven build"
./mvnw clean package -DskipTests=true

echo "Setting the default builder for pack"
pack set-default-builder cloudfoundry/cnb:bionic

echo "Packing the Service"
cd service
mvn compile com.google.cloud.tools:jib-maven-plugin:1.3.0:dockerBuild -Dimage=scg-demo-service:latest
#pack build scg-demo-service --env "BP_JVM_VERSION=8.*"
cd ..

echo "Packing the Eureka Discovery Server"
cd registry
mvn compile com.google.cloud.tools:jib-maven-plugin:1.3.0:dockerBuild -Dimage=scg-demo-registry:latest
# pack build scg-demo-registry --env "BP_JVM_VERSION=8.*"
cd ..

echo "Packing the Spring Cloud Gateway"
cd gateway
mvn compile com.google.cloud.tools:jib-maven-plugin:1.3.0:dockerBuild -Dimage=scg-demo-gateway:latest
# pack build scg-demo-gateway --env "BP_JVM_VERSION=8.*"
cd ..
```

#### docker-compose.yml
```yml
version: '3'
services:
  registry:
    image: scg-demo-registry:latest
    container_name: registry
    expose: 
      - "8761"
  greeting-service:
    image: scg-demo-service:latest
    container_name: greeting-service
    expose: 
      - "8762"
    depends_on: 
      - "registry"
    environment:
      - EUREKA_SERVER=http://registry:8761/eureka
  gateway:
    image: scg-demo-gateway:latest
    container_name: gateway
    ports:
      - "127.0.0.1:8080:8760"
    depends_on: 
      - registry
      - greeting-service
    environment:
      - EUREKA_SERVER=http://registry:8761/eureka
```

#### 실행

`runtime-discovery` 디렉토리에서 `pack-images.sh` 실행

- windows라면 `git bash` 에서 실행
```sh
$ ./pack-images.sh
Performing a clean Maven build
[INFO] Scanning for projects...
[INFO] ------------------------------------------------------------------------
[INFO] Reactor Build Order:
[INFO]
[INFO] service                                                            [jar]
[INFO] registry                                                           [jar]
[INFO] gateway                                                            [jar]
[INFO] runtime-discovery-demo                                             [pom]
[INFO]
[INFO] --------------------------< com.scg:service >---------------------------
[INFO] Building service 0.0.1-SNAPSHOT                                    [1/4]

...
[INFO]
[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------
[INFO] Total time:  5.451 s
[INFO] Finished at: 2024-04-26T22:18:00+09:00
[INFO] ------------------------------------------------------------------------
```

- docker-compose.yml 실행
```sh
$ docker-compose up
```

- 이후 `docker-desktop` 띄우면 아래 컨테이너 3개 실행중임을 확인 가능
![](https://blog.kakaocdn.net/dn/lPUU9/btsGZYsxfyk/GSo1NbJFFKrgZt0DGDIVa1/img.png)

> `greeting-service` 의 경우 `connection-refused` 에러 로그가 있을 수 있음.

> `gateway`나 `registry` 구동중에 `service` 구동 완료되어 접속 시도 실패하여 뜸.

> `docker-desktop`에서 `greeting-service`만 `stop` 한뒤 다시 `startup` 하면 에러 메세지 없음 확인 가능.


## 결과 확인

이후 `작성 목적` 목차에 나와있는대로 실행해보면 결과 잘 나옴

First, Check that the Greeting Service is Hidden:  

The Greeting Service operates on port 8762 and is hidden inside the Docker network. Let's try to call it from your favorite browser using http://localhost:8762/greeting.  
You should be told that "the site can't be reached" by your browser.  
This is because the Greeting Service is hidden inside the Docker network (as if it were behind a company firewall).  
It shouldn't be possible for us to talk to the greeting service directly.  Instead, you’ll see an error page similar to the one below.

Next, Access the Greeting Service via the Gateway:
Now, Navigate your browser to http://localhost:8080/service/greeting. You should now get a valid response with content similar to the "Hello, World" JSON shown below:

{ "id": 1, "content": "Hello, World!"}

Now, View the Registry of Services:
The microservices on the Docker network are each registering themselves with the Registry server (this may take a couple of minutes, so be patient). The Registry server acts an address book for the services. If the services move, or if new instances are created, they will add themselves to the registry.

To view the current list of registered services, point your browser at http://localhost:8080/registry. You should see a screen similar to the one below.

![](https://blog.kakaocdn.net/dn/dbLOFg/btsGXRPzoud/XVo8JYDKUcnaScLK3Obxa0/img.png)

