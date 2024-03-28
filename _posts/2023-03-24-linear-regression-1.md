---
title: Liniear regression (선형회귀)
date: 2023-03-23 20:13:00 +0900
categories: [PYTHON, NLP, ML]
tags: [python, nlp, ml, linear regression]  # TAG names should always be lowercase
authors: [gonnichiwa]
---

## 개념과 용어
- 선형회귀 : 알려진 관련 데이터를 사용하여 알수 없는 **종속변수**를 **예측**하는 데이터 분석 기법
+ 독립변수와 종속변수의 관계를 예측함
  - 변수간의 관계가 있다면, 그 값은 함께 변화함
  - 변수가 증가|감소
+ 단순 선형 회귀 : 1개의 독립변수`x`와 종속변수`y`간의 선형관계
  - `y = cx + m` 의 꼴
  - 실데이터 (x,y) = [{1,2},{2,1},{3,4},{4,3}]를 주고 프로그램을 돌려 `c`, `m`의 값을 찾고자 함
  - 찾은 `c`,`m`을 기준으로 다른 독립변수(x)를 줬을 때 나오는 **y가 뭐냐**? (예측)
+ 다중 선형 회귀 : n개의 독립변수들`x1`,`x2`,`xn`과 종속변수`y`간의 선형관계
  - `y = cx1 + cx2 + cxn.. + m`

### 독립변수 : x
- input
- 실제 주어진 값
- 연구자가 의도적으로 변화시키는 값

### 종속변수 : y
- output
- 예측하여 알고자 하는 값

### 문제 예시
+ 코알라의 나이`독립변수` 에 따라 치아 마모 정도`종속변수`는 어느정도 될까?
  - 나이가 많을 수록 치아 마모정도가 높은게 상식이겠으나, 선형회귀를 이용하여 `마모정도 값`를 알고자 함
  - 코알라는 나뭇잎을 씹어먹음
    * 문제의 본질은 `씹는것`
    * 측정이 가능하다면, `하루에 씹는양`과 `치아 마모 정도(누적)`의 관계를 풀어내봄
    

### 풀이 예시
- 단순 선형회귀 (y = cx + m)


## code : 단순 선형 회귀
``` python
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
from sklearn.linear_model import LinearRegression

# np.random.seed(2021)  # seed를 줘서 같은 random 결과를 얻을 수 있도록 함.

X = np.array([1, 2, 3, 4])  # 독립 변수
y = np.array([2, 1, 4, 3])  # 종속 변수 (독립변수에 따라 실제 측정된 값)

data = X.reshape(-1, 1)  # x 배열을 n행(알아서 정하도록 하고), 1열의 행렬로 변환 해라.

model = LinearRegression()  # LinearRegression 모델을 사용하자
model.fit(data, y)  # model에 넣을 x,y데이터를 set
print('model intercept_:', model.intercept_)  # y절편, 1.0000000000000004
print('model coef_:', model.coef_)  # 모델의 기울기, 0.6
'''
so,
y = 0.6 * x + 1.0000000000000004
'''

pred = model.predict(data)  # data 기준 으로 그려진 추세선에 따라 나온 값을 pred에 넣어라
print('pred:', pred)  # pred: [1.6 2.2 2.8 3.4]

plt.scatter(X, y)  # 산점도 그려라
plt.plot(X, pred, color='green')  # 추세선 그려라
plt.show()  # 보여 달라
```


#### 출처
- 선형 회귀란 무엇인가요? https://aws.amazon.com/ko/what-is/linear-regression/
