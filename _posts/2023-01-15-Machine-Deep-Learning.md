---
layout: post
title:  "[Machine Learning] 머신러닝의 개념과 용어"

categories : Machine Learning
  
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

## Machine Learning Basic

### Machine Learning

explicit programming -> 각 환경에서의 출력을 명시해놓음   
- explicit programming 의 한계
  - Spam filter : 너무 많은 규칙들
  - Automatic driving   
    
- Machine Learning   
 > explicit programming 처럼 일일히 프로그래밍하지 말고 어떤 자료나 현상에서 자동적으로 컴퓨터가 배우도록 해보자    
 > 컴퓨터가 데이터를 보고 학습하여 어떠한 능력을 가지게 하는 것   
 > 입력을 기반으로 데이터를 읽어서 가공하여 출력함   

* * *   

### Learning

#### Supervised Learning
- label들이 정해져 있는 데이터 (training set)를 가지고 학습을 하는 것   
- Machine Learning에서 가장 많이 사용되는 문제해결 방식
  - Image Labeling : 강아지와 고양이 사진들이 주어지고 그것을 구분하는 알고리즘   
    <img width="661" alt="image" src="https://user-images.githubusercontent.com/110437548/212524855-82aff950-02d3-4a50-b346-ea92e00fbeeb.png">
  - Email spam filter : spam인지 ham인지 구분되어 있는 데이터들을 가지고 Email spam을 걸러내는 알고리즘
  - Predicting exam score : 공부한 시간을 통한 성적 예측 알고리즘

- Training data set   

    |x|y|             
    |--|--|
    |3,6,9|3|  
    |2,5,7|2|
    |2,3,5|1|   
    
  - Machine Learning 에 test값 X[9,3,6]이 들어오면 Training data set을 통해 Y값 예측하여 출력  
  - AlphaGO Machine Learning -> 기존에 사람들이 바둑을 둔 다양한 경우의 수의 기보를 학습
  

- Supervised Learning 의 종류
  - Regression : 보낸 시간을 기반으로 시험의 점수를 예측
  - Binary Classification : 보낸 시간을 기반으로 Pass/non-Pass를 예측
  - Multi-Label Classification : 보낸 시간을 기반으로 Grade를 예측 
![8AF8BDD8-BADF-4F42-ACC7-0F5B25E2032E](https://user-images.githubusercontent.com/110437548/212526229-5bb4e0d3-e3ab-4333-ae0e-869b333b8d7e.jpeg)


#### Unsupervised Learning
- Un-Labeled data를 기반으로 한 학습
- 레이블을 미리 정해주지 않고 주어진 데이터를 보고 스스로 학습
- Exapmle
  - Google news grouping : 구글에서 유사한 주제의 뉴스들을 그룹화
  - Word clustering : 비슷한 단어들을 모으는 머신러닝
 
