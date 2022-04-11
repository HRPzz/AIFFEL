# 7. Object Detection

**Object detection 문제와 이를 해결하기 위한 다양한 detection 모델들을 알아본다.**

---

|-|목차|⏲ 180분|
|:---:|---|:---:|
|7-1| 들어가며 | 5분|
|7-2| 용어 정리 | 25분|
|7-3| Localization | 20분|
|7-4| Detection (1) 슬라이딩 윈도우, 컨볼루션 | 25분|
|7-5| Detection (2) 앵커 박스, NMS | 25분|
|7-6| Detection Architecture | 50분|
|7-7| Anchor | 30분|

---

## 학습 목표

- 딥러닝 기반의 Object detection 기법을 배워갑니다.
- Anchor box의 개념에 대해 이해합니다.
- single stage detection 모델과 two stage detection 모델의 차이를 이해합니다.

---

## 노드 내용

- Object detection
  - 이미지 내에서 물체의 위치와 그 종류를 찾아내는 것
  - 쓰이는 곳: 자율 주행을 위해서 차량/사람 찾아내기, 얼굴 인식을 위해 사람 얼굴을 찾아내는 경우, 등등
- 객체 검출 용어 정리
  - Classification: 주어진 이미지 안의 물체가 무엇인지 알아내는 것
  - Localization
    - 주어진 이미지 안의 물체가 어느 위치에 있는지 찾아내는 것
    - Bounding Box라는 사각형 형태로 위치를 나타냄
  - Object Detection
    - 주어진 이미지 안의 물체가 무엇인지, 어디에 있는지 모두 알아내는 것
    - 보통 여러 물체를 찾아냄
    - 사각형으로 나타냄
  - Semantic Segmentation
    - 주어진 이미지 안의 물체의 영역(Object Mask)을 알아내는 것
    - 실제 영역을 표현해야 하므로 사각형으로 나타낼 수 없음 => 각각 그 모습 그대로 파악
    - 강아지가 두 마리인 경우 두 강아지를 별도로 구분하지 않음
  - Instance Segmentation
    - Semantic Segmentation과 동일한 역할
    - 강아지가 두 마리인 경우 두 강아지를 별도로 구분 => 서로 다르게 mask
- 바운딩 박스(Bounding box)
  - 모델이 추론한 물체의 위치가 표현된 박스 or 물체 위치의 실제값으로 데이터셋에 준비된 라벨 => 물체마다 1개 존재
  - 2개의 점을 표현하는 방법 2가지
    - 전체 이미지의 좌측 상단을 원점으로 정의하고 바운딩 박스의 좌상단 좌표와 우하단 좌표 두 가지 좌표로 표현
    - 이미지 내의 절대 좌표로 정의하지 않고 바운딩 박스의 폭과 높이로 정의 => 좌측 상단의 점에 대한 상대적인 위치로 물체의 위치 정의
- IoU(Intersection over Union)
  - localization 모델 인식 결과 평가 지표(metric)
  - 면적의 절대적인 값에 영향을 받지 않도록 두 개 박스의 차이를 상대적으로 평가하기 위한 방법
  - 교차하는 영역을 합친 영역으로 나눈 값 => 정답(Ground truth)와 예측값의 영역이 일치하는 경우 IoU=1 이 된다.
- Localization
  - 일정한 크기의 입력 이미지에 어떤 물체가 있는지 확인하고 그 위치를 찾아내는 방법
- Object Detection
  - 여러 물체를 한꺼번에 찾아내기
  - 슬라이딩 윈도우(Sliding Window)
    - 전체 이미지를 적당한 크기의 영역으로 나눈 후에, 각각의 영역에 대해 이전 스텝에서 만든 Localization network를 반복 적용해 보는 방식
    - Localization network의 입력으로 만들기 위해서 원본 이미지에서 잘라내는 크기를 윈도우 크기로 하여, 동일한 윈도우 사이즈의 영역을 이동시키면서(sliding) 수행해 주는 방식
    - 단점: 처리해야할 window 갯수만큼 시간이 더 걸림(순차 실행)
  - 컨볼루션(Convolution)
    - Sliding window의 단점인 연산량과 속도를 개선하기 위한 방법 => 병렬적으로 동시에 진행되므로 convolution은 속도 면에서 훨씬 효율적
  - 앵커 박스(Anchor box)
    - 서로 다른 형태의 물체와 겹친 경우에 대응
    - 모델이 추론해야 할 위치의 후보들 => 다양한 앵커 박스를 만들어서 그중에 물체가 있을 확률이 가장 높은 앵커 박스를 물체의 바운딩 박스라고 예측
    - 학습 데이터의 라벨도 앵커 박스 형태로 바꿔야 사용 가능
    - 차와 사람을 Detection하기 위한 모델의 output이 4x4 grid이고 각 cell에 Anchor box를 9개씩 정의한 경우 output의 dimension = [batch_size, 4, 4, 63 9x(1+4+2)]
      - 채널 63 = 앵커박스 9개 * (한 개의 그리드 셀에 대한 결괏값 벡터가 물체가 있을 확률 + 클래스 개수 2개 + 바운딩 박스 4개)
  - NMS(Non-Max Suppression)
    - 겹친 여러 개의 바운딩 박스를 하나로 줄여줄 수 있는 방법
    - 겹친 박스들이 있을 경우 가장 확률이 높은 박스를 기준으로 기준이 되는 IoU 이상인 것들을 없앰
      - IoU 기준인 이유: 어느 정도 겹치더라도 다른 물체가 있는 경우가 있을 수 있기 때문
    - 같은 class인 물체를 대상으로 적용

---

- 정리
  - Convolutional implementation of Sliding Windows
    - Convolution으로 슬라이딩 윈도우를 대체 => 여러 영역에서 Object localization을 병렬로 수행 => 속도 측면의 개선
  - Anchor box
    - 겹친 물체가 있을 때, IoU를 기준으로 서로 다른 Anchor에 할당하도록 하여 생긴 영역이 다른 물체가 겹쳤을 때도 물체를 검출할 수 있도록 할 수 있음
  - Non-max suppression(NMS)
    - 딥러닝 모델에서 나온 Object detection 결과들 중 겹친 결과들을 하나로 줄이면서 더 나은 결과를 얻을 수 있음

---

- Detection Architecture
  - ![stage detector](https://d3s0tskafalll9.cloudfront.net/media/images/GC-4-L-11.stage_comparison.max-800x600.png)
  - ![Object Detection Milestones](https://d3s0tskafalll9.cloudfront.net/media/images/gc-4v2-l-5-1.max-800x600.png)
  - Single stage detector
    - 객체의 검출과 분류, 그리고 바운딩 박스 regression을 한 번에 하는 방법 => 속도면에서는 빠름
    - YOLO (You Only Look Once)
      - 이미지를 그리드로 나누고, 슬라이딩 윈도 기법을 컨볼루션 연산으로 대체해 Fully Convolutional Network 연산을 통해 그리드 셀 별로 바운딩 박스를 얻어낸 뒤 바운딩 박스들에 대해 NMS를 한 방식
      - 그리드 셀마다 클래스를 구분하는 방식 => 두 가지 클래스가 한 셀에 나타나는 경우 정확하게 동작하지 않음
      - 매우 빠른 인식 속도
      - 특성이 담고 있는 정보가 동일한 크기의 넓은 영역을 커버하기 때문에 작은 물체를 잡기에 적합하지 않음
    - SSD (Single-Shot Multibox Detector)
      - 다양한 크기의 특성 맵으로부터 classification과 바운딩 박스 regression을 수행 => 다양한 크기의 물체에 대응
  - Two stage detector
    - R-CNN
      - 물체가 있을 법한 후보 영역을 뽑아내는 "Region proposal" 알고리즘과 후보 영역을 분류하는 CNN을 사용
      - region proposal을 selective search(비 신경망 알고리즘)로 수행한 뒤 후보 이미지 각각에 대해서 컨볼루션 연산 수행 => 한 이미지에서 특성을 반복해서 추출하기 때문에 비효율적이고 느리다.
    - Fast R-CNN
      - 후보 영역의 classification과 바운딩 박스 regression을 위한 특성을 한 번에 추출하여 사용
      - 한 번의 CNN을 거쳐 그 결과물을 재활용할 수 있으므로 연산수를 줄임
      - 잘라낸 특성 맵의 영역의 크기를 모두 같은 크기로 변형하기 위해 RoI(Region of Interest) Pooling 적용
      - 반복되는 CNN 연산을 크게 줄여냈지만 region proposal 알고리즘이 병목되는 문제 발생
    - Faster R-CNN
      - 기존의 Fast R-CNN을 더 빠르게 만들기 위해서 region proposal 과정에서 **RPN(Region Proposal Network)** 라고 불리는 신경망 네트워크 사용
        - RPN: 특성 맵을 보고 후보 영역들을 얻어내는 네트워크
      - 후보 영역을 얻어낸 다음은 Fast R-CNN과 동일
- Anchor
  - cf. Anchor를 쓰지 않는 FCOS(Fully Convolutional One-Stage Object Detection) 방법도 있다. but 많은 방법들이 Anchor를 기반으로 구현되고 있다.
  - Anchor box == 물체를 예측하기 위한 기준이 되는 Box
  - Anchor를 기반으로 Loss를 계산하는 방식 2가지(e.g. YOLO, Faster-RCNN)
    - Background IoU threshold
    - Foreground IoU threshold
  - 객체(=bounding box)와 Anchor box의 IoU가 0.7이상일 경우 Foreground로 할당하고 0.3 이하일 경우는 배경(Background)으로 할당 => 0.3과 0.7 중간인 Anchor는 불분명한 영역으로 학습에 활용하지 않음
  - 다양한 물체에 Anchor Box가 걸쳐있는 경우 가장 높은 IoU를 가진 물체의 Bounding box를 기준으로 계산
  - 탐지하고자 하는 물체에 따라서 Anchor box의 크기나 aspect ratio를 조정해 주어야 함<br>![Anchor](https://d3s0tskafalll9.cloudfront.net/media/images/GC-4-L-21.max-800x600.jpg)
    - 영역에 들어간 Anchor box이라도 교차(intersection)하는 면적이 작기 때문에 IoU가 작아 매칭이 되지 않는 경우가 발생할 수 있다.
    - 세로로 긴 물체를 주로 탐지해야 하면 세로로 긴 Anchor box를 많이 만들고 Matching되는 box를 늘려야 학습이 잘 될 수 있다.
  - Anchor를 많이 둘 수록 더 좋은 Recall을 얻음 => 적절하지 않은 Anchor는 틀린 Detection 결과를 만들어서 전체 성능을 낮추게 됨
- Bounding box Regression
  - YOLOv3의 전신인 YOLOv2부터 Bounding box Regression 방식 사용
  - Bounding box를 예측하기 위해 bounding box의 중심점($b_x, b_y$), 그리고 width($b_w$)와 height($b_h$)를 예측해야 함 => anchor box의 정보 $c_x, c_y, p_w, p_h$ 와 연관지어 찾는 방법 사용
  - 기존의 Anchor box 위 좌측 상단 $c_x, c_y$ 이고 width, height $p_w, p_h$ 일 때, 이를 얼마나 x축 또는 y축 방향으로 옮길지 그리고 크기를 얼마나 조절해야 하는지를 예측하여 물체의 bounding box를 추론
