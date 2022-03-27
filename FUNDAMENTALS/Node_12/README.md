# 12. 사이킷런으로 구현해 보는 머신러닝

**머신러닝의 다양한 알고리즘에 대해 알아보고 사이킷런 라이브러리 사용법을 익힙니다. 사이킷런에서 제공하는 모듈을 이해하고, 머신러닝에 적용해 봅니다.**

---

|-|목차|⏲ 180분|
|:---:|---|:---:|
|12-1| 들어가며 | 5분|
|12-2| 머신러닝 알고리즘 | 20분|
|12-3| 사이킷런에서 가이드하는 머신러닝 알고리즘 | 15분|
|12-4| Hello Scikit-learn | 40분|
|12-5| 사이킷런의 주요 모듈 (1) 데이터 표현법 | 10분|
|12-6| 사이킷런의 주요 모듈 (2) 회귀 모델 실습 | 20분|
|12-7| 사이킷런의 주요 모듈 (3) datasets 모듈 | 20분|
|12-8| 사이킷런의 주요 모듈 (4) 사이킷런 데이터셋을 이용한 분류 문제 실습 | 15분|
|12-9| 사이킷런의 주요 모듈 (5) Estimator | 10분|
|12-10| 훈련 데이터와 테스트 데이터 분리하기 | 20분|
|12-11| 마무리 | 5분|

---

## 학습목표

- 머신러닝의 다양한 알고리즘을 소개합니다.
- 사이킷런 라이브러리의 사용법을 익힙니다.
- 사이킷런에서 데이터를 표현하는 방법에 대해 이해하고 훈련용 데이터셋과 테스트용 데이터셋으로 데이터를 나누는 방법을 이해합니다.

## 목차

1. 다양한 머신러닝 알고리즘
2. 사이킷런에서 가이드하는 머신러닝 알고리즘
3. Hello Scikit-learn
4. 사이킷런의 주요 모듈
    - 4.1. 데이터 표현법
    - 4.2. 회귀 모델 실습
    - 4.3. datasets 모듈
    - 4.4. 사이킷런 데이터셋을 이용한 분류 문제 실습
    - 4.5. Estimator
5. 훈련 데이터와 테스트 데이터 분리하기

---

## 노드 내용

- 머신러닝의 알고리즘 종류 => 정답 유무, 데이터의 종류, 특성, 문제 정의에 따라 복합적으로 사용
  - 지도학습 (Supervised Learning)
    - 분류(Classification), 회귀(Regression), 예측(Forecasting)
  - 준(반)지도학습(Semi-Supervised Learning) or 약지도학습(Weakly Supervised Learning)
  - 비지도학습 (Unsupervised Learning)
    - 클러스터링(Clustering), 차원축소(Dimensionality Reduction)
  - 강화학습 (Reinforcement Learning)
    - e.g. 알파고: 지도학습으로 바둑 기보 학습 + 강화학습으로 최적화
- 머신러닝 라이브러리: 사이킷런(Scikit-Learn)
  - 사이킷런에서 알고리즘의 Task 4가지: Classification, Regression, Clustering, Dimensionality Reduction
  - 사이킷런에서 알고리즘 나눈 기준: 데이터 수량, 라벨(정답) 유무, 데이터 종류(수치형(quantity), 범주형(category))
  - import sklearn
  - Estimator, train_test_split(), fit(), predict()
  - Scikit-learn API<br>![Scikit-learn API](https://d3s0tskafalll9.cloudfront.net/media/images/Untitled_8_gdSwRxK.max-800x600.png)
  - sklearn.datasets 모듈의 Toy dataset의 예시
    - datasets.load_boston(): 회귀 문제, 미국 보스턴 집값 예측(version 1.2 이후 삭제 예정)
    - datasets.load_breast_cancer(): 분류 문제, 유방암 판별
    - datasets.load_digits(): 분류 문제, 0 ~ 9 숫자 분류
    - datasets.load_iris(): 분류 문제, iris 품종 분류
    - datasets.load_wine(): 분류 문제, 와인 분류
- 데이터셋 표현
  - NumPy의 ndarray
  - Pandas의 DataFrame
  - SciPy의 Sparse Matrix
    - 특성 행렬(Feature Matrix) == X
      - 입력 데이터
      - 특성(feature): 데이터에서 수치/이산/불리언 값으로 표현되는 개별 관측치를 의미 => 특성 행렬에서는 열에 해당하는 값
      - 표본(sample): 각 입력 데이터, 특성 행렬에서는 행에 해당하는 값
      - n_samples: 행의 개수(표본의 개수)
      - n_features: 열의 개수(특성의 개수)
      - **X** : 통상 특성 행렬은 변수명 X로 표기
      - [n_samples, n_features]은 [행, 열] 형태의 2차원 배열 구조 사용 => NumPy의 ndarray, Pandas의 DataFrame, SciPy의 Sparse Matrix를 사용하여 나타낼 수 있음
    - 타겟 벡터(Target Vector) == y
      - 입력 데이터의 라벨(정답)
      - 목표(Target): 라벨/타겟값/목표값이라 부름 => 특성 행렬(Feature Matrix)로부터 예측하고자 하는 것
      - n_samples: 벡터의 길이(라벨의 개수)
      - 타겟 벡터에서 n_features는 없음
      - **y** : 통상 타겟 벡터는 변수명 y로 표기
      - 타겟 벡터는 보통 1차원 벡터로 나타냄 => NumPy의 ndarray, Pandas의 Series를 사용하여 나타낼 수 있음
      - (단, 타겟 벡터는 경우에 따라 1차원으로 나타내지 않을 수도 있습니다. 이 노드에서 사용되는 예제는 모두 1차원 벡터입니다.)
    - **❗️특성 행렬 X의 n_samples와 타겟 벡터 y의 n_samples는 동일해야 함**
    - 훈련 데이터와 테스트 데이터는 달라야 함
- 회귀 모델 성능 평가: RMSE(Root Mean Square Error)

```python
from sklearn.metrics import mean_squared_error

error = np.sqrt(mean_squared_error(y,y_new))
```

- 분류 문제 성능 평가: classification_report(y, y_pred), accuracy_score(y, y_pred)

```python
from sklearn.metrics import accuracy_score
from sklearn.metrics import classification_report

# 인자: 타겟 벡터(라벨) y 와 예측값 y_pred
print(classification_report(y, y_pred))
# 정확도 출력
print("accuracy = ", accuracy_score(y, y_pred))
```

- 비지도 학습 과정

![Unsupervised Learning](https://d3s0tskafalll9.cloudfront.net/media/images/Untitled_4_QrGQlgR.max-800x600.png)

- 실습

![Training Data != Test Data](https://d3s0tskafalll9.cloudfront.net/media/images/Untitled_6_uWp8wos.max-800x600.png)

```python
# 데이터셋 로드하기
data = load_wine()
# 훈련용 데이터셋 나누기
X_train, X_test, y_train, y_test = train_test_split(data.data, data.target, test_size=0.2)
# 훈련하기
model = RandomForestClassifier()
model.fit(X_train, y_train)
# 예측하기
y_pred = model.predict(X_test)
# 정답률 출력하기
print("정답률=", accuracy_score(y_test, y_pred))
```
