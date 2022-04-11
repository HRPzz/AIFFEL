# 9. 물체를 분리하자! - 세그멘테이션 살펴보기

**픽셀 수준에서 이미지의 각 부분이 어떤 의미를 갖는 영역인지 분리를 해내는 세그멘테이션을 학습한다. 세그멘테이션의 종류, 주요 모델, 평가 기준을 알아본다.**

---

|-|목차|⏲ 240분|
|:---:|---|:---:|
|9-1| 들어가며 | 5분|
|9-2| 세그멘테이션 문제의 종류 | 15분|
|9-3| 주요 세그멘테이션 모델 (1) FCN | 60분|
|9-4| 주요 세그멘테이션 모델 (2) U-Net | 60분|
|9-5| 주요 세그멘테이션 모델 (3) DeepLab 계열 | 60분|
|9-6| 세그멘테이션의 평가 | 20분|
|9-7| Upsampling의 다양한 방법 | 20분|

---

## 실습 목표

- 세그멘테이션의 방식을 공부합니다.
- 시맨틱 세그멘테이션 모델의 결괏값을 이해합니다.
- 시맨틱 세그멘테이션을 위한 접근 방식을 이해합니다.

## 학습 내용

1. 세그멘테이션 문제의 종류
2. 주요 세그멘테이션 모델
    - FCN
    - U-NET
    - DeepLab 계열
3. 세그멘테이션의 평가
4. Upsampling의 다양한 방법

---

## 노드 내용

- 비교
  - 이미지 분류(image classification): 이미지에서 어떤 물체의 종류 분류
  - 객체 인식(object detection): 이미지에서 물체의 존재와 위치 탐지
  - 세그멘테이션(segmentation)
    - **픽셀 수준** 에서 이미지의 각 부분이 어떤 의미를 갖는 영역인지 분리해 내는 방법
    - 이미지의 영역을 **어떤 영역인지 분리** 해 내는 기술
- Segmentation 문제 종류
  - ![세그멘테이션 문제 종류](https://d3s0tskafalll9.cloudfront.net/media/original_images/semantic_vs_instance.png)
  - 시맨틱 세그멘테이션(semantic segmentation)
    - 어떤 물체들이 모여 있는 영역의 위치를 인식(localization)
    - 물체들이 양이라는 것을 판별(classification)
  - 인스턴스 세그멘테이션(instance segmentation)
    - 개별 양들의 개체 하나하나의 위치를 정확히 식별하는 객체 인식(object detection)
- Semantic Segmentation
  - U-Net
    - 구조<br>![U-Net 구조](https://d3s0tskafalll9.cloudfront.net/media/images/u-net.max-800x600.png)
    - 시맨틱 세그멘테이션의 대표적인 모델
    - 입력으로 572x572 크기인 이미지가 들어가고 출력으로 388x388의 크기에 두 가지의 클래스(=가장 마지막 레이어의 채널 개수 2)를 가진 세그멘테이션 맵(segmentation map)이 나옴
    - 두 가지의 클래스를 문제에 따라 다르게 정의하면 클래스에 따른 시맨틱 세그멘테이션 맵(semantic segmentation map) 을 얻을 수 있다.
- Instance Segmentation
  - 같은 클래스 내에서도 각 개체(instance)들을 분리하여 세그멘테이션 수행
  - Mask R-CNN
    - 2-Stage Object Detection의 가장 대표적인 Faster-R-CNN을 계승
      - Faster-R-CNN 의 RoIPool
        - 다양한 RoI 영역을 Pooling을 통해 동일한 크기의 Feature map으로 추출 -> 고정 사이즈의 Feature map을 바탕으로 바운딩 박스와 object의 클래스 추론
        - RoIPool 과정에서 Quantization이 필요하다. => 시맨틱 세그멘테이션의 정보손실과 왜곡을 야기함!<br>![시맨틱 세그멘테이션에서 RoIPool 문제점](https://d3s0tskafalll9.cloudfront.net/media/images/GC-5-L-RoIPool-01.max-800x600.png)
    - 사용한 아이디어 2개: RoIAlign, 클래스별 마스크 분리
      - RoIAlign
        - Quantization하지 않고도 RoI를 처리할 고정 사이즈의 Feature map을 생성할 수 있게 아이디어 제공
        - RoI 영역을 pooling layer의 크기에 맞추어 등분한 후, RoIPool을 했을 때의 quantization 영역 중 가까운 것들과의 bilinear interpolation 계산을 통해 생성해야 할 Feature Map을 계산해 낸다.
      - 클래스별 마스크 분리
        - 피처 맵(feature map)의 크기를 키워 마스크(mask)를 생성해 내는 부분을 통해 인스턴스에 해당하는 영역(=인스턴스 맵) 추론
        - 클래스에 따른 마스크를 예측할 때, 여러 가지 태스크를 한 모델로 학습하여 물체 검출의 성능을 높임
    - 클래스별 Object Detection과 시멘틱 세그멘테이션을 사실상 하나의 Task로 엮어낸 것으로 평가받는 중요한 모델
- 주요 세그멘테이션 모델
  - [FCN(Fully Convolutional Network)](https://arxiv.org/abs/1411.4038)
    - AlexNet, VGG-16 등의 모델을 세그멘테이션에 맞게 변형한 모델
      - 기본적인 VGG 모델: 이미지의 특성을 추출하기 위한 네트워크의 뒷단에 fully connected layer를 붙여서 계산한 클래스별 확률을 바탕으로 이미지 분류 수행
    - 세그멘테이션을 하기 위해서 네트워크 뒷단에 fully connected layer 대신 **CNN** 을 붙임
      - **위치정보** 를 유지하면서 클래스 단위의 히트맵(heat map)을 얻어 세그멘테이션을 하기 위함
      - 위치의 특성을 유지하면서 이미지 분류를 하기 위해서 마지막 CNN은 1x1의 커널 크기(kernel size)와 클래스의 개수만큼의 채널을 가짐 => 클래스 히트맵을 얻을 수 있다.
      - CNN과 pooling 레이어를 거치면서 크기가 줄었기 때문에 히트맵의 크기는 일반적으로 원본 이미지보다 작다. -> Upsampling만 하면 원하는 세그멘테이션 맵을 얻을 수 있다.
        - Upsampling 방법 중 FCN 에서는 Deconvolution과 Interpolation 방식 활용
          - Deconvolution: 컨볼루션 연산을 거꾸로 해준 것
          - Interpolation: 보간법으로 주어진 값들을 통해 추정해야 하는 픽셀(=특성 맵의 크기가 커지면서 메꾸어야 하는 중간 픽셀들)을 추정하는 방법
    - 논문에서는 더 나은 성능을 위해서 Skip Architecture라는 방법 사용
  - [U-Net](https://arxiv.org/pdf/1505.04597.pdf)
    - 네트워크 구조가 U자 형태
      - 좌측의 Contracting path
        - 2개의 3x3 conv layer, ReLU, 2x2 커널 2 strides max pooling(=down sampling) => 다음 conv 채널 크기 2배씩 늘어남
      - 우측의 Expansive path
        - 2x2 up-conv layer 를 통해 채널이 절반 줄어들고 특성 맵 크기 증가, 2개의 3x3 conv layer 사용
      - 두 Path에서 크기가 같은 블록의 출력과 입력은 skip connection처럼 연결함 => low-level의 feature 활용
      - 마지막에는 1x1 convolution으로 원하는 시맨틱 세그멘테이션 맵을 얻을 수 있다.
    - FCN에서 upsampling을 통해서 특성 맵을 키운 것을 입력값과 **대칭** 적으로 만들어 준 것
    - 본래 의학 관련 논문으로 시작되었다.
    - 입력으로 572x572 크기인 이미지가 들어가고 출력으로 388x388의 크기에 두 가지의 클래스를 가진 세그멘테이션 맵(segmentation map)이 나옴 => 세그멘테이션 맵을 원본 이미지에 맞게 크기를 조정(=resize)해 주면 위에서 봤던 우리가 원하는 시맨틱 세그멘테이션 결과를 얻을 수 있다.
    - 타일(tile) 방식을 사용해서 네트워크 추론 => 큰 이미지에서도 **높은 해상도의 세그멘테이션 맵** 을 얻을 수 있다.
    - 클래스 간 데이터 양의 불균형을 해결해 주기 위해서 분포를 고려한 weight map을 학습 때 사용
      - 여기서 말하는 weight는 손실 함수(loss)에 적용되는 가중치를 의미함
      - 의료 영상에서 세포 내부나 배경보다는 상대적으로 면적이 작은 세포 경계를 명확하게 추론해 내는 것이 더욱 중요하기 때문에, 세포 경계의 손실에 더 많은 페널티를 부과하는 방식
  - [DeepLab v3+](https://arxiv.org/pdf/1802.02611.pdf)
    - 구조<br>![DeepLab v3+ 구조](https://d3s0tskafalll9.cloudfront.net/media/images/deeplab_v3.max-800x600.png)
      - 인코더(Encoder)
        - U-Net에서의 Contracting path 역할
        - 이미지에서 필요한 정보를 특성으로 추출해 내는 모듈
      - 디코더(Decoder)
        - U-Net에서의 Expansive path의 역할
        - 출된 특성을 이용해 원하는 정보를 예측하는 모듈
    - 핵심 아이디어 2가지: Atrous Convolution, Spatial Pyramid Pooling
      - Spatial Pyramid Pooling
        - 여러 가지 스케일로 convolution과 pooling을 하고 나온 다양한 특성 연결(concatenate) => 멀티스케일로 특성을 추출하는 것을 병렬로 수행하는 효과
        - 컨볼루션을 Atrous Convolution으로 바꾸어 적용한 것 == Atrous Spatial Pyramid Pooling
        - 장점
          - 입력 이미지의 크기와 관계없이 동일한 구조를 활용할 수 있다.
          - 제각기 다양한 크기와 비율을 가진 RoI 영역에 대해 적용하기에 유리하다.
      - Atrous Convolution 사용
        - 띄엄띄엄 보는 컨볼루션
        - 컨볼루션 레이어를 너무 깊게 쌓지 않아도 넓은 영역의 정보를 커버할 수 있다.
      - ASPP(Atrous Spatial Pyramid Pooling)
        - Atrous Convolution을 여러 크기에 다양하게 적용한 것
        - DeepLab V3+는 ASPP가 있는 블록을 통해 특성을 추출하고 디코더에서 Upsampling을 통해 세그멘테이션 마스크를 얻음
- 세그멘테이션 평가
  - 시맨틱 세그멘테이션의 결괏값: 이미지의 크기에 맞는 세그멘테이션 맵 크기, 시맨틱 클래스의 수에 맞는 채널 크기 => 각 채널의 max probability에 따라서 해당 위치의 클래스가 결정됨
  - 픽셀별 정확도 (Pixel Accuracy)
    - ![Pixel Accuracy](https://d3s0tskafalll9.cloudfront.net/media/images/error_metric.max-800x600.jpg)
    - 픽셀에 따른 이미지 분류 문제
    - 예측 결과 맵(prediction map)을 클래스 별로 평가하는 경우에는 이진 분류 문제(binary classification)로 생각해 픽셀 및 채널 별로 평가 => 픽셀 별로 정답 클래스를 맞추었는지 여부, 즉 True/False를 구분
  - 마스크 IoU (Mask Intersection-over-Union)
    - 물체 검출 모델을 평가: 정답 라벨(ground truth)와 예측 결과 바운딩 박스(prediction bounding box) 사이의 IoU(intersection over union) 사용
    - 마스크 IoU를 클래스 별로 계산하면 한 이미지에서 여러 클래스에 대한 IoU 점수를 얻을 수 있다. -> 평균하면 전체적인 시맨틱 세그멘테이션 성능을 가늠할 수 있다.
- Up sampling 방법
  - Nearest Neighbor
    - scale을 키운 위치에서 원본에서 가장 가까운 값을 그대로 적용하는 방법
  - Bilinear Interpolation
    - 두 개의 축(Bilinear)에 대해서 선형보간법(Linear interpolation)을 통해 필요한 값을 메우는 방식
    - cf. Bicubic interpolation의 경우 삼차보간법 사용
  - Transposed Convolution
    - 학습할 수 있는 파라미터를 가진 Upsampling 방법
    - 학습된 파라미터로 입력된 벡터를 통해 더 넓은 영역의 값을 추정
