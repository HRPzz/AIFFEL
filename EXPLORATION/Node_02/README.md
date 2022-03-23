# 2. Iris의 세 가지 품종, 분류해볼 수 있겠어요?

**캐글의 iris 데이터셋을 이용해 기본적인 머신러닝 분류 태스크를 진행하고, 자주 사용되는 모델과 훈련기법을 알아본다.**

---

[➡ [E-02] Classifier_Examples (Digits, Wine, Breast_Cancer).ipynb with nbviewer](https://nbviewer.org/github/HRPzz/AIFFEL/blob/main/EXPLORATION/Node_02/%5BE-02%5D%20Classifier_Examples%20(Digits%2C%20Wine%2C%20Breast_Cancer).ipynb)

---

|-|목차|⏲ 360분|
|:---:|---|:---:|
|2-1| 들어가며 | 5분|
|2-2| Iris의 세 가지 품종, 분류해 볼까요? (1) 붓꽃 분류 문제 | 15분|
|2-3| Iris의 세 가지 품종, 분류해 볼까요? (2) 데이터 준비, 그리고 자세히 살펴보기는 기본! | 15분|
|2-4| 첫 번째 머신러닝 실습, 간단하고도 빠르게! (1) 머신러닝 모델을 학습시키기 위한 문제지와 정답지 준비 | 20분|
|2-5| 첫 번째 머신러닝 실습, 간단하고도 빠르게! (2) 첫 번째 머신러닝 모델 학습시키기 | 20분|
|2-6| 첫 번째 머신러닝 실습, 간단하고도 빠르게! (3) 첫 번째 머신러닝 모델 평가하기 | 10분|
|2-7| 첫 번째 머신러닝 실습, 간단하고도 빠르게! (4) 다른 모델도 해 보고 싶다면? 코드 한 줄만 바꾸면 돼! | 40분|
|2-8| 내 모델은 얼마나 똑똑한가? 다양하게 평가해 보기 (1) 정확도에는 함정이 있다 | 20분|
|2-9| 내 모델은 얼마나 똑똑한가? 다양하게 평가해 보기 (2) 정답과 오답에도 종류가 있다! | 20분|
|2-10| 데이터가 달라도 문제 없어요! | 15분|
|2-11| 프로젝트 (1) load_digits : 손글씨를 분류해 봅시다 | 60분|
|2-12| 프로젝트 (2) load_wine : 와인을 분류해 봅시다 | 60분|
|2-13| 프로젝트 (3) load_breast_cancer : 유방암 여부를 진단해 봅시다 | 60분|
|2-14| 프로젝트 제출|-|

---

## 학습 목표

- scikit-learn에 내장된 예제 데이터셋의 종류를 알고 활용할 수 있다.
- scikit-learn에 내장된 분류 모델들을 학습시키고 예측해 볼 수 있다.
- 모델의 성능을 평가하는 지표의 종류에 대해 이해하고, 활용 및 확인해 볼 수 있다.
- Decision Tree, XGBoost, RandomForest, 로지스틱 회귀 모델을 활용해서 간단하게 학습 및 예측해 볼 수 있다.
- 데이터셋을 사용해서 스스로 분류 기초 실습을 진행할 수 있다.

## 학습 전제

- scikit-learn을 활용해서 머신러닝을 시도해본 적이 없다.
- scikit-learn에 내장된 분류 모델을 활용해본 적이 없다.
- 지도학습의 분류 실습을 해 본 적이 없다.
- 머신러닝 모델을 학습시켜보고, 그 성능을 평가해본 적이 없다.

---

## 노드 내용

- **머신러닝**
  - 지도학습 (Supervised Learning): 정답이 있는 문제 학습
    - 분류(Classification): 입력받은 데이터를 특정 카테고리 중 하나로 분류
    - 회귀(Regression): 입력받은 데이터에 따라 특정 필드의 수치를 맞히는 문제
  - 비지도 학습 (Unsupervised Learning): 정답이 없는 문제 학습
- **사이킷런(scikit-learn)**
  - 머신러닝에서 가장 많이 쓰이는 라이브러리
  - 사이킷런 제공 데이터셋: Toy datasets, Real world datasets
    - 데이터셋을 다루기 전에 데이터 정보를 먼저 확인해야 함!
    - from sklearn.datasets import load_iris
    - iris = load_iris()
      - iris.keys()
      - iris.data.shape
      - iris_data[0]
- 정답지(target == label)와 문제지(feature)
  - from sklearn.model_selection import train_test_split
  - X_train, X_test, y_train, y_test = train_test_split()
  - 정답지(label == target, 변수 y): 머신러닝 모델이 출력해야 하는 정답
    - iris.target.shape
    - iris.target_names
    - iris.DESCR
  - 문제지(feature, 변수 X): 머신러닝 모델에 입력되는 데이터
    - iris.feature_names
- pandas: 표 형태(행, 열 있는 2차원) 데이터를 다룰 때 사용
- **머신러닝 모델**
  - Decision Tree
    - 결정경계(decision boundary)가 데이터 축에 수직이어서 특정 데이터에만 잘 작동할 가능성이 높다.
    - 단점 극복: Random Forest 사용
    - from sklearn.tree import DecisionTreeClassifier
    - decision_tree = DecisionTreeClassifier(random_state=32)
  - Random Forest
    - Decision Tree를 여러 개 합쳐서 Decision Tree의 단점을 극복한 모델 => 앙상블(Ensemble) 기법
      - 앙상블(Ensemble method): 의견을 통합하거나 여러가지 결과를 합치는 방식
    - 각각의 의사 결정 트리를 만드는데 있어 쓰이는 요소들 (흡연 여부, 나이, 등등)을 무작위적으로 선정
    - 의견 통합은 투표로 결정함
  - SVD(Support Vector Machine)
    - Support Vector와 Hyperplane(초평면)을 이용해서 분류를 수행하게 되는 대표적인 선형 분류 알고리즘 => 이진 분류 모델
      - 일대다(one-vs.-rest 또는 one-vs.-all) 방법을 사용해 다중 클래스 분류 알고리즘으로 사용
      - 클래스의 수만큼 이진 분류 모델이 만들어지고 예측할 때는 만들어진 모든 이진 분류기가 작동하여 가장 높은 점수를 내는 분류기의 클래스를 예측값으로 선택
    - 2차원 공간, 2개 클래스만 존재할 때 => Margin 이 넓을수록 새로운 데이터를 잘 구분함(Margin 최대화 -> robustness 최대화)
      - Decision Boundary(결정 경계): 두 개의 클래스를 구분해 주는 선
      - Support Vector: Decision Boundary에 가까이 있는 데이터
      - Margin: Decision Boundary와 Support Vector 사이의 거리
  - SGD (Stochastic Gradient Descent Classifier)
    - 배치 크기가 1인 경사하강법 알고리즘 => 반복당 하나의 예(배치 크기 1)만을 사용
      - 확률적 경사하강법: 데이터 세트에서 무작위로 균일하게 선택한 하나의 예를 의존하여 각 단계의 예측 경사를 계산
        - 확률적(Stochastic): 각 배치를 포함하는 하나의 예가 무작위로 선택된다는 것
      - 배치
        - 경사하강법에서 배치는 단일 반복에서 기울기를 계산하는 데 사용하는 예(data)의 총 개수
        - Gradient Descent 에서의 배치는 전체 데이터 셋라고 가정
        - 배치 클수록 단일 반복 계산 오래 걸림, 중복 가능성 높아짐
    - 단점
      - 반복 충분해야 효과 있음 => 노이즈 심함
      - 최저점을 찾지 못할 수 있음
    - 단점 극복: 미니 배치 확률적 경사하강법(미니 배치 SGD)
  - Logistic Regression
    - 가장 널리 알려진 선형 분류 알고리즘
    - 소프트맥스(softmas) 함수를 사용한 다중 클래스 분류 알고리즘 => 소프트맥스 회귀(Softmax Regression)
      - 소프트맥스 함수: 클래스가 N개일 때, N차원의 벡터가 각 클래스가 정답일 확률을 표현하도록 정규화를 해주는 함수
- 모델 학습: .fit()
  - decision_tree.fit(X_train, y_train)
- 모델 예측
  - y_pred = decision_tree.predict(X_test)
- 성능 평가
  - print(classification_report(y_test, y_pred))
  - **성능 평가 척도(지표)**
    - 정확도(Accuracy)
      - 전체 개수 중 맞은 것의 개수
      - 데이터가 균형할 경우 accuracy 사용
    - 오차 행렬(confusion matrix): 정답과 오답을 구분하여 표현하는 방법
      - 오차행렬 예측 결과: TN(True Negative), FP(False Positive), FN(False Negative), TP(True Positive)
      - 오차행렬 성능 지표: Precision, Negative Predictive Value, Sensitivity(=Recall), Specificity, Accuracy
      - 데이터 불균형(unbalanced)할 경우 f1 score 사용
        - F1 score = Recall(재현율)과 Precision(정밀도)의 조화평균

---

## 2-11 ~ 2-13. 프로젝트 (1~3) : 손글씨, 와인, 유방암 여부를 분류해 봅시다

- 2-11. 프로젝트 (1) load_digits : 손글씨를 분류해 봅시다
- 2-12. 프로젝트 (2) load_wine : 와인을 분류해 봅시다
- 2-13. 프로젝트 (3) load_breast_cancer : 유방암 여부를 진단해 봅시다

데이터에 어떤 정보가 담겨있는지, feature는 무엇이고 label은 무엇인지 확인

- 목차
  - (1) 필요한 모듈 import 하기
  - (2) 데이터 준비
    - load_digits(): 손글씨 이미지 데이터
      - 데이터 총 1797개
      - feature 64개
      - label 클래스 10개
    - load_wine(): 와인 데이터
      - 데이터 총 178개
      - feature 13개
      - label 클래스 3개
    - load_breast_cancer(): 유방암 데이터
      - 데이터 총 569개
      - feature 30개
      - label 클래스 2개
  - (3) 데이터 이해하기
    - Feature Data 지정하기
    - Label Data 지정하기
    - Target Names 출력해 보기
    - 데이터 Describe 해 보기
  - (4) train, test 데이터 분리
    - 모델 학습과 테스트용 문제지(X_train, X_test)와 정답지(y_train, y_test)를 준비
  - (5) 다양한 모델로 학습시켜보기
    - Decision Tree 사용해 보기
    - Random Forest 사용해 보기
    - SVM 사용해 보기
    - SGD Classifier 사용해 보기
    - Logistic Regression 사용해 보기
  - (6) 모델을 평가해 보기
    - sklearn.metrics 에서 제공하는 평가지표 중 적절한 것을 선택

---

>## **루브릭**
>
>|번호|평가문항|상세기준|평가결과|
>|:---:|---|---|:---:|
>|1|3가지 데이터셋의 구성이 합리적으로 진행되었는가?|feature와 label 선정을 위한 데이터 분석과정이 체계적으로 전개됨|⭐|
>|2|3가지 데이터셋에 대해 각각 5가지 모델을 성공적으로 적용하였는가?|모델학습 및 테스트가 정상적으로 수행되었음|⭐|
>|3|3가지 데이터셋에 대해 모델의 평가지표가 적절히 선택되었는가?|평가지표 선택 및 이유 설명이 타당함|⭐|
