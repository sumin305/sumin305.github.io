---
layout: post
title:  "[Machine Learning] Linear Regression Cost Function 최소화"

categories : Machine_Learning
  
tags:
  - Machine Learning
  - Deep Learning
  - AI
  - Tensorflow
  - Python
---



머신러닝과 딥러닝에 대해 공부해보려고 합니다   
모두를 위한 머신러닝/딥러닝 강의를 수강하고 배운 것들을 정리해보겠습니다   
[참고] (https://hunkim.github.io/ml/)   

## Cost Function 최소화
### Hypothesis and Cost
H(x) = Wx + b
- cost를 최소화하는 W와 b의 값을 우리가 가지고 있는 데이터를 통해 구해보자 !

### What cost(W) looks like?
- cost함수   

![image](https://user-images.githubusercontent.com/110437548/212616443-73eef6b9-c5f1-4796-b572-eaba363c3df5.png)   

|x|y|
|--|--|
|1|1|
|2|2|
|3|3|   

- W = 1일때, cost(W)=?
{(1 x 1 - 1)^2 + (1 x 2 - 2)^2 + (1 x 3 - 3)^2}/3 = 0 + 0 + 0 = 0
- W = 0일때 cost(W) = 4.67
- W = 2일때 cost(W) = 4.67   

<img width="263" alt="image" src="https://user-images.githubusercontent.com/110437548/212618591-6c2f2b63-8865-4a45-bef8-80a87ab26864.png"> 
- 목적 : Cost = 0인 지점을 찾는 법

### Gradient Descent Algorithm
- Gradient Descent Alogorithm 이란?   
  - 경사하강법
  - cost function을 최소화 하는데에 사용
  - cost function이 주어졌을 때 그것을 최소화하는 W,b를 찾음
  - 다항 cost function에도 적용 가능

- 어떻게 작동하는가?   
: 시작점에서 계속 주위의 경사를 살피며 가장 낮은 부분으로 이동한다 (더 이상 경사가 하강되지 않을 때까지)
  - 아무 값에서 시작
  - W와 b값을 조금씩 바꿔감 
  - 그 때의 경사들을 계산하여 더 낮은 경사로 이동
  - 이 과정을 반복
> 어떤 지점에서 시작하든 항상 최소 cost 값을 구할 수 있다

- Formal definition : 미분 개념 이용
![IMG_5F5D70E0F266-1](https://user-images.githubusercontent.com/110437548/212935372-dd9681b6-ed26-4a46-9894-0ab6b660fef6.jpeg)


## TensorFlow로 구현하기
H(x) = Wx (+b)생략하고 minimize 해보자
```python
import numpy as np
import tensorflow as tf
import matplotlib.pyplot as plt

x_train = [1, 2, 3, 4]
y_train = [0, -1, -2, -3]

tf.model = tf.keras.Sequential()
tf.model.add(tf.keras.layers.Dense(units=1, input_dim=1))

sgd = tf.keras.optimizers.SGD(lr=0.1)
tf.model.compile(loss='mse', optimizer=sgd)

tf.model.summary()

# fit() trains the model and returns history of train
history = tf.model.fit(x_train, y_train, epochs=100)

y_predict = tf.model.predict(np.array([5, 4]))
print(y_predict)

# Plot training & validation loss values
plt.plot(history.history['loss'])
plt.title('Model loss')
plt.ylabel('Loss')
plt.xlabel('Epoch')
plt.legend(['Train', 'Test'], loc='upper left')
plt.show()
```
