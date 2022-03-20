# 4. 이미지 어디까지 우려볼까?

**텐서플로우의 random augmentation 기법을 적용해 보고, 최신 augmentation 기법을 익힌다. 직접 모델을 학습시켜 비교 실험을 진행해 본다.**

---

|-|목차|⏲ 360분|
|:---:|---|:---:|
|4-1| 들어가며 | 5분|
|4-2| Augmentation 적용 (1) 데이터 불러오기 🔒| 20분|
|4-3| Augmentation 적용 (2) Augmentation 적용하기 🔒| 30분|
|4-4| Augmentation 적용 (3) 비교실험 하기 🔒| 45분|
|4-5| 심화 기법 (1) Cutmix Augmentation 🔒| 50분|
|4-6| 심화 기법 (2) Mixup Augmentation 🔒| 30분|
|4-7| 프로젝트: CutMix 또는 Mixup 비교실험 하기 🔒| 180분|
|4-8| 프로젝트 제출 🔒|

---

### 실습목표

- Augmentation을 모델 학습에 적용하기
- Augmentation의 적용을 통한 학습 효과 확인하기
- 최신 data augmentation 기법 구현 및 활용하기

### 학습 내용

1. Augmentation 적용 (1) 데이터 불러오기
2. Augmentation 적용 (2) Augmentation 적용하기
3. Augmentation 적용 (3) 비교 실험하기
4. 심화 기법 (1) Cutmix Augmentation
5. 심화 기법 (2) Mixup Augmentation
6. 프로젝트: CutMix 또는 Mixup 비교 실험하기

---

- 사용할 데이터셋: [stanford_dogs](https://www.tensorflow.org/datasets/catalog/stanford_dogs)
  - 120개 견종의 이미지
  - 총 20,580장
  - 12,000장은 학습셋, 나머지 8,580장은 평가용 데이터셋
- Augmentation
  - 입력 이미지 데이터를 변경해주는 과정
  - 일반적인 이미지 데이터 전처리 방법과 활용방법이 동일하다.
- Augmentation 적용
  - Tensorflow Random Augmentation API 사용
    - `random_brightness()`
      - 밝기를 조절하여 다양한 환경에서 얻어진 이미지에 대응하도록 함
    - `random_contrast()`
    - `random_crop()`
    - `random_flip_left_right()`
      - 개 이미지는 좌우 대칭해도 문제 생기지 않음 -> 좌우대칭 적용을 통해 데이터 늘릴 수 있음
    - `random_flip_up_down()`
      - 상하대칭은 테스트 데이터셋 이미지 중에서 위아래가 뒤집힌 사진 없음 -> 적용X
    - `random_hue()`
    - `random_jpeg_quality()`
    - `random_saturation()`
- 이미지 전처리 함수 형태
  - 모든 이미지(훈련용, 테스트용)에 적용

```python
def 전처리_함수(image, label):   # 변환할 이미지와 라벨
    # 이미지 변환 로직 적용
    new_image = 이미지_변환(image)
    return new_image, label
```

- test-time augmentation
  - 여러 결과를 조합하기 위한 앙상블(ensemble) 방법 중 하나
  - 테스트 데이터셋에 augmentation을 적용
- augmentation 적용 전 vs 후 모델 성능 비교
  - 텐서플로우 케라스 ResNet50 중 imagenet 으로 훈련된 모델로 실험
    - include_top=False: 마지막 fully connected layer 포함X
      - 포함X => 특성 추출기(feature extractor)만 가져옴
      - 이미지넷과 우리의 테스트셋이 서로 다른 클래스를 가지므로 마지막에 추가해야 하는 fully connected layer 구조(=뉴런 개수)가 다르기 때문
  - Augmentation 적용 효과를 명확히 검증하기 위해서는 최소 EPOCHS=20 이어야 함
  - Augmentation 적용한 경우가 보다 천천히 학습되지만, EPOCH 10을 전후해서 aug_resnet50의 accuracy가 더 높게 형성됨
- [Cutmix](https://arxiv.org/pdf/1905.04899.pdf) Augmentation [[참고]](https://www.kaggle.com/cdeotte/cutmix-and-mixup-on-gpu-tpu)
  - 네이버 클로바(CLOVA)에서 발표
  - 일정 영역을 잘라서 붙여주는 방법
    - Mixup: 특정 비율로 픽셀별 값을 섞는 방식
    - Cutout: 이미지를 잘라내는 방식
  - 바운딩 박스(bounding box): 이미지에서 잘라서 섞어주는 영역
  - 이미지 섞기 -> 라벨 섞기
  - 면적에 비례해서 라벨을 섞음
  - 라벨 벡터는 보통 클래스를 표시하듯 클래스 1개만 1의 값을 가지는 원-핫 인코딩이 아니라 A와 B 클래스에 해당하는 인덱스에 각각 0.4, 0.6을 배분하는 방식을 사용
- [Mixup](https://arxiv.org/abs/1710.09412) Augmentation
  - 두 개 이미지의 픽셀별 값을 비율에 따라 섞어주는 방식
  - CutMix보다 구현이 간단하다.
  - 두 이미지 쌍을 섞을 비율은 일정한 범위 내에서 랜덤하게 뽑고, 해당 비율 값에 따라 두 이미지의 픽셀별 값과 라벨을 섞음

---

## 4-7. 프로젝트: CutMix 또는 Mixup 비교실험 하기

1. Augmentation을 적용한 데이터셋 만들기
2. 모델 만들기
3. 모델 훈련하기
4. 훈련 과정 시각화하기
5. Augmentation에 의한 모델 성능 비교

---

>## **루브릭**
>
>|번호|평가문항|상세기준|평가결과|
>|:---:|---|---|:---:|
>|1|CutMix와 MixUp 기법을 ResNet50 분류기에 성공적으로 적용하였는가?|CutMix와 MixUp을 적용한 데이터셋으로 훈련한 각각의 ResNet 모델이 수렴하였다.|-|
>|2|다양한 실험을 통해 태스크에 최적인 Augmentation 기법을 찾아내었는가?|Augmentation 적용을 통해 Augmentaion 미적용시 대비 5% 이상의 성능향상을 확인함|-|
>|3|여러가지 Augmentation 기법을 적용한 결과를 체계적으로 비교분석하였는가?|기본 Augmentation, CutMix, MixUp이 적용된 결과를 시각화와 함께 체계적으로 분석하였다.|-|
