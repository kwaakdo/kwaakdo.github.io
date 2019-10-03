---
title: Deep Learning Tutorial With Keras
tags: DeepLearning AI Python
---
![image](https://s3.amazonaws.com/keras.io/img/keras-logo-2018-large-1200.png)
[Keras](https://keras.io/)는 딥 러닝 모델을 개발할 수 있는 쉽고 강력한 파이썬 오픈소스 라이브러리다. 유명한 딥 러닝 라이브러리 [Theano](https://machinelearningmastery.com/introduction-python-deep-learning-library-theano/)와 [Tensorflow](https://machinelearningmastery.com/introduction-python-deep-learning-library-tensorflow/)를 백엔드로써 활용하면 몇 줄의 코드만으로도 뉴럴 네트워크 모델을 만들고 학습시킬 수 있다.

이번 강좌를 통해 Keras를 활용하여 딥러닝 뉴럴 네트워크를 만드는 방법에 대해 알아볼것이다.

<!--more-->
# Keras
**요구사항**

1. Python 2 혹은 3이 설치되어 있어야 한다. [설치하기](https://www.python.org/downloads/){:.button.button--primary.button--rounded.button--xs}
2. SciPy(NumPy 포함)이 설치되어 있어야 한다. [설치하기](https://www.scipy.org/install.html){:.button.button--primary.button--rounded.button--xs}
3. Keras와 backend(Theano 혹은 Tensorflow)가 설치되어 있어야 한다. [설치하기](https://keras.io/#installation){:.button.button--primary.button--rounded.button--xs}

---
## 1) 데이터 불러오기
첫번째로, 라이브러리를 불러와야한다. [NumPy](https://www.numpy.org/)의 [loadtxt](https://docs.scipy.org/doc/numpy/reference/generated/numpy.loadtxt.html)를 사용하여 데이터를 불러오고 [Keras](https://keras.io/)의 [Sequential](https://keras.io/models/sequential/) 과 [Dense](https://keras.io/layers/core/)를 사용할 것이다. [Sequential](https://keras.io/models/sequential/)은 순차적으로 레이어 층을 더해주는 순차모델이다. [Dense](https://keras.io/layers/core/)는 뉴럴 네트워크를 구성하는 레이어를 생성할 때 필요하다.

```python
from numpy import loadtxt
from keras.models import Sequential
from keras.layers import Dense
```
`imports`를 마쳤다면 이제 데이터를 불러올 수 있다.

이 강좌에서는 피마 인디언 데이터셋을 사용할 것이다. [UCI Machine Learning Repository](https://archive.ics.uci.edu/ml/index.php)에서 제공한다. 피마 인디언 데이터셋은 [Binary Classification Problem](https://en.wikipedia.org/wiki/Binary_classification)으로써, 당뇨병의 음성, 양성이 1과 0으로 표현되어 있으며 5년간의 당뇨병 의료 기록이 저장되어있다.

데이터셋
: [CSV FILE](https://raw.githubusercontent.com/jbrownlee/Datasets/master/pima-indians-diabetes.data.csv)

데이터셋 정보
: [DETAILS](https://raw.githubusercontent.com/jbrownlee/Datasets/master/pima-indians-diabetes.names)

데이터셋을 `pima-indians-diabetes.csv`로 저장한다. 데이터셋을 살펴보면 아래와 같은 숫자들이 보인다.
```
6,148,72,35,0,33.6,0.627,50,1
1,85,66,29,0,26.6,0.351,31,0
8,183,64,0,0,23.3,0.672,32,1
1,89,66,23,94,28.1,0.167,21,0
0,137,40,35,168,43.1,2.288,33,1
...
```

위 숫자들이 의미하는 것은 8개의 input과 1개의 output이다. 한 row당 좌측 8개는 Feature이고 우측 1개는 당뇨병의 양성, 음성을 나타낸다. 아래는 데이터셋의 세부적인 내용이다.

**Input Variables(X) :**
1. 임신 횟수
2. 구강내 2시간 동안의 글루코오즈 잔류량 테스트
3. 혈압 (mm Hg)
4. 삼두박근 쪽의 피부 두께 (mm)
5. 2시간 동안의 세륨 인슐린의 양 (mu U/ml)
6. BMI 지수 (weight in kg/(height in m)^2)
7. 가계 당뇨병 병력 함수
8. 나이 (years)

**Output Variables(y) :**
1. 양성, 음성 (0 or 1)

NumPy는 [Slicing](https://docs.scipy.org/doc/numpy-1.13.0/reference/arrays.indexing.html)을 지원한다. loadtxt로 데이터셋을 불러오고나서, 같은 Row에 있는 Input과 Output을 분리하기 위해 [Slicing](https://docs.scipy.org/doc/numpy-1.13.0/reference/arrays.indexing.html)처리를 해준다. 
```python
dataset = loadtxt('pima-indians-diabetes.csv', delimiter=',')

X = dataset[:,0:8]
y = dataset[:,8]
```
[Slicing](https://docs.scipy.org/doc/numpy-1.13.0/reference/arrays.indexing.html)을 하게 되면 좌측 8개의 Input Variables와 우측 1개의 Output Variables가 각각 X, y에 분리되어 저장된 것을 알 수 있다.

*input:*
```python
X
```
*output:*
```
array([[  6.   , 148.   ,  72.   , ...,  33.6  ,   0.627,  50.   ],
       [  1.   ,  85.   ,  66.   , ...,  26.6  ,   0.351,  31.   ],
       [  8.   , 183.   ,  64.   , ...,  23.3  ,   0.672,  32.   ],
       ...,
       [  5.   , 121.   ,  72.   , ...,  26.2  ,   0.245,  30.   ],
       [  1.   , 126.   ,  60.   , ...,  30.1  ,   0.349,  47.   ],
       [  1.   ,  93.   ,  70.   , ...,  30.4  ,   0.315,  23.   ]])
```

## 2) 케라스 모델 정의하기
Dense 레이어를 사용하여 다층 퍼셉트론 모델을 만들 수 있다.
- 속성이 8개이기 때문에, 입력 뉴런도 8개이다.
- 첫번째 hidden layer는 12개의 노드를 가지고 있으며 [ReLu Activation Function](https://machinelearningmastery.com/rectified-linear-activation-function-for-deep-learning-neural-networks/)를 사용한다.
- 두번째 hidden layer는 8개의 노드를 가지고 있으며 [ReLu Activation Function](https://machinelearningmastery.com/rectified-linear-activation-function-for-deep-learning-neural-networks/)를 사용한다.
- 세번째 output layer는 한개의 노드이며 [Sigmoid Activation Function](https://towardsdatascience.com/activation-functions-neural-networks-1cbd9f8d91d6)을 사용한다.
```python
# define the keras model
model = Sequential()
model.add(Dense(12, input_dim=8, activation='relu'))
model.add(Dense(8, activation='relu'))
model.add(Dense(1, activation='sigmoid'))
```
> **Note.** 헷갈리기 쉬운 것은, 첫번째 Dense 레이어의 역할이다. 첫번째 Dense 레이어의 역할은 input 레이어와 첫번째 hidden layer 이 두 가지의 역할을 한다.

## 3) 케라스 모델 학습과정 설정하기
케라스를 정의하였으면 손실함수(Loss Functions)와 최적화(Optimizer) 알고리즘을 설정해야 한다.
- 손실함수로는  [Binary Classification Problem](https://en.wikipedia.org/wiki/Binary_classification)이기 때문에,[Binary_Cross Entropy](https://towardsdatascience.com/understanding-binary-cross-entropy-log-loss-a-visual-explanation-a3ac6025181a)를 사용한다. 
- 최적화 알고리즘으로는 [Adam](https://towardsdatascience.com/adam-latest-trends-in-deep-learning-optimization-6be9a291375c)을 사용한다. [Adam](https://towardsdatascience.com/adam-latest-trends-in-deep-learning-optimization-6be9a291375c)은 학습률을 줄여나가고 속도를 계산하여 학습의 갱신강도를 적응적으로 조정해나가는 방법이다.
- metric는 평가척도를 나타낸다. 일반적으로 `accuracy`를 사용한다.
```python
...
# compile the keras model
model.compile(loss='binary_crossentropy', optimizer='adam', metrics=['accuracy'])
...
```

## 4) 케라스 모델 학습시키기
**fit()** 함수를 호출함으로써 모델을 학습시킬 수 있다. 첫번째 인자는 input이고, 두번째 인자는 output이다.
Epoch
: 학습 반복 횟수이다. 여기서는 150회로 지정해보겠다.
Batch
: 가중치를 업데이트할 배치 크기이다. 10으로 지정해보겠다.
```python
...
# fit the keras model on the dataset
model.fit(X, y, epochs=150, batch_size=10)
...
```


> 참고 : [Your First Deep Learning Project in Python with Keras Step-By-Step](https://machinelearningmastery.com/tutorial-first-neural-network-python-keras/)