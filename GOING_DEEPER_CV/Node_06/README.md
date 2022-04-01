# 6. 나를 찾아줘 - Class Activation Map 만들기

**CAM, Grad-CAM을 위한 모델을 직접 만들고, CAM을 추출해 시각화 해본다. CAM을 Object detection에 적용해 결과를 평가해 본다.**

---

[➡ [CV-06] CAM_vs_Grad_CAM.ipynb with nbviewer](https://nbviewer.org/github/HRPzz/AIFFEL/blob/main/GOING_DEEPER_CV/Node_06/%5BCV-06%5D%20CAM_vs_Grad_CAM.ipynb)

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

---

- CAM(Class Activation Map)
  - 특성 추출 CNN + GAP(Global Average Pooling) + 소프트맥스 레이어(softmax layer)가 붙는 형태
  - 클래스에 대한 활성화 정도를 나타낸 지도
  - 분류(Classfication) 문제
  - 프로젝트 모델 구성
    - ResNet50 기반
    - pooling layer
    - softmax 활성화 함수
    - GAP 연산
  - conv5_block3_out의 output = feature map
  - CAM 생성에 필요한 것
    - (1) 특성 맵
    - (2) 클래스별 확률을 얻기 위한 소프트맥스 레이어의 가중치
    - (3) 원하는 클래스의 출력값

```python
# CAM 모델 구성
num_classes = ds_info.features["label"].num_classes
base_model = keras.applications.resnet50.ResNet50(
    include_top=False,    # Imagenet 분류기  fully connected layer 제거
    weights='imagenet',
    input_shape=(224, 224, 3),
    pooling='avg',      # GAP를 적용  
)
x = base_model.output
preds = keras.layers.Dense(num_classes, activation='softmax')(x)
cam_model = keras.Model(inputs=base_model.input, outputs=preds)
```

- Grad-CAM
  - 모델의 구조에 제약이 없음
  - 프로젝트 모델 구성 => CAM 모델 재활용
    - ResNet50 기반
    - pooling layer
    - softmax layer: softmax 활성화 함수를 사용하는 fully connected layer
  - 관찰을 원하는 레이어와 정답 클래스에 대한 예측값 사이의 그래디언트 구함 -> GAP 연산 -> 관찰 대상이 되는 레이어의 채널별 가중치를 구함 -> (레이어 채널별 가중치, 레이어 채널별 특성 맵 가중합 => 최종 CAM 이미지 구하기)
  - 어떤 레이어든 CAM 이미지를 뽑아낼 수 있음
- 데이터셋 [stanford_dogs](https://www.tensorflow.org/datasets/catalog/stanford_dogs)
  - 120 종의 개를 사진으로 판별하는 분류 문제 데이터셋
  - 라벨이 위치 정보인 바운딩 박스(bounding box) 정보를 포함
  - 12,000장의 학습용 데이터셋 + 8,580장의 평가용 데이터셋
  - minmax 방식으로 바운딩 박스 라벨링
- 바운딩 박스(Bounding Box)
  - bbox
  - [BBoxFeature](https://www.tensorflow.org/datasets/api_docs/python/tfds/features/BBoxFeature) 타입
  - 물체의 위치를 사각형 영역으로 표기하는 방법
  - 라벨링 방법
    - xywh
      - 바운딩박스 중심점/좌측상단을 x, y로 표기
      - 사각형의 너비 w와 높이 h를 표기
    - minmax
      - 바운딩박스를 이루는 좌표의 최소값과 최대값을 통해 표기하는 방법
      - 좌표의 절대값이 아니라, 전체 이미지의 너비와 높이를 기준으로 normalize한 상대적인 값을 표기하는 것이 일반적
    - 이미지의 상하좌우 끝단으로부터 거리로 표현하는 방법
    - LRTB: 좌우측의 x값과 상하측의 y값 네 개로 표시하는 방법
    - QUAD: 네 점의 x, y 좌표 값을 모두 표시하는 방법
- Object Detection: CAM 이미지에서 물체 위치 찾기
- IoU(Intersection Over Union)
  - ![IoU](https://d3s0tskafalll9.cloudfront.net/media/images/GC-3-P-3.max-800x600.jpg)
  - 정답 바운딩 박스(ground truth bbox)와 CAM/Grad-CAM 으로 얻은 바운딩 박스(prediction bbox) 비교/평가 지표
  - 두 영역의 교집합인 intersection 영역의 넓이를 두 영역의 합집합인 union 영역으로 나누어준 값
  - 찾고자 하는 물건의 절대적인 면적과 상관없이, 영역을 정확하게 잘 찾아내었는지의 상대적인 비율을 구할 수 있음
