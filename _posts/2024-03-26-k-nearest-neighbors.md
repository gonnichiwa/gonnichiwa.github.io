---
title: k-Nearest Neibhors (k-최근접 이웃 알고리즘1)
date: 2023-03-26 14:45:00 +0900
categories: [PYTHON, SCIKIT-LEARN, ML]
tags: [python, nlp, ml, scikit-learn, knn]  # TAG names should always be lowercase
authors: [gonnichiwa]
---

## k-최근접 이웃 분류

- `분류` : `샘플`을 몇개의 `클래스` 중 하나로 분류

## k-최근접 이웃을 사용하여 도미와 빙어를 구분하기
- 생선의 종류`클래스` 중 하나를 구별해 내는 문제를 `분류`라고 함. (classification)
- 본문 제목처럼 `2개 중 하나`를 구별하는 문제를 `이진 분류`(binary classification)이라고 함.
- 생선길이(25.4), 생선무게(242.0) 같은 데이터 들을 `특성` 이라고 함. (`길이`특성, `무게`특성)

```python
import matplotlib.pyplot as plt
# 도미 길이
bream_length = [25.4, 26.3, 26.5, 29.0, 29.0, 29.7, 29.7, 30.0, 30.0, 30.7, 31.0, 31.0, 
                31.5, 32.0, 32.0, 32.0, 33.0, 33.0, 33.5, 33.5, 34.0, 34.0, 34.5, 35.0, 
                35.0, 35.0, 35.0, 36.0, 36.0, 37.0, 38.5, 38.5, 39.5, 41.0, 41.0]
# 도미 무게
bream_weight = [242.0, 290.0, 340.0, 363.0, 430.0, 450.0, 500.0, 390.0, 450.0, 500.0, 475.0, 500.0, 
                500.0, 340.0, 600.0, 600.0, 700.0, 700.0, 610.0, 650.0, 575.0, 685.0, 620.0, 680.0, 
                700.0, 725.0, 720.0, 714.0, 850.0, 1000.0, 920.0, 955.0, 925.0, 975.0, 950.0]

# 빙어 길이
smelt_length = [9.8, 10.5, 10.6, 11.0, 11.2, 11.3, 11.8, 11.8, 12.0, 12.2, 12.4, 13.0, 14.3, 15.0]
# 빙어 무게
smelt_weight = [6.7, 7.5, 7.0, 9.7, 9.8, 8.7, 10.0, 9.9, 9.8, 12.2, 13.4, 12.2, 19.7, 19.9]

# arr lengths
print('bream_length={}'.format(len(bream_length)))
print('bream_weight={}'.format(len(bream_weight)))
print('smelt_length={}'.format(len(smelt_length)))
print('smelt_weight={}'.format(len(smelt_weight)))


plt.scatter(bream_length, bream_weight) # 도미데이터 산점도 그려라 (x축 데이터들, y축 데이터들)
plt.scatter(smelt_length, smelt_weight) # 빙어데이터 산점도 그려라
plt.xlabel('length')
plt.ylabel('weight')
plt.show()
```

- 결과
```
bream_length=35
bream_weight=35
smelt_length=14
smelt_weight=14
```
![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fcigycg%2FbtsF7QaY4Zm%2FbFZ1xrJSfCmc9594OjnwdK%2Fimg.png)

- 위 그래프처럼 일직선에 가까운 형태로 나타나는 경우를 `선형`(linear) 적 이라고 말함.

- 그래프 보니까
- 도미는 길이가 길수록 무게가 늘어난다
- 빙어는 길이가 길어져도 무게 차이가 별로 안난다.

- k-최근접 이웃(k-Nearest Neightbors)알고리즘을 사용해 도미와 빙어 데이터를 구분해보자.

```python
# 오 위 코드조각에 연계되어서 실행된다 쩐당
length = bream_length + smelt_length # length 도미 + 빙어
weight = bream_weight + smelt_weight # weight 도미 + 빙어
print('length= {}'.format(length))
print('weight= {}'.format(weight))

# scikit-learn 쓰기위해서 2차원 배열로 만듬.
fish_data = [[l,w] for l,w in zip(length, weight)] # zip() : 나열된 리스트 각각에서 하나씩 elem 꺼내 리턴해줌
print('fish_data={}'.format(fish_data))

# 일단 도미를 1 빙어를 0으로 정함
# bream_length(weight) 35개, smelt_length(weight) 14개니까
# 그 배열방 만큼 도미(1), 빙어(0) 인 target 배열 만들어줌.
# 머신러닝 세계에서는 이진분류 문제에서 찾으려는 대상을 1, 아닌걸 0으로 놓음.
fish_target = [1] * 35 + [0] * 14
print('fish_target={}'.format(fish_target))

# k-최근접 이웃 알고리즘 구현한 KNeighborsClassifier 임포트
# 특정 클래스(sklearn.neighbors.KNeighborsClassifier)만 임포트함.
from sklearn.neighbors import KNeighborsClassifier
kn = KNeighborsClassifier()
kn.fit(fish_data, fish_target) # 이것이 훈련. 주어진 데이터로 알고리즘을 사용해 훈련 시킴.

# 이제 평가의 시간
kn.score(fish_data, fish_target) # 1.0, 이 값이 정확도.
```

- 실행결과
```
length= [25.4,...19.7, 19.9]
fish_data=[[25.4, 242.0], ..., [14.3, 19.7], [15.0, 19.9]]
fish_target=[1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0]
1.0
```


### 여기서 사용한 k-최근접 이웃 알고리즘.

- 어떤 데이터에 대한 답을 구할 때
- `주위의 다른 데이터(들)`을 보고 `다수`를 차지하는 것을 정답으로 사용함.
- `주위의` 데이터로 `주어진 데이터가 뭔지`를 판단함

- 테스트 해보자

```
kn.predict([[30,600]]) #array([1]) 이면 도미임. 위에서 fish_target에 도미를 1이라 하기로 정했으니까
```

```
# 전달한 데이터를 kn이 모두 가지고 있다
# print(kn._fit_X)
# print(kn._y)

kn49 = KNeighborsClassifier(n_neighbors=49) # 참고할 데이터를 49개로 정함. 위 기본값은 5개 였음.
kn49.fit(fish_data, fish_target) # 훈련
kn49score = kn49.score(fish_data, fish_target)  # 평가 0.7142857142857143. 
print(kn49score)
print(35/49)  # 평가값이 같다. 49개 데이터 중 도미가 35개니까 도미만 잘 맞춘다.
```

```

# 문제 n_neighbors 매개변수 1~50를 줘서 점수가 1.0(정확도100%) 아래로 내려가기 시작하는 이웃의 갯수를 찾아보자.

knNum = KNeighborsClassifier()
knNum.fit(fish_data, fish_target)

for n in range(5, 50):
  # k-최근접 이웃 갯수 설정
  knNum.n_neighbors = n
  # 점수 계산
  score = knNum.score(fish_data, fish_target)
  # 100% 정확도 아닌 이웃 정보 출력
  if score < 1.0 :
    print(n, score)
    break
```

- 결과
```
18 0.9795918367346939
```
- 18개 이웃 근접 설정하면 정확도가 내려간다.