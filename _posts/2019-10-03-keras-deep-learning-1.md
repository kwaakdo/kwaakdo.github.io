---
title: Deep Learning Tutorial With Keras
tags: DeepLearning AI Python
---
![image](https://s3.amazonaws.com/keras.io/img/keras-logo-2018-large-1200.png)
[Keras](https://keras.io/)는 딥 러닝 모델을 개발할 수 있는 쉽고 강력한 파이썬 오픈소스 라이브러리다. 효율적인 라이브러리인 [Theano](https://machinelearningmastery.com/introduction-python-deep-learning-library-theano/)와 [Tensorflow](https://machinelearningmastery.com/introduction-python-deep-learning-library-tensorflow/)를 활용하면 몇 줄의 코드만으로도 뉴럴 네트워크 모델을 만들고 학습시킬 수 있다.

이번 강좌를 통해 Keras를 활용하여 딥러닝 뉴럴 네트워크를 만드는 방법에 대해 알아볼것이다.

<!--more-->
# Keras
## 요구사항
1. Python 2 혹은 3이 설치되어 있어야 한다. [설치하기](https://www.python.org/downloads/)[BUTTON](#){:.button.button--primary.button--pill}
2. SciPy(NumPy 포함)이 설치되어 있어야 한다. [설치하기](https://www.scipy.org/install.html)
3. Keras와 backend(Theano 혹은 Tensorflow)가 설치되어 있어야 한다. [설치하기](https://keras.io/#installation)

---
## 1) 데이터 불러오기
첫번째로, 라이브러리를 불러와야한다. [NumPy](https://www.numpy.org/)의 [loadtxt](https://docs.scipy.org/doc/numpy/reference/generated/numpy.loadtxt.html)를 사용하여 데이터를 불러오고 [Keras](https://keras.io/)의 [Sequential](https://keras.io/models/sequential/) 과 [Dense](https://keras.io/layers/core/)를 사용할 것이다. [Sequential](https://keras.io/models/sequential/)은 순차적으로 레이어 층을 더해주는 순차모델이다. [Dense](https://keras.io/layers/core/)는 뉴럴 네트워크를 구성하는 레이어를 생성할 때 필요하다.

```python
from numpy import loadtxt
from keras.models import Sequential
from keras.layers import Dense
```
`imports`를 마쳤다면 이제 데이터를 불러올 수 있다.


> 참고 : [Your First Deep Learning Project in Python with Keras Step-By-Step](https://machinelearningmastery.com/tutorial-first-neural-network-python-keras/)