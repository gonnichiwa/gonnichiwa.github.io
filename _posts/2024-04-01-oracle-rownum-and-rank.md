---
title: oracle ROW_NUMBER() 와 RANK()를 선택해야 할 때.
date: 2024-04-02 07:13:00 +0900
categories: [ORACLE,FUNCTION]
tags: [oracle,db,partition by,row_number,rank]  # TAG names should always be lowercase
authors: [gonnichiwa]
---

## 왜?
- PARTITION BY `UITEM_ID` ORDER BY `APPLY_DT` 의 기준이 동률일때,  
ROW_NUMBER()를 써서 공동순위를 없앨건지?
RANK()|DENSE_RANK() 를 써서 공동순위를 써야할건지의 판단,  
ROW_NUMBER()는 `동률없는 정렬순서`가 보장되는지부터 확인해야함.

### CASE
햄버거 가게들의 `세트메뉴`+`단품`이 ITEM_ID고  
`단품`만 있는게 `UITEM_ID`  
`UITEM_ID`들의 조합으로 `ITEM_ID`가 만들어 지기도 함.  
`UITEM_ID`의 `SALE_PRICE`는 수시로 바뀔 수 있으므로 수정할때 `APPLY_DT`, 최신날짜로 인서트함.


### 요구
`ITEM_ID`별로 SALE_PRICE(가격)을 내달라.  


### 구현방안
`PARTITION BY`로 그룹핑할 데이터(`UITEM_ID`) `13,34,68`있음  
order by 할 데이터 `APPLY_DT`의 최근(최신가격) 순으로 단품(`UITEM_ID`)를 sum하여
뿌리면 그만으로 보임.

```sql
WITH u AS (
	SELECT 
		ROW_NUMBER() OVER (PARTITION BY up.UITEM_ID ORDER BY up.APPLY_DT DESC) AS rn
		, up.ITEM_ID
		, up.UITEM_ID
		, up.APPLY_DT
		, up.SALE_PRICE
		, up.DISCOUNT_PRICE
		, up.COST_PRICE
		, up.REG_ID
		, up.REG_DTS
	FROM UITEM_PRICE up --WHERE up.ITEM_ID IN (13,34,68,92)
)
SELECT 
     u.ITEM_ID
    ,sum(u.SALE_PRICE)
FROM u
WHERE 1=1
AND u.rn = 1
GROUP BY u.ITEM_ID
;
```
돌렸더니 92개 제품이 나와야 할 것이 75개만 나옴.  
세트는 나오지 않음.  
에러 없음.  
서브쿼리에 세트메뉴 ID 한개를 넣으면 세트 가격이 나오나  
전체 메뉴(단품+세트)를 돌리면 세트가 안나옴.

## 원인
ROW_NUMBER()는 동일 값(기준)에 대한 순위를 내기 부적합하다.  
사용 목적 자체가 일관된 결과를 얻으려면 쿼리에서 결정적인 `정렬순서`를 보장해야 하기 때문.
상위 N 하위 N 및 내부 N 보고를 하기 위해서는 `동률없는 정렬순서`가 보장되는지부터 확인해야함.

https://docs.oracle.com/en/database/oracle/oracle-database/19/sqlrf/ROW_NUMBER.html#GUID-D5A157F8-0F53-45BD-BF8C-AE79B1DB8C41

쿼리에 따라서 `FROM` - `WHERE` - `GROUP BY` 로 이어지는 쿼리 수행 결과와도 맞지 않는다.

- ROW_NUMBER()  `RN`  
<br/>

|RN|ITEM_ID|UITEM_ID|APPLY_DT|SALE_PRICE|
|--|-------|--------|--------|----------|
|**1**|**13**|**13**|**20130115**|**2600**|
|**1**|**34**|**34**|**20130115**|**1700**|
|**1**|**92**|**68**|**20130115**|**2000**|
| 3|     13|      13|20120101|      2300|
| 3|     92|      68|20120101|      1700|

위 쿼리대로라면 `업무상 틀린 값`이더라도 92가 나와야 하는데 나오지 않았다.  
ROW_NUMBER() 자체의 내부 메커니즘 문제가 아닐까?  
확실한 정렬기준이 보장되지 않았기 때문에.

---

- RANK()  `RN`
<br/>

|RN|ITEM_ID|UITEM_ID|APPLY_DT|SALE_PRICE|
|--|-------|--------|--------|----------|
|**1**|**13**|**13**|**20130115**|**2600**|
|**1**|**34**|**34**|**20130115**|**1700**|
|**1**|**68**|**68**|**20130115**|**2000**|
|**1**|**92**|**13**|**20130115**|**2600**|
|**1**|**92**|**34**|**20130115**|**1700**|
|**1**|**92**|**68**|**20130115**|**2000**|
| 3|     13|      13|20120101|2300|
| 3|     92|      68|20120101|1700|


<br/>

## 결론

정렬 기준에 동률이 있거나 있을것 으로 판단된다면 RANK()를 쓰자.