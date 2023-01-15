---
layout: post
title:  "[Machine Learning] Linear Regression"

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

## Linear Regression
### Regression 
- Training Data

    |x|y|
    |--|--|
    |1|1|
    |2|2|
    |3|3|
  
* * *   

### Linear Hypothesis 
우리의 데이터들과 맞아 떨어지는 어떠한 **선형적인 모델**
> H(x)= Wx + b     

- linear model 을 통한 가설을 세운다

- 어떤 방정식이 더 가설과 잘 맞을까 ?
가장 적합한 W와 b값을 구해야함! -> 더 좋은 모델    

* * *

### Cost Function
= Loss function   
- 우리가 세운 Linear Hypothesis와 실제 데이터가 얼마나 다른가?
> H(x)-y      -> 음수가 나올수도 있기 때문에 좋은 식은 아니다   
> (|H(x)-y|)^2   

> <img width="359" alt="image" src="https://user-images.githubusercontent.com/110437548/212549325-c6902af8-bc85-4082-9b34-d7e3c341e387.png">

m = 학습 데이터의 개수   
cost = 예측한 값과 실제 값의 차이의 제곱을 다 더한 후 학습 데이터의 개수로 나누어준다   

- Minimize Cost
> Linear Regression의 목표   
> -> Cost의 값이 가장 작게 하도록 W와 b를 설정한다!


## TensorFlow로 구현하기

### 1) Build graph using TF operations

H(x) = Wx + b 의 선형식에서 W와 b 구하기
tensorFlow를 통한 regression graph 구현하기

- Variable : tensorflow가 변경시키는 값 (trainable variable)
- 만드는 법 
    - shape에 랜덤한 값을 넣어준다   
 tf.random_normal([1]) : rank가 1이고 값이 1인 array
    - 이름 선언


```python
import tensorflow.compat.v1 as tf
tf.disable_v2_behavior()

# X and Y data가 주어짐
x_train = [1,2,3]
y_train = [1,2,3]


W = tf.Variable(tf.random.normal([1]), name="weight")
b = tf.Variable(tf.random.normal([1]), name="bias")
# Our hypothesis XW + b
hypothesis = x_train * W + b

# cost/Loss function
# tf.reduce_mean -> tensor들의 평균값을 반환해준다
cost = tf.reduce_mean(tf.square(hypothesis - y_train))

#Minimize -> GradientDescent 이용
optimizer = tf.train.GradientDescentOptimizer(learning_rate = 0.01)
train = optimizer.minimize(cost)
print(train)
```

### 2) Run/update graph and get results
regression을 실행하기 위한 session을 만들고 W,b와 같은 Variable을 그래프에 global Variable로 초기화 해줘야한다





```python
# Launch the graph in a session
sess = tf.Session()
# Initializes global variables in the graph
sess.run(tf.global_variables_initializer())

# Fit the line
for step in range(2001):
    sess.run(train)
    # 2001번 출력은 너무 많으니까 줄임
    if step % 20  == 0:
         print(step, sess.run(cost), sess.run(W),sess.run(b) )
```

- Placeholder
: x, y data를 직접 값을 주지 않고 Placeholder를 사용하여 필요할때 노드를 주고 필요할 때 마다 값을 던져준다 


```python
X = tf.placeholder(tf.float32, shape=[None])
Y = tf.placeholder(tf.float32, shape=[None])

for step in range(2001):
    cost_val, W_val, b_val, _ = sess.run([cost,W,b,train], 
        feed_dict = {X:[1,2,3,4,5], Y:[2.1,3.1,4.1,5.1,6.1]})
    if step % 20 ==0 :
        print(step, cost_val, W_val, b_val)
```
