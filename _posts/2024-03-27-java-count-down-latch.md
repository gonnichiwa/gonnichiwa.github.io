---
title: How to Use CountDownLatch
date: 2023-03-27 15:20:00 +0900
categories: [JAVA]
tags: [java, countdownlatch]  # TAG names should always be lowercase
authors: [gonnichiwa]
---

## 정의
### latch
1. 최초 입력 한번 받아들이고 저장.
2. 후속 입력으로 입력 이전상태 복원.

## 사용방법
- 생성자(count) 하나 지정하면서 인스턴스 생성
- `countDown()` 호출하여 생성자 count-- 처리
- `await()` 호출하여 `count = 0` 될때까지 메인스레드 멈춰줌.
- 메소드 코드 라인 수행 도중 정하고 싶은 시점에 `countDown()` 호출
+ 하나의 JVM 프로세스 안에서 **글로벌하게 공유**되는 LatchCount 인스턴스임.
  + 메인스레드와 스레드 병렬 작업 수행간 작업 시작과 종료시점을 제어할 수 있음.
    1. Worker 스레드 모두 작업 끝마친 후 메인 실행 시키고 싶을때


### 예제
``` java
package org.example;

import java.util.concurrent.CountDownLatch;

public class Driver {
    public static void main(String[] args) throws InterruptedException {
        CountDownLatch startSignal = new CountDownLatch(1); // worker들을 일괄 실행시킬려고 생성
        CountDownLatch doneSignal = new CountDownLatch(5);  // worker들 갯수, 각 worker가 작업 끝내면 countDown() 호출

        for (int i = 0; i < 5; ++i) // create and start threads
            new Thread(new Worker(startSignal, doneSignal)).start();
        System.out.println("all worker thread started, but all worker won't process before you call startSignal.countDown() below");

        doSomethingElse();            // 이 라인 실행동안 Worker가 await()에 걸려 다음 라인 실행못함.
        startSignal.countDown();      // for로 생성한 스레드를 이때 모두 진행시킴 (startSignal을 0을 만듬으로써)
                                      // CountDownLatch 가 프로세스 글로벌 하게 공유되는 int임을 알 수 있음.
        doneSignal.await();           // doneSignal 이 0 될때까지 기다림.
        doSomethingElse();
        System.out.println("==main end==");
    }

    private static void doSomethingElse() throws InterruptedException {
        // 메인스레드에서 수행하는 작업 mockup : 1초 걸린다 가정
        System.out.println("doSomethingElse start");
        Thread.sleep(1000);
        System.out.println("doSomethingElse end");
    }
}
class Worker implements Runnable {
    private final CountDownLatch startSignal;
    private final CountDownLatch doneSignal;
    Worker(CountDownLatch startSignal, CountDownLatch doneSignal) {
        this.startSignal = startSignal;
        this.doneSignal = doneSignal;
    }
    public void run() {
        try {
            startSignal.await(); // main에서 지정한 startSignal.getCount() 0 될때까지 기다림
            doWork();
            doneSignal.countDown();
        } catch (InterruptedException ex) {} // return;
    }

    void doWork() throws InterruptedException {
        System.out.println("working start");
        Thread.sleep(1000);
        System.out.println("working end");
    }
}
```

### 나는 어디에 쓸수있겠나?
 - API 서비스 레이어의 트랜잭션 데이터 무결성 검증 유닛 테스트

## 참고자료
- [CountDownLatch documentation](https://docs.oracle.com/javase/8/docs/api/java/util/concurrent/CountDownLatch.html)