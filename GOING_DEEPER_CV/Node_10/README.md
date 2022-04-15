# 10. 도로 영역을 찾자! - 세그멘테이션 모델 만들기

**시맨틱 세그멘테이션을 이용해 자율주행 차량을 위해 도로영역을 찾는 모델을 간단히 만들어 본다.**

---

[➡ [CV-10] U-Net_vs_U-Net_pp_in_Semantic_Segmentation.ipynb with nbviewer](https://nbviewer.org/github/HRPzz/AIFFEL/blob/main/GOING_DEEPER_CV/Node_10/%5BCV-10%5D%20U-Net_vs_U-Net_pp_in_Semantic_Segmentation.ipynb)

---

|-|목차|⏲ 270분|
|:---:|---|:---:|
|10-1| 들어가며 | 5분|
|10-2| 시맨틱 세그멘테이션 데이터셋 | 25분|
|10-3| 시맨틱 세그멘테이션 모델 | 30분|
|10-4| 시맨틱 세그멘테이션 모델 시각화 | 30분|
|10-5| 프로젝트 : 개선된 U-Net 모델 만들기 | 180분|
|10-6| 프로젝트 제출 |-|

---

## 실습목표

- 시맨틱 세그멘테이션 데이터셋을 전처리할 수 있습니다.
- 시맨틱 세그멘테이션 모델을 만들고 학습할 수 있습니다.
- 시맨틱 세그멘테이션 모델의 결과를 시각화할 수 있습니다.

## 학습내용

1. 시맨틱 세그멘테이션 데이터셋
2. 시맨틱 세그멘테이션 모델
3. 시맨틱 세그멘테이션 모델 시각화

---

## 10-5. 프로젝트 : 개선된 U-Net 모델 만들기

- Step 1. KITTI 데이터셋 수집과 구축
- Step 2. U-Net++ 모델의 구현
- Step 3. U-Net 과 U-Net++ 모델이 수행한 세그멘테이션 결과 분석

---

>## **루브릭**
>
>|번호|평가문항|상세기준|평가결과|
>|:---:|---|---|:---:|
>|1|U-Net을 통한 세그멘테이션 작업이 정상적으로 진행되었는가?|KITTI 데이터셋 구성, U-Net 모델 훈련, 결과물 시각화의 한 사이클이 정상 수행되어 세그멘테이션 결과 이미지를 제출하였다.|⭐|
>|2|U-Net++ 모델이 성공적으로 구현되었는가?|U-Net++ 모델을 스스로 구현하여 학습 진행 후 세그멘테이션 결과까지 정상 진행되었다.|⭐|
>|3|U-Net과 U-Net++ 두 모델의 성능이 정량적/정성적으로 잘 비교되었는가?|U-Net++ 의 세그멘테이션 결과 사진과 IoU 계산치를 U-Net과 비교하여 우월함을 확인하였다.|⭐|

---

## 노드 내용

- 이번 프로젝트: Semantic Segmentation(시맨틱 세그멘테이션) 을 이용해서 자율주행차량이 주행해야 할 도로 영역을 찾는 상황을 가정하고 모델 만들기
  - 이미지가 입력되면 도로의 영역을 Segmentation 하는 모델
    - U-Net
      - 구조<br>![U-Net](https://d3s0tskafalll9.cloudfront.net/media/images/u-net_1kfpgqE.max-800x600.png)
      - 시맨틱 세그멘테이션 모델 중 구조상 비교적 구현이 단순하다.
      - Conv2D, Conv2DTranspose, MaxPooling2D, concatenate, Dropout 또는 batch normalization 사용
    - U-Net++ [v1](https://arxiv.org/abs/1807.10165) [v2](https://arxiv.org/abs/1912.05074)
      - 구조<br>![U-Net++](https://d3s0tskafalll9.cloudfront.net/media/images/GC-5-P-UNPP.max-800x600.png)
      - U-Net의 네트워크 구조에 DenseNet의 아이디어를 가미하여 성능을 개선한 모델
  - 도로의 영역을 라벨(=이미지 형태)로 가진 데이터셋을 가지고 학습할 수 있도록 파싱 해야 함
  - 물체 검출(object detection)으로 사용했던 [KITTI 데이터셋의 세그멘테이션 데이터](http://www.cvlibs.net/datasets/kitti/eval_semantics.php) 다운로드
    - 도로 라벨: 7
    - 데이터 로더에 albumentations 라이브러리로 augmentation 적용
    - cf. imgaug 라이브러리도 있다.
    - tf.keras.utils.Sequence를 상속받은 generator 형태로 데이터 구성
  - 입력 이미지와 라벨을 한 번에 볼 수 있도록 모델의 출력값(=추론(inference)한 결과)을 입력 이미지 위에 겹쳐서 보이게(=오버레이(overray), PIL 패키지 Image.blend() 활용) 하기
  - 세그멘테이션이 성능 비교
    - 정성적 비교: 세그멘테이션 결과 시각화
    - 정량적 비교: IoU(Intersection over Union) 계산, 2가지 행렬 필요
      - prediction: 모델이 도로 영역이라고 판단한 부분이 1로, 나머지 부분이 0으로 표시된 행렬
      - target: 라벨 데이터에서 도로 영역이 1, 나머지 부분이 0으로 표시된 행렬
