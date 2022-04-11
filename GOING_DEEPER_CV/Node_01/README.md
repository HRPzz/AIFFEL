# 1. 백본 네트워크 구조 상세분석

**컴퓨터 비전 분야에 실전적으로 사용되는 주요 백본 네트워크(VGG, ResNet, SENet, EfficientNet) 구조에 대해 논문에 정리된 이론을 토대로 심화 원리를 학습한다.**

---

|-|목차|⏲ 260분|
|:---:|---|:---:|
|1-1| 들어가며 | 5분|
|1-2| 딥러닝 논문의 구조 | 15분|
|1-3| ResNet의 핵심개념과 그 효과 | 40분|
|1-4| ResNet 이후 시도 (1) Connection을 촘촘히 | 40분|
|1-5| ResNet 이후 시도 (2) 어떤 특성이 중요할까? | 40분|
|1-6| 모델 최적화하기 (1) Neural Architecture Search | 30분|
|1-7| 모델 최적화하기 (2) EfficientNet | 30분|
|1-8| 직접 찾아보기 | 60분|

---

## 학습목표

- 딥러닝 논문의 구조에 익숙해지기
- 네트워크를 구조화하는 여러 방법의 발전 양상을 알아보기
- 새로운 논문 찾아보기

## 학습내용

1. 딥러닝 논문의 구조
2. ResNet의 핵심 개념과 그 효과
3. ResNet 이후 시도 (1) Connection을 촘촘히
4. ResNet 이후 시도 (2) 어떤 특성이 중요할까?
5. 모델 최적화하기 (1) Neural Architecture Search
6. 모델 최적화하기 (2) EfficientNet
7. 직접 찾아보기

---

## 노드 내용

- 논문
  - 논리 구조
    - 1. 이전까지의 연구가 해결하지 못했던 문제의식
    - 2. 이 문제를 해결하기 위한 그동안의 다른 시도들
    - 3. 이 문제에 대한 이 논문만의 독창적인 시도
    - 4. 그러한 시도가 가져온 차별화된 성과
  - 형식적 구조<br>![paper structure](https://d3s0tskafalll9.cloudfront.net/media/images/GC-1-L-4.max-800x600.png)
    - 초록(abstract): 아이디어를 제안하는 방식과 학계에 이 논문이 기여하는 점 요약
    - 논문 내용
      - 서론(introduction): 1) 내용 - 문제의식, 솔루션 개요
      - 관련 연구(related work): 2) 내용 - 솔루션, 다른 시도
      - 이론 설명: 3) 내용 - 제안ㅇ 방법의 구체적 내용과 구현 방법
      - 실험(experiments): 3) 내용 - 비교실험, 결과를 통한 검증
      - 결론(conclusion): 4) 내용
    - 참고문헌(reference): 논문의 설명과 구현에 있어 인용한 논문들의 리스트 소개
    - 부록(appendix):  미처 본문에서 설명하지 못한 구현 또는 추가적인 실험 설명 포함
  - 논문1 -> 한계점(문제의식) -> 새로운 방법으로 해결(논문2) -> ... (반복)
    - 논문 이해 + 논문의 역사적 의의, 처음 제시했던 독창적인 아이디어가 무엇인가? 파악하는 것이 중요함!
- [ResNet](https://arxiv.org/abs/1512.03385)
  - 서론(Introduction)
    - 문제: Degradation Problem
      - 딥러닝 모델의 레이어가 깊어졌을 때 모델의 training/test error가 더 커지는 현상
      - 레이어를 깊이 쌓았을 때 최적화가 잘 안되기 때문에 발생하는 문제
  - 솔루션: **Residual Block**
    - Residual Block = shortcut connection을 가진 ResNet의 기본 블록
      - 블록(block): 레이어를 묶은 모듈
    - Shortcut connection: 앞에서 입력으로 들어온 값을 네트워크의 출력층에 곧바로 더해줌 => 출력값에서 원본 입력을 제외한 잔차(residual) 함수를 학습<br>![residual function](https://d3s0tskafalll9.cloudfront.net/media/original_images/GC-1-L-2.png)
    - 학습해야 할 Residual 레이어: F(x) = H(x) - x
      - 출력값: H(x) = F(x) + x
        - x: 입력값
        - F(x) 가  Vanishing Gradient현상으로전혀 학습이 안되어 zero mapping이 될지라도, 최종 H(x)는 최소한 identity mapping이라도 될 테니 성능 저하는 발생하지 않게 된다.
  - 검증
    - ResNet vs PlainNet<br>![ResNet vs PlainNet Top-1 error](https://d3s0tskafalll9.cloudfront.net/media/original_images/GC-1-L-6.png)
      - ResNet: shortcut connection을 사용한 네트워크
      - PlainNet: shortcut connection이 없는 네트워크
- cf. Vanishing/Exploding Gradient 문제의 해결책: normalized initialization, intermediate normalization layers

- State of the art(SOTA)
  - 벤치마크 데이터셋: 어떤 태스크(task)의 모델 간 성능을 비교할 때 널리 사용되어 기준이 되는 데이터셋
  - 이를 통해서 최첨단 성능, 즉 State of the art(SOTA)을 기록

- ResNet 이후
  - [DenseNet](https://arxiv.org/abs/1608.06993)
    - ResNet의 shortcut connection을 마치 Fully Connected Layer처럼 촘촘히 가지도록 한다면 더욱 성능 개선 효과가 클 것이라고 생각하고 이를 실험으로 입증
    - 구조: dense connectivity<br>![DenseNet](https://d3s0tskafalll9.cloudfront.net/media/images/GC-1-L-7.max-800x600.png)![DenseNet 구조](https://d3s0tskafalll9.cloudfront.net/media/images/gc-1v2-l-3-1.max-800x600.png)
      - Dense Block은 L 개의 레이어가 있을 때 레이어 간 L(L+1)/2 개의 직접적인 연결(direct connection) 생성 => 각 레이어는 이전 레이어들에서 나온 특성 맵(feature map) 전부를 입력값으로 받음
      - 장점
        - 경사 소실 문제(gradient vanishing) 개선
        - 특성 계속 재사용 가능
      - Short Connection
        - ![DenseNet 합성함수](https://d3s0tskafalll9.cloudfront.net/media/original_images/GC-1-L-preactivation.png)
        - DenseNet은 하나하나를 차원으로(=채널 방향으로) 쌓아서(concatenate) 하나의 텐서로 만들어 냄 => 진행할수록 특성 맵의 크기가 매우 커지는 문제 방지하기 위해 growth rate 를 조정해서 채널 개수를 조절함
        - 배치 정규화(batch normalization, BN), ReLU 활성화 함수, 그리고 3x3 컨볼루션 레이어를 통해서 pre-activation을 수행
        - pre-activation == ReLU가 컨볼루션 블록 안으로 들어간 것
  - [SENet](https://arxiv.org/abs/1709.01507)
    - Squeeze-and-Excitation Networks의 줄임말
    - 채널 방향으로 global average pooling을 적용, 압축된 정보를 활용하여 중요한 채널이 활성화되도록 함 => CNN에서 나온 특성 맵의 채널에 어텐션(attention) 매커니즘을 적용한 블록을 만들어냄
    - 2017 ILSVRC 분류 문제(classification task)에서 1등
    - Squeeze
      - 특성에서 중요한 정보를 짜내는 과정
      - pooling: 커널(kernel) 영역의 정보(=채널별 정보)를 압축
        - 커널 영역에 대해 최댓값만 남기는 것이 Max Pooling
        - 평균값을 남기는 것이 Average Pooling
    - Excitate
      - 채널을 강조하는 것
      - GAP 적용 특성(=squeeze 결과물)과 W1 곱하는 linear layer를 거치고 ReLU 거치고 W2 곱하는 linear layer를 거치고 Sigmoid 거침
      - Sigmoid 사용 이유
        - 여러 채널들이 서로 다른 정도로 활성화되도록 하기 위함
        - 하나의 대상에도 여러 개의 클래스의 정답 라벨을 지정할 수 있는 다중 라벨 분류(multi label classification)
      - 계산된 벡터를 기존의 특성 맵에 채널에 따라서 곱해주어 중요한 채널이 활성화 되도록 만들어 줌

- 모델 최적화
  - [NASNet](https://arxiv.org/abs/1707.07012)
    - NAS에 강화학습을 적용하여 500개의 GPU로 최적화한 CNN 모델들
      - 아키텍쳐 탐색(architecture search): 모델 구조 최적화를 위한 여러 가지 네트워크 구조 탐색
      - NAS(neural architecture search): 그중 신경망을 사용해 모델의 구조를 탐색하는 접근 방법
    - 딥러닝에서 모델을 탐색하기 위해 강화학습 모델이 대상 신경망의 구성(하이퍼파라미터)을 조정하면서 최적의 성능을 내도록 하는 방법이 제안되었으며, NASNet은 그중 하나
      - 신경망의 구성(하이퍼파라미터)을 조정: 레이어의 세부 구성, CNN의 필터 크기, 채널의 개수, connection 등
      - 탐색 공간(search space): 네트워크 구성에 대한 요소들을 조합할 수 있는 범위
      - 탐색 공간을 줄이기 위해서 모듈(cell) 단위의 최적화를 하고 그 모듈을 조합하는 방식을 채택 => 논문의 모델은 normal cell과 reduction cell 내부만을 최적화
      - Convolution Cell
        - normal cell: 특성 맵의 가로, 세로가 유지되도록 stride를 1로 고정
        - reduction cell:  stride를 1 또는 2로 가져가서 특성 맵의 크기가 줄어들 수 있도록 함
  - [EfficientNet](https://arxiv.org/abs/1905.11946)
    - 작고 효율적인 네트워크 사용
    - 이미지에 주로 사용하는 CNN을 효율적으로 사용할 수 있도록 네트워크의 형태를 조정할 수 있는 width, depth, resolution 세 가지 요소(Scaling Factor)에 집중 => resolution, depth, width를 최적으로 조정하기 위해서 앞선 NAS와 유사한 방법을 사용해 기본 모델(baseline network)의 구조를 미리 찾고 고정, 개별 레이어의 resolution, depth, width 를 조절해 기본 모델을 적절히 확장시킴
      - width: CNN 채널 -> 채널을 늘려줄수록 CNN의 파라미터와 특성을 표현하는 차원의 크기를 키울 수 있다.
      - depth: 네트워크의 깊이
      - resolution: 입력값의 너비(w), 높이(h) 크기 -> 입력이 클수록 정보가 많아져 성능이 올라갈 여지가 생기지만 레이어 사이의 특성 맵이 커지는 단점
    - Compound Scaling
      - 세 가지 요소(Scaling Factor)를 동시에 고려함
      - resolution, depth, width를 각각 조정하는 것이 아니라 고정된 계수 ϕ(=compound coefficient)에 따라서 변하도록 하면 보다 일정한 규칙에 따라(in a principled way) 모델의 구조가 조절됨

- CV 주요 Task
  - 분류(classification task)
  - 물체 검출(detection)
  - 물체의 영역을 분리해내는 세그멘테이션(segmentation)
- [9 Applications of Deep Learning for Computer Vision](https://machinelearningmastery.com/applications-of-deep-learning-for-computer-vision/)
  - Image Classification
  - Image Classification With Localization
  - Object Detection
  - Object Segmentation
  - Image Style Transfer
  - Image Colorization
  - Image Reconstruction
  - Image Super-Resolution
  - Image Synthesis
  - Other Problems

---

## ResNet 요약

- 해결하고자 하는 것: 모델의 레이어가 깊어지더라도 성능이 증가하지 않고 학습이 되지않는 Gradient descent 문제를 해결하고자 합니다.
- 적용한 방법: 블록의 Input을 skip connection을 통해 output에 더해줌으로써 정보가 소실되지 않고 깊은 레이어까지 전달되도록 합니다.
