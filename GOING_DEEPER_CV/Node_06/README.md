# 6. 나를 찾아줘 - Class Activation Map 만들기

**CAM, Grad-CAM을 위한 모델을 직접 만들고, CAM을 추출해 시각화 해본다. CAM을 Object detection에 적용해 결과를 평가해 본다.**

---

|-|목차|⏲ 420분|
|:---:|---|:---:|
|6-1| 들어가며 | 5분|
|6-2| CAM, Grad-CAM용 모델 준비하기 (1) 데이터셋 준비하기 | 60분|
|6-3| CAM, Grad-CAM용 모델 준비하기 (2) 물체의 위치정보 | 15분|
|6-4| CAM, Grad-CAM용 모델 준비하기 (3) CAM을 위한 모델 만들기 | 15분|
|6-5| CAM, Grad-CAM용 모델 준비하기 (4) CAM 모델 학습하기 | 60분|
|6-6| CAM | 30분|
|6-7| Grad-CAM | 25분|
|6-8| Detection with CAM | 30분|
|6-9| 프로젝트: CAM을 만들고 평가해 보자 | 180분|
|6-10| 프로젝트 제출 |-|

---

## 실습목표

- Classification model로부터 CAM을 얻어낼 수 있다.
- CAM으로 물체의 위치를 찾을 수 있다.
- CAM을 시각화 비교할 수 있다.

## 학습 내용

1. CAM, Grad-CAM용 모델 준비하기
2. CAM
3. Grad-CAM
4. Detection with CAM

---

## 6-9. 프로젝트: CAM을 만들고 평가해 보자

- 라이브러리 버전 확인하기
- CAM 구현하기
- Grad-CAM 구현하기
- 바운딩 박스 구하기
- IoU 구하기

---

>## **루브릭**
>
>|번호|평가문항|상세기준|평가결과|
>|:---:|---|---|:---:|
>|1|CAM을 얻기 위한 기본모델의 구성과 학습이 정상 진행되었는가?|ResNet50 + GAP + DenseLayer 결합된 CAM 모델의 학습과정이 안정적으로 수렴하였다.|-|
>|2|분류근거를 설명 가능한 Class activation map을 얻을 수 있는가?|CAM 방식과 Grad-CAM 방식의 class activation map이 정상적으로 얻어지며, 시각화하였을 때 해당 object의 주요 특징 위치를 잘 반영한다.|-|
>|3|인식결과의 시각화 및 성능 분석을 적절히 수행하였는가?|CAM과 Grad-CAM 각각에 대해 원본이미지합성, 바운딩박스, IoU 계산 과정을 통해 CAM과 Grad-CAM의 object localization 성능이 비교분석되었다.|-|
