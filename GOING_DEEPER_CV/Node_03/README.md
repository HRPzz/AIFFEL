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
