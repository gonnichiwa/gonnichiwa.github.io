---
title: jpa n+1 problem
date: 2024-04-14 14:05:00 +0900
categories: [JAVA, SPRING, JPA]
tags: [java, spring, JPA]  # TAG names should always be lowercase
authors: [gonnichiwa]
---


### 지금까지 이해한것.

+ Member와 Order의 연관관계 있을경우. (1:N)
  - 연관관계 엔티티간 fetch 정책이
  - eager loading 정책일땐 jpql을 사용하여 조회 할 경우 (jpql은 로딩 fetch 전략을 무시하고 동작한다)
  - lazy loading 정책일땐 연관 Order 테이블 검색 시 복수개의 쿼리 실행.
  ```java
  List<Member> members = memberRepository.findMembersWhereIdLessThenEqual(5);
  for(Member member : members) {
      member.getOrders().size(); // N+1
  }
  ```
  + MemberId 범위 수 만큼 Order select 쿼리가 여러번 실행됨.
    - select * from Member where MemberId <= 5 일 때
    - select * from Order where memberId = 1
    - select * from Order where memberId = 2
    - select * from Order where memberId = 3
    - select * from Order where memberId = 4
    - select * from Order where memberId = 5

+ 즉 N+1 문제란
  - 처음 조회된 데이터 rowcount 수**(N)**만큼 연관관계 테이블의 데이터 조회쿼리**(1)** 날리는 문제


### 그래서 사용할 수 있는 개발 정책

#### **페치 조인 사용**
```sql
select m from member m join fetch m.orders
```

```sql
select M.*, O.* from member m INNER JOIN ORDERS O ON M.id = O.id
```
- 중복결과 나올 수 있음.
- jpql `distinct`로 중복제거 필요함.

<br/>

#### **하이버네이트 @BatchSize(int)**
- member 예시 기준 where조건 걸린 member 수 / int 수만큼 쿼리 조건 실행
- 예)
```java
@org.hibernate.annotations.BatchSize(size = 5)
@OneToMany(mappedBy = "member", fetch = FetchType.EAGER)
private List<Order> orders = new ArrayList<Order>();
```

- size 수만큼 in절로 쿼리 들어감
```java
select * from orders
where member_id in (?,?,?,?,?)
```


#### **하이버네이트 SUBSELECT**
```java
@org.hibernate.annotations.Fetch(FetchMode.SUBSELECT)
@OneToMany(mappedBy = "member", fetch = FetchType.EAGER)
private List<Order> orders = new ArrayList<Order>();
```

- size 수만큼 in절의 **서브쿼리**로 들어감
```sql
select O.* from ORDERS o
  where O.MEMBER_ID in (
    select M.ID
    from Member M
    where M.id <= 5
  )
```

#### 즉시로딩의 단점

+ 비즈니스 로직상 필요하지 않은 엔티티를 로딩하는 상황 발생함.
  - (운영기간이 늘수록) 성능문제 발생 가능성 높아짐
  - 복수개의 엔티티 연관구조가 확장될수록 예상하지 못한 SQL이 실행될 수 있음.

#### 그래서

- 가능한 지연로딩 (LAZY) 사용 추천함. (@OneToOne, @ManyToOne의 기본 페치 전략이 즉시로딩이므로 바꿔줄 필요 있음.)


### 참고

https://blog.naver.com/PostView.naver?blogId=whdgml1996&logNo=222026729704&parentCategoryNo=&categoryNo=88&viewDate=&isShowPopularPosts=false&from=postView

[java orm표준 jpa 프로그래밍](https://www.yes24.com/Product/Goods/19040233)