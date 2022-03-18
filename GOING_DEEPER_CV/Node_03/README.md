# 3. 잘 만든 Augmentation, 이미지 100장 안 부럽다

**데이터셋이 부족한 상황을 해결하기 위한 Data Augmentation의 다양한 기법에 대해 알아보고, 활용 가능한 라이브러리, 실전상황에서 주의해야 할 팁 등을 정리해 본다.**

---

|-|목차|⏲ 210분|
|:---:|---|:---:|
|3-1| 들어가며 | 5분|
|3-2| 데이터셋의 현실 🔒| 5분|
|3-3| Data Augmentation이란? (1) 개요 🔒| 15분|
|3-4| Data Augmentation이란? (2) 다양한 Image Augmentation 방법 🔒| 20분|
|3-5| 텐서플로우를 사용한 Image Augmentation (1) Flip 🔒| 20분|
|3-6| 텐서플로우를 사용한 Image Augmentation (2) Center Crop 🔒| 20분|
|3-7| 텐서플로우를 사용한 Image Augmentation (3) 직접 해보기 🔒| 40분|
|3-8| albumentations 라이브러리 🔒| 80분|
|3-9| 더 나아간 기법들 🔒| 5분|

---

### 실습목표

- Augmentation을 하는 이유를 알아갑니다.
- 여러 가지 Augmentation 방법을 알아둡니다.
- 학습에 Augmentation을 적용할때 주의해야 할 점을 숙지합니다.

### 학습 내용

1. 데이터셋의 현실
2. Data Augmentation이란?
3. 텐서플로우를 사용한 Image Augmentation
4. imgaug 라이브러리
5. 더 나아간 기법들

---

- 데이터셋
  - 데이터를 몇 만 장씩 구축하는 데는 많은 비용과 시간이 필요
  - 제한된 데이터셋 활용: augmentation
- Data Augmentation
  - 갖고 있는 데이터셋을 여러 가지 방법으로 증강시켜(augment) 실질적인 학습 데이터셋의 규모를 키울 수 있는 방법
  - 하드디스크에 저장된 이미지 데이터를 메모리에 로드한 후, 학습시킬 때 변형(노이즈 삽입)을 가하는 방법
  - 데이터가 많을수록 과적합(overfitting)을 줄일 수 있다.
- Image Augmentation
  - 텐서플로우 API
  - Albummentations 라이브러리
  - GAN (딥러닝 활용)
- 텐서플로우 API
  - Flipping
    - 이미지 좌우/상하 반전/대칭
      - 일괄 적용
        - flip_lr_tensor = tf.image.flip_left_right(image_tensor)
        - flip_ud_tensor = tf.image.flip_up_down(image_tensor)
        - flip_lr_image = tf.keras.preprocessing.image.array_to_img(flip_lr_tensor)
        - flip_ud_image = tf.keras.preprocessing.image.array_to_img(flip_ud_tensor)
      - 확률 적용
        - flip_lr_tensor = tf.image.random_flip_left_right(image_tensor)
        - flip_ud_tensor = tf.image.random_flip_up_down(image_tensor)
        - flip_lr_image = tf.keras.preprocessing.image.array_to_img(flip_lr_tensor)
        - flip_ud_image = tf.keras.preprocessing.image.array_to_img(flip_ud_tensor)
    - 물체 탐지(detection), 세그멘테이션(segmentation) 문제에서는 라벨도 같이 좌우 반전
    - 알파벳 문자를 인식(recognition) 문제는 상하/좌우 반전으로 다른 글자가 될 수 있으니 주의
  - Gray scale
    - 3가지(RGB) 채널(channel) -> 1가지 채널
    - weight_R + weight_G + weight_B = 1
  - Saturation
    - RGB -> HSV, S(saturation) 채널에 오프셋(offset) 적용 -> RGB
    - cf. 색상 모델
      - Gray
        - 밝기(intensity) 정보만으로 영상 표현
        - 검정(0) ~ 흰색(256)
      - RGB
        - R, G, B 각각은 0 ~ 255 사이의 값을 가짐
        - 256 *256* 256 = 16,777,216 가지의 색 표현
      - HSV
        - Hue(색조): 붉은색 계열인지 푸른색 계열인지 => 색 종류 나타냄, 크기 의미 없음
        - Saturation(채도): 그 색이 얼마나 선명한(순수한) 색인지 => 0 이면 무채색(gray), 255 면 가장 선명(순수)한 색
        - Value(명도): 밝기(Intensity)
        - H, S, V 각각은 0 ~ 255 사이의 값으로 표현
      - YCbCr(=YUV)
        - RGB 색에서 밝기 성분(Y)과 색차 정보(Cb, Cr)를 분리하여 표현
        - Y, Cb, Cr 은 각각 0 ~ 255 사이의 값을 가짐
  - Brightness
    - 밝기 조절
    - 반드시 tf.image.random_brightness() 다음에는 tf.clip_by_value()를 적용해야 함
      - random_bright_tensor = tf.image.random_brightness(image_tensor, max_delta=128)
      - random_bright_tensor = tf.clip_by_value(random_bright_tensor, 0, 255)
      - random_bright_image = tf.keras.preprocessing.image.array_to_img(random_bright_tensor)
  - Rotation
    - 이미지 각도 변환
    - 90도 단위로 돌리지 않는 경우 직사각형 형태에서 기존 이미지로 채우지 못하는 영역을 어떻게 처리해야 할지 유의
  - Center Crop
    - 이미지 중앙을 기준으로 확대
      - 일괄 적용
        - cropped_tensor = tf.image.central_crop(image_tensor, frac)  # frac: 얼마나 확대할 것인지 조절
        - cropped_img = tf.keras.preprocessing.image.array_to_img(cropped_tensor)
      - 랜덤 적용
        - 랜덤하게 centeral_crop을 적용하는 함수는 텐서플로우에서 기본적으로 제공되지 않음
        - central_fraction = tf.random.uniform([1], minval=range[0], maxval=range[1], dtype=tf.float32)  # 텐서플로우의 랜덤 모듈 사용
          - tf.random.uniform(): 랜덤값을 uniform distribution으로 뽑는 것
          - tf.random.normal(): 랜덤값을 normal distribution으로 뽑는 것
        - cropped_tensor = tf.image.central_crop(image_tensor, central_fraction)
    - e.g. 이미지 내에 고양이의 형상을 유지해야 하고 털만 보이는 이미지를 만들어서는 안 된다.
  - Random Crop
    - random_crop_tensor = tf.image.random_crop(image_tensor,[180,180,3])
    - random_crop_image = tf.keras.preprocessing.image.array_to_img(random_crop_tensor)
  - 그 외
    - Gaussian noise
    - Contrast change
    - Sharpen
    - Affine transformation
    - Padding
    - Blurring
- Albummentations 라이브러리
  - 이미지 증강을 위한 Python 라이브러리
  - 이미지 증강은 딥 러닝 및 컴퓨터 비전 작업에서 훈련된 모델의 품질을 높이는 데 사용
  - PIL Image 데이터형을 넘파이(numpy) 배열로 변환하여 사용
  - 기법
    - transforms.Affine()
      - 아핀 변환(Affine transform)을 이미지에 적용
      - 아핀 변환: 이미지의 스케일(scale)을 조절하거나 평행이동, 또는 회전 등의 변환을 줌
    - ransforms.RandomCrop()
    - MedianBlur()
    - ToGray()
    - MultiplicativeNoise()
  - 여러 가지의 augmentation 기법을 순차적으로 적용하는 것도 가능

```python
import albumentations as A

for i in range(10):
    transform = A.Compose([
        A.Affine(rotate=(-45, 45),scale=(0.5,0.9),p=0.5) ,
        A.ToGray(p=1),
        A.MultiplicativeNoise(multiplier=[0.5, 1.5], 
                            elementwise=True, per_channel=True, p=1),
        A.RandomCrop(width=256, height=256)
    ])
    transformed = transform(image=image_arr)
    plt.figure(figsize=(12,12))
    plt.imshow((transformed['image']))
    plt.show()
```

- GAN
  - 딥러닝을 활용한 data augmentation 방식
  - e.g. 스타일 트랜스퍼(style transfer) 모델
