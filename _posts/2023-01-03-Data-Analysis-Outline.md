---
layout: post
title:  "[Data Analysis] 데이터분석 - 개요"

categories : DataAnalysis
  
tags:
  - Data Analysis
  - AI
  - Python
---
## 데이터 분류

> 데이터들이 점점 많아지면서 데이터를 이해하고 분석하는 능력은 매우 중요하다    
학교에서 배운 데이터 분석 관련 내용들을 정리해보겠다   

### 데이터의 다양한 종류
- Structured or Unstructured
  - 구조화가 되어있는가
- i.i.d. data or non-i.i.d. data (independent and identically distributed)
  - 독립적이고 균일하게 분포되어 있는가
- Vectorial or non-Vectorial data 
  - 벡터 형태의 데이터인가
- Labeled or unLabeled data
  - 라벨화가 되어있는가
- 데이터의 종류
  - Images, text, languages, time series, graphs, and so on   

## Regression
### Correlation Analysis (상관분석)
- 두 연속형 변수 사이 상관관계가 존재하는지 파악, 상관관계의 정도 확인
- 상관계수 : 통계학적 관점에서 선형적 상관도를 확인하여 정도를 파악
  - 음의 상관 : 반비례 관계
  - 무상관 : 상관이 없음
  - 양의 상관 : 정비례 관계
- 상관 분석 과정
  - 산점도(Scatter) 두 변수 상관 파악
  - 상관계수 확인
  - 의사결정
  
### Regression Analysis (회귀분석)
- 상관분석과 다르게 두 연속형 변수 X와 Y를 인과관계로 설명할 수 있음 
- X : 독립변수 -> 다른 변수에 영향을 주는 원인
- Y : 상관변수 -> 다른 변수에 영향을 받는 결과
- 데이터 x에 대한 결과를 y를 통해 둘 사이의 함수 f(x)를 학습한다   
<img width="616" alt="image" src="https://user-images.githubusercontent.com/110437548/210358354-bc2c8228-cfd9-48e7-855f-7ed86c221af6.png">

## Classification
### Classification이란
- 말 그대로 '분류'이다
- 데이터 x에 대한 결과 y를 통해 둘 사이의 분류 함수 f(x)를 학습한다
- 예 : 인공지능의 시각화를 통한 강아지와 고양이를 분류
<img width="453" alt="image" src="https://user-images.githubusercontent.com/110437548/210358725-ab1af89c-2032-415c-907d-17b1491166c9.png">

## Clustering
### Clustering이란
- 데이터 사이의 숨겨진 구조를 밝혀 비슷한 데이터들을 군집화하는 것
### K-means clustering
- 주어진 데이터들을 K개의 군집을 그룹화하는 알고리즘
- 각 그룹간의 거리 차이를 비슷하도록 한다.
- 계산이 빠르고 안정적이지만 초기 값에 따라 결과가 달라질 수 있음   


## Machine Learning
### Training - Test 
   - 데이터가 많을수록 더 정확해진다   
   - 대규모 데이터에서 패턴과 상관관계를 찾는다   
   - 최적의 의사결정과 예측을 수행하기 위해 훈련   
- Training : 데이터를 학습시켜 모델을 추출한다
- Test : 훈련된 모델로 테스트 데이터를 연습에 적용하고 성능을 측정한다

### 머신러닝 종류
1. Supervised Learning
- 지도 학습
- 훈련할 데이터와 결과값 (라벨) 제공
- 예 : regression, classification     
2. UnSupervised Learning
- 비지도 학습
- 훈련할 데이터만 제공
- 예 : clustering   
3. Semi - Supervised Learning
- 준지도 학습
- 훈련할 데이터와 약간의 결과값 제공
- Labeled data와 UnLabeled data가 모두 사용되는 머신러닝 기법
- 데이터를 수집하는 '데이터 레이블링' 작업에 소요되는 자원과 비용 감소
- Proxy-Label Method : Labeld data로 UnLabeled data에 Label을 예측하여 달아준다.
4. Reinforcement Learning
- 강화 학습
- 일련의 행동으로부터의 보상 제공
5. Self-Supervised Learning
- UnLabeled dataset으로부터 good representation을 얻고자 하는 학습 방법
- 비지도 학습의 일종
- 모델 스스로 task를 정해서 지도학습 방식으로 모델을 학습
- Self-prediction
  - 개별 data sample에 대해, sample 내의 한 파트를 통해서 다른 파트를 예측하는 task 수행
- Contrastive Learning
  - batch 내의 data sample이 주어졌을 때, 그들 사이의 관계를 예측하는 task 수행   

## 모델 평가
### 모델 테스트
- 데이터 분석 모델이 완성되었다면 전체 데이터를 모델 생성을 위해 분할하여 사용
- 훈련용, 테스트용, (검증용)으로 분할   
 
### 평가 항목
- 정확도(accuracy) : (TP+TN)/(TP+FN+FP+TN)
- 정밀도(precision): TP/(TP+FP)
- 재현율(recall)  : TP/(TP+FN)

|예측\정답|참|거짓|
|---|---|---|
|참|TP|FP|
|거짓|FN|TN|

#### ROC (Receiver Operating Characteristic)
- y축에 TP, x축에 FP 수치를 배치해 두 수치의 균형을 살펴보는 머신러닝 평가를 위한 시각화 모델
- ROC 커브 아래 면적인 AUC(Area Under Curve)가 1에 가까워질수록, 모델이 Y를 예측하는 정확도가 높은 모델
 <img width="294" alt="image" src="https://user-images.githubusercontent.com/110437548/210363188-7415d8ce-2d11-4cdd-ac9e-f4f2938698c7.png">

### 모델의 fairness 평가
- 사람에게 미치는 피해 평가

## 참고

- "따라하며 배우는 파이썬과 데이터 과학" - 천인국﹒박동규﹒강영민 저, 생능출판
