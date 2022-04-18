# 16. 행동 스티커 만들기

**simplebaseline 모델을 활용하여 실제로 구현을 진행해 보면서 Human Pose Estimation을 좀더 깊이있게 이해해 본다.**

---

[➡ [CV-16] StackedHourglass_Network_vs_Simplebaseline.ipynb with nbviewer](https://nbviewer.org/github/HRPzz/AIFFEL/blob/main/GOING_DEEPER_CV/Node_16/%5BCV-16%5D%20StackedHourglass_Network_vs_Simplebaseline.ipynb)

---

|-|목차|⏲ 400분|
|:---:|---|:---:|
|16-1| 데이터셋을 어디에서 구할까? | 10분|
|16-2| 데이터 전처리하기 | 30분|
|16-3| TFRecord 파일 만들기 | 30분|
|16-4| Ray | 30분|
|16-5| data label 로 만들기 | 30분|
|16-6| 모델을 학습해보자 | 30분|
|16-7| 학습 엔진 만들기 | 30분|
|16-8| 둠칫둠칫 댄스타임 | 30분|
|16-9| Project: 모델 바꿔보기 | 180분|
|16-10| 프로젝트 제출 |-|

---

>## **루브릭**
>
>|번호|평가문항|상세기준|평가결과|
>|:---:|---|---|:---:|
>|1|tfrecord를 활용한 데이터셋 구성과 전처리를 통해 프로젝트 베이스라인 구성을 확인하였다.|MPII 데이터셋을 기반으로 1epoch에 30분 이내에 학습가능한 베이스라인을 구축하였다.|-|
>|2|simplebaseline 모델을 정상적으로 구현하였다.|simplebaseline 모델을 구현하여 실습코드의 모델을 대체하여 정상적으로 학습이 진행되었다.|-|
>|3|Hourglass 모델과 simplebaseline 모델을 비교분석한 결과를 체계적으로 정리하였다.|두 모델의 pose estimation 테스트결과 이미지 및 학습진행상황 등을 체계적으로 비교분석하였다.|-|
