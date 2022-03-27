# 10. 다양한 데이터 전처리 기법

**EDA를 통해 도출된 데이터 인사이트를 토대로, 효과적인 Feature Engineering을 위해 사용하는 Encoding, Scaling, Feature Selection 등의 전처리 기법을 실습해 본다.**

---

|-|목차|⏲ 180분|
|:---:|---|:---:|
|10-1| 들어가며 | 5분|
|10-2| 결측치(Missing Data) | 15분|
|10-3| 중복된 데이터 | 15분|
|10-4| 이상치(Outlier) | 30분|
|10-5| 정규화(Normalization) | 20분|
|10-6| 원-핫 인코딩(One-Hot Encoding) | 15분|
|10-7| 구간화(Binning) | 80분|

---

## 학습 목표

- 중복된 데이터를 찾아 제거할 수 있고, 결측치(missing data)를 제거하거나 채워 넣을 수 있다.
- 데이터를 정규화시킬 수 있다.
- 이상치(outlier)를 찾고, 이를 처리할 수 있다.
- 범주형 데이터를 원-핫 인코딩할 수 있다.
- 연속적인 데이터를 구간으로 나눠 범주형 데이터로 변환할 수 있다.

## 학습 목차

- 결측치(Missing Data)
- 중복된 데이터
- 이상치(Outlier)
- 정규화(Normalization)
- 원-핫 인코딩(One-Hot Encoding)
- 구간화(Binning)

---

## 노드 내용

- “데이터 분석의 8할은 데이터 전처리이다.”
  - 전처리에 따라서 데이터 분석(분석 결과의 신뢰도, 예측 모델의 정확도)의 질이 달라짐
- 사용할 데이터
  - trade.csv
  - import pandas as pd
  - trade = pd.read_csv(csv_file_path)
  - [관세청 수출입 무역 통계](https://unipass.customs.go.kr/ets/index.do)
- 결측치(Missing Data) 처리 방법
  - 결측치가 있는 데이터 제거
  - 어떤 값으로 대체
    - 특정 값(평균, 중앙값, 최빈값 등) 지정 => 결측치가 많은 경우 데이터의 분산이 실제보다 작아지는 문제 발생
    - 다른 데이터를 이용해 예측값으로 대체 (e.g. 머신러닝 모델로 2020년 4월 미국의 예측값을 만들고, 이 값으로 결측치 보완)
    - 시계열 특성을 가진 데이터의 경우 앞뒤 데이터를 통해 결측치 대체 (e.g. 기온을 측정하는 센서 데이터에서 결측치가 발생할 경우, 전후 데이터의 평균으로 보완)
- 중복된 데이터
  - df.duplicated(): 중복된 데이터 여부 불리언 값으로 반환
  - [df.drop_duplicates()](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.DataFrame.drop_duplicates.html): 중복된 데이터 삭제
- 이상치(Outlier)
  - 극단적으로 크거나 작은 값
  - 이상치 찾기(anomaly detection)
    - z score
      - z score($\frac {X−μ}{σ}$)
      - z score가 특정 기준을 넘어서는 데이터에 대해 이상치라고 판단
      - 기준을 작게 하면 이상치라고 판단하는 데이터가 많아지고, 기준을 크게 하면 이상치라고 판단하는 데이터가 적어짐
      - z score 코딩
        - abs(df[col] - np.mean(df[col])) : |데이터-평균|
        - abs(df[col] - np.mean(df[col]))/np.std(df[col]) : |데이터-평균|/표준편차
        - df[abs(df[col] - np.mean(df[col]))/np.std(df[col])>z].index: 값이 z보다 큰 데이터의 인덱스 추출
      - 단점
        - Robust하지 못하다 - 왜나하면 평균과 표준편차 자체가 이상치의 존재에 크게 영향을 받기 때문이다.
        - 작은 데이터셋의 경우 z-score의 방법으로 이상치를 알아내기 어렵다. 특히 item이 12개 이하인 데이터셋에서는 불가능하다.
    - IQR(Interquartile range)
      - 사분위 범위수
      - IQR = Q3 - Q1  # IQR = 제 3분위수 - 제 1분위수
      - 이상치 판단: data < Q1-1.5*IQR or Q3+1.5*IQR > data
      - ![IQR](https://d3s0tskafalll9.cloudfront.net/media/images/F-19-1.max-800x600.jpg)
      - IQR 코딩
        - Q3, Q1 = np.percentile(data, [75 ,25])
        - IQR = Q3 - Q1
        - data[(Q1-1.5*IQR > data)|(Q3+1.5*IQR < data)]
  - 이상치 처리
    - 이상치 삭제 -> 이상치를 원래 데이터에서 삭제하고, 이상치끼리 따로 분석
    - 다른 값(최댓값, 최솟값 등)으로 대체 (데이터가 적으면 이상치를 삭제하기보다 다른 값으로 대체하는 것이 나을 수 있음)
    - 다른 데이터를 활용하여 예측 모델을 만들어 예측값 활용
    - binning을 통해 수치형 데이터를 범주형으로 변환
- 정규화(Normalization)
  - 컬럼 간에 범위가 크게 다를 경우 전처리 과정에서 데이터를 정규화해야 함
  - 정규화 방법
    - 표준화(Standardization)
      - 데이터의 평균은 0, 분산은 1로 변환
      - $\frac{X−μ}{σ}$
      - x_standardization = (x - x.mean())/x.std()
      - scikit-learn의 StandardScaler()를 사용할 수도 있음
    - Min-Max Scaling
      - 데이터의 최솟값은 0, 최댓값은 1로 변환
      - $\frac{X−X_{min}}{X_{max}-X_{min}}$
      - x_min_max = (x-x.min())/(x.max()-x.min())
      - scikit-learn의 MinMaxScaler()를 사용할 수도 있음
  - train 데이터와 test 데이터가 나눠져 있는 경우 train 데이터를 정규화시켰던 기준 그대로 test 데이터도 정규화 시켜줘야 한다!!
  - cf. 로그 변환 등의 기법을 정규화와 함께 사용하면 도움이 된다.
- 원-핫 인코딩(One-Hot Encoding)
  - 카테고리별 이진 특성을 만들어 해당하는 특성만 1, 나머지는 0으로 만드는 방법
  - pd.get_dummies()
- 구간화(Data Binning, bucketing)
  - 연속적인 데이터를 구간을 나눠 분석할 때 사용하는 방법
  - e.g. 히스토그램
  - 수치형 -> 범주형 데이터로 변형
    - pd.cut(데이터, bins=리스트/정수): 데이터를 구간별로 나눔
    - pd.qcut(데이터, q=정수): 데이터의 분포를 비슷한 크기의 그룹으로 나눔
  - .value_counts().sort_index()  # 구간별로 값이 몇 개가 속해 있는지 확인
- 관심있는 분야 데이터 찾기
  - [공공데이터포털](https://www.data.go.kr/)
  - [캐글](https://www.kaggle.com/)
