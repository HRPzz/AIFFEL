# 11. 어제 오른 내 주식, 과연 내일은?

**ARIMA 시계열 분석법을 배우고, 직접 주식 시세를 예측해 본다.**

- 시계열 예측(Time-Series Prediction)을 다루는 가장 널리 알려진 통계적 기법: ARIMA(Auto-regressive Integrated Moving Average)
  - => 특정 주식 종목의 가격을 예측해 보는 실습을 진행
- 시계열 예측에 사용되는 모델
  - 통계학 이론적 기반 ARIMA
  - 페이스북에서 발표한 Prophet
  - LSTM 등 딥러닝을 활용하는 방법

---

[➡ [E-11] forecast_stock.ipynb with nbviewer](https://nbviewer.org/github/HRPzz/AIFFEL/blob/main/EXPLORATION/Node_11/%5BE-11%5D%20forecast_stock.ipynb)

---

|-|목차|⏲ 390분|
|:---:|---|:---:|
|11-1| 들어가며 | 5분|
|11-2| 시계열 예측이란(1) 미래를 예측한다는 것은 가능할까? | 20분|
|11-3| 시계열 예측이란(2) Stationary한 시계열 데이터 | 40분|
|11-4| 시계열 예측이란(3) 시계열 데이터 사례분석 | 40분|
|11-5| 시계열 예측이란(4) Stationary 여부를 체크하는 통계적 방법 | 25분|
|11-6| 시계열 예측의 기본 아이디어 : Stationary하게 만들 방법은 없을까? | 40분|
|11-7| ARIMA 모델의 개념 | 40분|
|11-8| ARIMA 모델 훈련과 추론 | 30분|
|11-9| 프로젝트 : 주식 예측에 도전해 보자 | 150분|
|11-10| 프로젝트 제출|-|

---

## 학습 목표

- 시계열 데이터의 특성과 안정적(Stationary) 시계열의 개념을 이해한다.
- ARIMA 모델을 구성하는 AR, MA, Diffencing의 개념을 이해하고 간단한 시계열 데이터에 적용해 본다.
- 실제 주식 데이터에 ARIMA를 적용해서 예측 정확도를 확인해 본다.

## 목차

### 시계열 예측이란

- 미래를 예측한다는 것은 가능할까?
- Stationary한 시계열 데이터란?
- 시계열 데이터 사례 분석
- Stationary 여부를 체크하는 통계적 방법

### ARIMA 시계열 예측

- 시계열 예측의 기본 아이디어 : Stationary하게 만들 방법은 없을까?
- ARIMA 모델의 개념
- ARIMA 모델 훈련과 추론

---

## 11-9. 프로젝트 : 주식 예측에 도전해 보자

- ARIMA를 통해 시계열 데이터를 예측하는 과정을 진행해 보았습니다. 이제 실제 주식값 예측에 도전해 봅시다. 데이터는 과거의 일자별 시세(Historical Prices)입니다.
- 데이터셋은 [Yahoo Finance](https://finance.yahoo.com/) 에서 다운로드할 수 있습니다.
![](https://d3s0tskafalll9.cloudfront.net/media/images/E-16-5.max-800x600.png)

- STEP 1 : 시계열 데이터 준비
- STEP 2 : 각종 전처리 수행
- STEP 3 : 시계열 안정성 분석
- STEP 4 : 학습, 테스트 데이터셋 생성
- STEP 5 : 적정 ARIMA 모수 찾기
- STEP 6 : ARIMA 모델 훈련과 테스트
- STEP 7 : 다른 주식 종목 예측해 보기

---

>## **루브릭**
>
>|번호|평가문항|상세기준|평가결과|
>|:---:|---|---|:---:|
>|1|시계열의 안정성이 충분히 확인되었는가?|플로팅과 adfuller 메소드가 모두 적절히 사용되었음|⭐|
>|2|ARIMA 모델 모수선택 근거를 체계적으로 제시하였는가?|p,q를 위한 ACF, PACF 사용과 d를 위한 차분 과정이 명확히 제시됨|⭐|
>|3|예측 모델의 오차율이 기준 이하로 정확하게 나왔는가?|3개 이상 종목이 MAPE 15% 미만의 정확도로 예측됨|⭐|
