# 16. 다음에 볼 영화 예측하기

**고객이 바로 지금 원하는 것이 무엇인지를 예측하여 추천하는 Session-based Recommendation 개념을 익히고 실제로 모델을 구축해 본다.**

---

[➡ [E-16] Movie_Session_Based_Recommendation.ipynb with nbviewer](https://nbviewer.org/github/HRPzz/AIFFEL/blob/main/EXPLORATION/Node_16/%5BE-16%5D%20Movie_Session_Based_Recommendation.ipynb)

---

|-|목차|⏲ 360분|
|:---:|---|:---:|
|16-1| 들어가며 | 15분|
|16-2| Data Preprocess | 45분|
|16-3| 논문소개(GRU4REC) | 20분|
|16-4| Data Pipeline | 40분|
|16-5| Modeling | 60분|
|16-6| 프로젝트 - Movielens 영화 SBR | 180분|
|16-7| 프로젝트 제출 |-|

---

## 16-6. 프로젝트 - Movielens 영화 SBR

- Step 1. 데이터의 전처리
  - 데이터: Movielens 1M Dataset
- Step 2. 미니 배치의 구성
- Step 3. 모델 구성
- Step 4. 모델 학습
- Step 5. 모델 테스트

---

>## **루브릭**
>
>|번호|평가문항|상세기준|평가결과|
>|:---:|---|---|:---:|
>|1|Movielens 데이터셋을 session based recommendation 관점으로 전처리하는 과정이 체계적으로 진행되었다.|데이터셋의 면밀한 분석을 토대로 세션단위 정의 과정(길이분석, 시간분석)을 합리적으로 수행한 과정이 기술되었다.|⭐|
>|2|RNN 기반의 예측 모델이 정상적으로 구성되어 안정적으로 훈련이 진행되었다.|적절한 epoch만큼의 학습이 진행되는 과정에서 train loss가 안정적으로 감소하고, validation 단계에서의 Recall, MRR이 개선되는 것이 확인된다.|⭐|
>|3|세션정의, 모델구조, 하이퍼파라미터 등을 변경해서 실험하여 Recall, MRR 등의 변화추이를 관찰하였다.|3가지 이상의 변화를 시도하고 그 실험결과를 체계적으로 분석하였다.|⭐|
