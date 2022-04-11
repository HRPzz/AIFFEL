# 8. GO/STOP! - Object Detection 시스템 만들기

**Object detection 모델을 사용해 자동차 또는 사람이 가까이 있는지 확인한 후 멈출 수 있는 시스템을 만든다.**

---

[➡ [CV-08] Object_Detection_with_RetinaNet.ipynb with nbviewer](https://nbviewer.org/github/HRPzz/AIFFEL/blob/main/GOING_DEEPER_CV/Node_08/%5BCV-08%5D%20Object_Detection_with_RetinaNet.ipynb)

---

|-|목차|⏲ 370분|
|:---:|---|:---:|
|8-1| 들어가며 | 10분|
|8-2| 자율주행 보조장치 (1) KITTI 데이터셋 | 20분|
|8-3| 자율주행 보조장치 (2) 데이터 직접 확인하기 | 30분|
|8-4| RetinaNet | 30분|
|8-5| 데이터 준비 | 30분|
|8-6| 모델 작성 | 20분|
|8-7| 모델 학습 | 90분|
|8-8| 결과 확인하기 | 20분|
|8-9| 프로젝트: 자율주행 보조 시스템 만들기 | 120분|
|8-10| 프로젝트 제출 |-|

---

## 실습 목표

1. Object detection 모델을 학습할 수 있습니다.
2. RetinaNet 모델을 활용한 시스템을 만들 수 있습니다.

## 학습 내용

1. 자율주행 보조장치
2. RetinaNet
3. 데이터 준비부터 모델 학습, 결과 확인까지
4. 프로젝트: 자율주행 보조 시스템 만들기

---

## 8-9. 프로젝트: 자율주행 보조 시스템 만들기

1. 자율주행 시스템 만들기
2. 자율주행 시스템 평가하기

---

>## **루브릭**
>
>|번호|평가문항|상세기준|평가결과|
>|:---:|---|---|:---:|
>|1|KITTI 데이터셋에 대한 분석이 체계적으로 진행되었다.|KITTI 데이터셋 구조와 내용을 파악하고 이를 토대로 필요한 데이터셋 가공을 정상 진행하였다.|⭐|
>|2|RetinaNet 학습이 정상적으로 진행되어 object detection 결과의 시각화까지 진행되었다.|바운딩박스가 정확히 표시된 시각화된 이미지를 생성하였다.|⭐|
>|3|자율주행 Object Detection 테스트시스템 적용결과 만족스러운 정확도 성능을 달성하였다.|테스트 수행결과 90% 이상의 정확도를 보였다.|⭐|

---

## 노드 내용

- 오늘 우리가 다루고자 하는 것
  - Grad-CAM(=약지도학습) 을 통한 Object Detection과 Semantic Segmentation
  - 자율주행 연구에 약지도학습을 활용한 예: [네이버랩스의 이미지기반의 차선변경 알고리즘(SLC)은 무엇을 보면서 판단을 할까?](https://www.naverlabs.com/storyDetail/16)
  - inaccurate supervision : 학습 데이터에 Noise가 있는 경우, (예: 개나 고양이의 라벨이 잘못 달린 경우)
- cf. 학습데이터 제작 비용: Image Classification용 학습데이터 < bounding box 정보까지 정확하게 포함해야 하는 Object Detection 또는 Semantic segmentation

- 자율주행 보조장치 object detection 요구사항
  - 1) 사람이 카메라에 감지되면 정지
  - 2) 차량이 일정 크기 이상으로 감지되면 정지

- [KITTI 데이터셋](http://www.cvlibs.net/datasets/kitti/)
  - tensorflow_datasets에서 제공
  - 자율주행을 위한 데이터셋
  - 2D object detection 뿐만 아니라 깊이까지 포함한 3D object detection 라벨 등을 제공
  - 6,347개의 학습 데이터(training data), 711개의 평가용 데이터(test data), 423개의 검증용 데이터(validation data)
  - 라벨에는 alpha, bbox, dimensions, location, occluded, rotation_y, truncated 등의 정보가 있음

```
데이터셋 이해를 위한 예시
Values    Name      Description
----------------------------------------------------------------------------
   1    type         Describes the type of object: 'Car', 'Van', 'Truck',
                     'Pedestrian', 'Person_sitting', 'Cyclist', 'Tram',
                     'Misc' or 'DontCare'
   1    truncated    Float from 0 (non-truncated) to 1 (truncated), where
                     truncated refers to the object leaving image boundaries
   1    occluded     Integer (0,1,2,3) indicating occlusion state:
                     0 = fully visible, 1 = partly occluded
                     2 = largely occluded, 3 = unknown
   1    alpha        Observation angle of object, ranging [-pi..pi]
   4    bbox         2D bounding box of object in the image (0-based index):
                     contains left, top, right, bottom pixel coordinates
   3    dimensions   3D object dimensions: height, width, length (in meters)
   3    location     3D object location x,y,z in camera coordinates (in meters)
   1    rotation_y   Rotation ry around Y-axis in camera coordinates [-pi..pi]
   1    score        Only for results: Float, indicating confidence in
                     detection, needed for p/r curves, higher is better.
```

- 이미지 위에 바운딩 박스 그리기: [Pillow 라이브러리의 ImageDraw 모듈](https://pillow.readthedocs.io/en/stable/reference/ImageDraw.html) 사용

- [RetinaNet](https://arxiv.org/abs/1708.02002)
  - focal loss와 FPN(Feature Pyramid Network) 를 적용한 네트워크를 사용
    - 1-stage detector 모델(YOLO, SSD)의 2-stage detector(Faster-RCNN) 등보다 속도는 빠르지만 성능이 낮은 문제를 해결
    - Focal Loss
      - 물체를 배경보다 더 잘 학습하자 == 물체인 경우 Loss를 작게 만들자
      - 1-stage detection 모델들(YOLO, SSD)이 물체 전경과 배경을 담고 있는 모든 그리드(grid)에 대해 한 번에 학습됨으로 인해서 생기는 클래스 간의 불균형을 해결하고자 도입
        - 픽셀(pixel): 7x7 feature level에서는 한 픽셀
        - 그리드(grid): image level(자동차 사진)에서 보이는 그리드는 각 픽셀의 receptive field
      - modulating factor + 교차 엔트로피
      - 대부분의 이미지에서는 물체보다 배경이 많다. => 배경의 class가 많은 class imbalanced data => 너무 많은 배경 class에 압도되지 않도록 modulating factor로 손실 조절 => γ가 커질수록 modulating이 강하게 적용됨
    - FPN(Feature Pyramid Network)
      - 여러 층의 특성 맵(feature map)을 다 사용해보자
      - 특성을 피라미드처럼 쌓아서 사용하는 방식 => 백본의 여러 레이어를 한꺼번에 쓰겠다. => 넓게 보는 것과 좁게 보는 것을 같이 쓰겠다.
      - receptive field
        - 입력 이미지와 먼 모델의 뒷쪽의 특성 맵일수록 하나의 "셀(cell)"이 넓은 이미지 영역의 정보를 담고 있고, 입력 이미지와 가까운 앞쪽 레이어의 특성 맵일수록 좁은 범위의 정보를 담음
        - 레이어가 깊어질 수록 pooling을 거쳐 넓은 범위의 정보(receptive field)를 가짐
      - SSD: 각 레이어의 특성 맵에서 다양한 크기에 대한 결과를 얻는 방식
      - RetinaNet
        - receptive field가 넓은 뒷쪽의 특성 맵을 upsampling(확대)하여 앞단의 특성 맵과 더해서 사용
        - RetinaNet 에서 FPN 구조<br>![FPN](https://d3s0tskafalll9.cloudfront.net/media/images/GC-3-P-FPN.max-800x600.png)
- One stage detector
  - Anchor Box라는 정해져 있는 위치, 크기, 비율 중에 하나로 물체의 위치가 결정됨
  - Anchor Box가 촘촘하게 겹치도록 생성
  - Anchor Box로 생성되는 것은 물체 위치 후보 => 물체 위치를 주관식이 아닌 객관식(=IoU 사용)으로 풀게 하는 것
    - IoU가 높은지 낮은지에 따라 Anchor Box가 정답(=물체)인지 오답(=배경)인지 체크
  - 여러 개의 Anchor Box 들 중 가장 근접한 하나 선택 -> 선택된 Anchor Box를 기초로 정확한 위치를 찾아냄 => Anchor Box와 실제 Bounding Box의 미세한 차이(=상하좌우 차이, 가로세로 크기 차이) 계산
  - RetinaNet 의 FPN 에서는 각 층마다 Anchor Box가 필요하다.
- Object Detection Label
  - class, box 각각을 추론하는 부분 필요 => 각각의 head 가 별도로 존재한다.
  - FPN을 통해 pyramid layer가 추출되고 나면 그 feature들을 바탕으로 class를 예상하고, box도 예상
- 프로젝트에 사용할 RetinaNet
  - 모델 구조: Backbone(=ResNet50) + FPN + classification용 head + box용 head
  - Box Regression에는 Smooth L1 Loss 사용
  - Classification Loss를 계산하기 위해 Focal Loss 사용
  - Optimizer: SGD
  - 모델 추론 결과 100개 Anchor Box 처리할 때 NMS(Non-Max Suppression) 사용
