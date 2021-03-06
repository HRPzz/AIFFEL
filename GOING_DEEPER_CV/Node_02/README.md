# 2. 없다면 어떻게 될까? (ResNet Ablation Study)

**핵심적인 기법들을 하나씩 제거했을 때의 효과를 각각 정량적으로 측정하는 ablation study 기법을 배운다. ResNet을 대상으로 실습해 보면서 이론적으로 익힌 기법의 효과를 체감하고 백본을 직접 다뤄보는 실전적 감각을 익힌다.**

---

[➡ [CV-02] ResNet_vs_Plain.ipynb with nbviewer](https://nbviewer.org/github/HRPzz/AIFFEL/blob/main/GOING_DEEPER_CV/Node_02/%5BCV-02%5D%20ResNet_vs_Plain.ipynb)

---

|-|목차|⏲ 400분|
|:---:|---|:---:|
|2-1| 들어가며 | 5분|
|2-2| Ablation Study | 10분|
|2-3| Ablation Study 실습 (1) CIFAR-10 데이터셋 준비하기 | 25분|
|2-4| Ablation Study 실습 (2) 블록 구성하기 | 30분|
|2-5| Ablation Study 실습 (3) VGG Complete Model | 30분|
|2-6| Ablation Study 실습 (4) VGG-16 vs VGG-19 | 60분|
|2-7| 프로젝트: ResNet Ablation Study | 240분|
|2-8| 프로젝트 제출 |

---

## 실습목표

- 직접 ResNet 구현하기
- 모델을 config에 따라서 변경 가능하도록 만들기
- 직접 실험해서 성능 비교하기

## 학습내용

1. Ablation Study
2. CIFAR-10 데이터셋 준비
3. 블록 구성
4. VGG Complete Model
5. VGG-16 vs VGG-19
6. ResNet Ablation Study

---

## 2-7. 프로젝트: ResNet Ablation Study

0) 라이브러리 버전 확인하기
1) ResNet 기본 블록 구성하기
2) ResNet-34, ResNet-50 Complete Model
3) 일반 네트워크(plain network) 만들기
4) ResNet-50 vs Plain-50 또는 ResNet-34 vs Plain-34

---

>## **루브릭**
>
>|번호|평가문항|상세기준|평가결과|
>|:---:|---|---|:---:|
>|1|ResNet-34, ResNet-50 모델 구현이 정상적으로 진행되었는가?|블록함수 구현이 제대로 진행되었으며 구현한 모델의 summary가 예상된 형태로 출력되었다.|⭐|
>|2|구현한 ResNet 모델을 활용하여 Image Classification 모델 훈련이 가능한가?|cats_vs_dogs 데이터셋으로 학습시 몇 epoch동안 안정적으로 loss 감소가 진행 확인되었다.|⭐|
>|3|Ablation Study 결과가 바른 포맷으로 제출되었는가?|ResNet-34, ResNet-50 각각 plain모델과 residual모델을 동일한 epoch만큼 학습시켰을 때의 validation accuracy 기준으로 Ablation Study 결과표가 작성되었다.|⭐|

---

## 노드 내용

- Ablation Study: "아이디어를 제거해 봄으로써" 제안한 방법이 어떻게 성능이나 문제에 해결에 효과를 주는지 확인하는 실험
- 이번 프로젝트
  - **논문만 보고 스스로 구현해 보는 경험을 통해 딥러닝 개발자로서의 내공과 자신감이 다져지게 될 것**
  - ResNet Ablation Study: residual connection이 없는 일반 네트워크(plain net)와 ResNet을 비교
  - **CIFAR-10에 대해 일반 네트워크와 ResNet을 구현/비교** => ResNet 및 residual connection의 유효성 확인
  - Input Normalization
    - 이미지 데이터 픽셀 정보 255로 나누어서 0~1.0 사이의 값을 가지게 만듦
    - 머신러닝에서 scale이 큰 feature의 영향이 비대해지는 것을 방지
    - 딥러닝에서 Local optimum에 빠질 위험을 줄임(학습 속도 향상)
  - 블록(block) 구현
    - 레이어(layer): 기본적으로 텐서플로우(TensorFlow), 케라스(Keras), 파이토치(PyTorch) 등에서 기본적으로 제공하는 단위
    - 블록(block): 주요 구조를 모듈화 시켜 조금씩 바꾸어 쓸 수 있는 단위
  - ResNet-18, 34, 50, 101, 152 구조<br>![ResNet](https://d3s0tskafalll9.cloudfront.net/media/images/resnet.max-800x600.png)
    - ResNet-34와 ResNet-50 구현하기
      - 공통점: conv block 각각 3, 4, 6, 3개씩 반복해서 쌓은 형태
      - 차이점
        - ResNet-34의 경우 Block은 3x3 kernel인 Convolution layer로만 구성
        - ResNet-50은 1x1 Convolution이 앞뒤로 붙어 더 많은 레이어를 한 블록 내에 가짐
