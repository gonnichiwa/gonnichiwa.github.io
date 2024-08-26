---
title: python lib functions, list, arr handling
date: 2024-04-07 17:19:00 +0900
categories: [PYTHON]
tags: [python, numpy, matplotlib]  # TAG names should always be lowercase
authors: [gonnichiwa]
---

### arr, list filtering 배열, 리스트 필터링(filter)

```py
arr = [0,1,1,0]
print(arr[:3]) # [0, 1, 1]
print(len(arr)) # 4

# arr에서 0인 갯수 구하기
def isZero(x):
  return x == 0

result = list(filter(isZero, arr)) 
print(len(result)) # 2

results = list(filter(lambda x: x == 0, arr))
print(results) # [0, 0]
```

### numpy

#### arange()

```py
print(np.arange(1,3,0.2))
# [1.  1.2 1.4 1.6 1.8 2.  2.2 2.4 2.6 2.8]
```

#### colmun_stack([arr, arr])

```py
fish_length = np.array([1,2,3])
fish_weight = np.array([4,5,6])
tuple = np.column_stack([fish_length, fish_weight]) # tuple, immutable
print(tuple)
#array([[1, 4],
#       [2, 5],
#       [3, 6]])
```

#### concatenate() : 두개의 배열 하나로 합치기

```py
aa = np.concatenate(([1,2,3],[1,4,5,6]))
print('aa={}'.format(aa)) # aa=[1 2 3 1 4 5 6]
bb = np.concatenate((np.ones(4), np.zeros(2))) # bb=[1. 1. 1. 1. 0. 0.]
```
- concatenate : (사슬같이) 잇다


#### ones(), zeros()
```py
print(np.ones(5)) # [1. 1. 1. 1. 1.]
print(np.zeros(3)) # [0. 0. 0.]
```


#### shuffle()

```py
arr = np.array([[1,2],[3,4],[5,6]])
np.random.shuffle(arr)
print(arr)
# [[5 6]
#  [1 2]
#  [3 4]]
```


#### seed()

```py
np.random.seed(49) # 같은 결과 나오도록 난수 생성seed 고정
arr = np.array([[1,2],[3,4],[5,6]])
np.random.shuffle(arr)
print(arr)
# [[5 6]
#  [1 2]
#  [3 4]]
```


#### zip()

```py
fish_length = [25.4, 26.3, 26.5]
fish_weight = [242.0, 290.0, 340.0]
fish_data = [[l,w] for l,w in zip(fish_length, fish_weight)]
print(fish_data) # [[25.4, 242.0], [26.3, 290.0], [26.5, 340.0]]
```

#### print()

```py
bream_length = [25.4, 26.3, 26.5]
print('bream_length={}'.format(len(bream_length))) # bream_length=3
print('bream_length={},{}'.format(len(bream_length),'aaaa')) # bream_length=3,aaaa
```

<br/>

---

<br/>

### matplotlib

#### pyplot.scatter()

```py
import matplotlib.pyplot as plt
# 도미 길이
bream_length = [25.4, 26.3, 26.5, 29.0]
# 도미 무게
bream_weight = [242.0, 290.0, 340.0, 363.0]
# 빙어 길이
smelt_length = [9.8, 10.5, 10.6, 11.0]
# 빙어 무게
smelt_weight = [6.7, 7.5, 7.0, 9.7]

plt.scatter(bream_length, bream_weight) # bream x,y
plt.scatter(smelt_length, smelt_weight, marker='^') # 산점도의 점 모양
# marker 모양 모음 : https://matplotlib.org/stable/api/markers_api.html
plt.xlabel('length')
plt.ylabel('weight')
plt.show()

```
![](https://blog.kakaocdn.net/dn/JB5Cc/btsGqLIaJtq/8mdRbO252ZMVZ1bLvhgj8K/img.png)


