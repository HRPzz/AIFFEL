# 5. 너의 속이 궁금해 - Class Activation Map 살펴보기

**모델의 작동 원리를 가늠할 수 있는 CAM, Grad-CAM, ACoL 모델을 공부하고 XAI(Explainable AI)의 기초를 익힌다.**

---

|-|목차|⏲ 180분|
|:---:|---|:---:|
|5-1| 들어가며 | 5분|
|5-2| Explainable AI | 15분|
|5-3| CAM: Class Activation Map | 50분|
|5-4| Grad-CAM | 50분|
|5-5| ACoL: Adversarial Complementary Learning | 45분|
|5-6| 생각해 보기 | 15분|

---

## 실습목표

1. 분류 모델의 활성화 맵을 이해합니다.
2. 다양한 활성화 맵을 구하는 방법을 알아갑니다.
3. 약지도학습(weakly supervised learning)을 이해합니다.

## 학습 내용

1. Explainable AI
2. CAM: Class Activation Map
3. Grad-CAM
4. ACoL: Adversarial Complementary Learning
5. 생각해 보기

---

## 노드 내용

- cf. 그동안 우리가 다룬 딥러닝은 모델의 추론 근거를 알 수 없는 블랙박스(Black Box) 모델
- XAI(Explainable Artificial Intelligence, 설명 가능한 인공지능)
  - 모델과 추론의 신뢰성에 대한 답을 찾는 기법
  - 모델의 성능을 개선할 수 있는 단서로도 유용하게 활용됨
  - 모델이 잘못된 답변을 준다면 어떻게 개선할 수 있을지, 잘 동작한다면 왜 이런 선택을 했는지 알고자 하는 것
- 이미지 분류(Image Classification)
  - 이미지 입력 -> (CNN 으로 구성된 백본 네트워크(backbone network)) -> 이미지 feature map 추출 -> (fully connected layer) -> logit -> (소프트맥스(softmax) 활성화 함수) -> 입력 이미지가 각 클래스에 속할 확률 구함
    - logit = sigmoid 의 역함수
    - softmax = sigmoid 를 K개의 클래스로 일반화한 것
  - 정답의 이유를 모델 내부에서 찾는 방법: 레이어마다 feature map 시각화
- [CAM(Class Activation Map)](https://arxiv.org/abs/1512.04150)
  - "모델이 어떤 곳을 보고 어떤 클래스임을 짐작하고 있는지" 확인할 수 있는 지도 => **클래스가 활성화되는 지도**
  - CNN 레이어를 거친 특성 맵에도 입력값의 위치정보 유지됨 => 특성 맵의 정보를 이미지 검출(detection)이나 세그멘테이션(Segmentation) 등에도 이용
  - 분류 모델의 마지막 부분(=CNN 이후)에서 fully connected layer 대신 **GAP(Global Average Pooling) 사용** -> 어떤 클래스가 어느 영역에 의해서 활성화되었는지 알 수 있음
    - ![pooling](https://d3s0tskafalll9.cloudfront.net/media/images/max_avg_pooling.max-800x600.png)
    - Average pooling: 커널과 겹치는 영역에 대해 Average 값을 취한 것
    - Max pooling: 커널과 겹치는 영역에 대해 Max 값을 취한 것
    - GAP(Global Average Pooling)
      - 매 채널별로, average pooling을 채널의 값 전체에 global하게 적용
      - (6,6,3) -> GAP 수행 -> (1,1,3)  # 결과 벡터의 각 차원 값 = 6x6 크기의 특성 맵을 채널별로 평균을 낸 값
      - GAP -> softmax activation function
        - 마지막 CNN 레이어의 채널 수 == 데이터의 클래스 수 => 각 클래스에 따른 확률을 얻음
        - fully connected layer와 달리 최적화할 파라미터가 존재하지 않으므로 과적합(overfitting)을 방지할 수 있다.
  - 정리: (Input Image) -> CNN -> (Feature Map) -> **GAP -> Softmax Layer(=softmax, biasX, fully connected layer)**
    - GAP: 각 채널별 정보 요약
    - Softmax Layer: 각 채널의 가중합을 구함 => 각 클래스에 대한 개별 채널의 중요도 결정
  - 가장 마지막 CNN 레이어의 결과물만을 시각화
  - 수식<br>![CAM 수식1](https://d3s0tskafalll9.cloudfront.net/media/images/GC-3-L-6.max-800x600.png)<br>![CAM 수식2](https://d3s0tskafalll9.cloudfront.net/media/original_images/GC-3-L-7.png)
- [Grad-CAM](https://arxiv.org/abs/1610.02391)(Gradient CAM)
  - CAM의 모델의 구조가 제한되는 문제(GAP->Fully Connected Layer)를 해결하고, 다양한 모델의 구조를 해석할 수 있는 방법을 제안
  - 장점
    - CNN 기반의 네트워크는 **굳이 모델 구조를 변경할 필요가 없음**
    - 분류 문제 외의 다른 태스크들에 유연하게 대처할 수 있음
  - 구조<br>![Grad-CAM 구조](https://d3s0tskafalll9.cloudfront.net/media/images/GC-3-L-9.max-800x600.png)
    - 정리: Input Image -> CNN -> (Feature Map) -> ... (다양한 레이어 사용 가능)
    - Grad-CAM 이 적용될 수 있는 다양한 컴퓨터 비전 문제
      - Image Classification: 이미지 분류
      - Image Captioning: 이미지에 대한 설명을 만들어내는 태스크
      - VQA(Visual question answering): 어떤 질문과 이미지가 주어졌을 때 이에 대한 답변을 내는 태스크
    - 장점: **복잡한 태스크에 사용되는 모델에서도 사용될 수 있다.**
  - 원하는 클래스에 대해서 관찰하는 레이어로 들어오는 그래디언트를 구할 수 있다면, 해당 클래스를 활성화할 때, 레이어의 특성 맵에서 어떤 채널이 중요하게 작용하는지 알 수 있다.
    - 가중치를 구하기 위해 CAM처럼 별도의 weight 파라미터를 도입할 필요가 없다.
    - 가중치 구하는 수식<br>![Grad-CAM 수식](https://d3s0tskafalll9.cloudfront.net/media/original_images/GC-3-L-10.png)
  - **마지막에 ReLU 사용 => 활성화된 영역을 확인해야 하기 때문에 불필요한 음의 값을 줄여주기 때문**<br>![Grad-CAM 마지막 ReLU](https://d3s0tskafalll9.cloudfront.net/media/original_images/GC-3-L-11.png)
- [ACoL](http://openaccess.thecvf.com/content_cvpr_2018/papers/Zhang_Adversarial_Complementary_Learning_CVPR_2018_paper.pdf)(Adversarial Complementary Learning)
  - 구조<br>![ACoL 구조](https://d3s0tskafalll9.cloudfront.net/media/images/GC-3-L-13.max-800x600.png)
  - 직접적으로 정답 위치 정보를 주지 않아도 간접적인 정보를 활용하여 학습하고 원하는 정보를 얻어낼 수 있도록 모델을 학습하는 방식
  - 물체의 전반적인 영역으로 CAM이 활성화되는 효과
  - CAM 단점 개선
    - CAM 모델이 특정 부위에 집중해 학습(=특징이 주로 나타나는 위치에 중점적으로 활성화)하는 단점 개선: 브랜치를 두 가지로 두어 너무 높은 점수를 지워줌 -> 주변의 특성 반영 => 적대적(Adversial) 학습방법
    - CAM 모델의 feed forward 외 별도의 연산(활성화 맵, 가중치 계산)이 필요한 단점 개선: **커널 사이즈는 1x1, 출력 채널의 개수는 분류하고자 하는 클래스 개수** 를 가진 컨볼루션 레이어를 특성 맵에 사용하고 여기에 GAP를 적용하여 Network in Network에서 본 구조와 유사한 방식을 사용 => 컨볼루션 레이어의 출력값은 활성화 맵이 됨<br>![ACoL 의 1x1 Conv](https://d3s0tskafalll9.cloudfront.net/media/images/GC-3-L-15.max-800x600.png)
- 약지도학습(weakly supervised learning)
  - incomplete supervision : 학습 데이터 중 일부에만 라벨이 달린 경우. 일반적으로 말하는 준지도학습과 같은 경우임. (예: 개와 고양이 분류 학습 시 10,000개의 이미지 중 1,000개만 라벨이 있는 경우)
  - **inexact supervision** : 학습데이터의 라벨이 충분히 정확하게 달려있지 않은 경우. (예: 개나 고양이를 Object Detection 또는 Semantic Segmentation해야 하지만 이미지 내에 정확한 bounding box는 주어져 있지 않고 이미지가 개인지 고양인지 정보만 라벨로 달려있는 경우)
    - 오늘 우리가 다루고자 하는 것
    - Grad-CAM(=약지도학습) 을 통한 Object Detection과 Semantic Segmentation
    - 자율주행 연구에 약지도학습을 활용한 예: [네이버랩스의 이미지기반의 차선변경 알고리즘(SLC)은 무엇을 보면서 판단을 할까?](https://www.naverlabs.com/storyDetail/16)
  - inaccurate supervision : 학습 데이터에 Noise가 있는 경우, (예: 개나 고양이의 라벨이 잘못 달린 경우)
- cf. 학습데이터 제작 비용: Image Classification용 학습데이터 < bounding box 정보까지 정확하게 포함해야 하는 Object Detection 또는 Semantic segmentation
- CAM 활용 프로젝트
  - 황준식님의 [CAM: 2017년 대선주자 얼굴 위치 추적기](https://jsideas.net/class_activation_map/)

---

- 생각해 볼 거리
  - 어떤 분류문제를 풀고 싶은가요?
  - 데이터를 어떻게 모을 수 있을까요?
  - 어떤 모델을 기반으로 사용할까요?
  - Class Activation Map을 활용해서 보여준다면 어떤 점이 좋을까요?
