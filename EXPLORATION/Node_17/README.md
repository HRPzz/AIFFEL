# 17. 난 스케치를 할 테니 너는 채색을 하거라

**이미지 생성 모델로 사용되는 GAN 중에서 조건이 추가된 cGAN에 대해 알아보고 Pix2Pix를 배워봅니다.**

---

- [➡ [E-17] Pix2Pix.ipynb with nbviewer](https://nbviewer.org/github/HRPzz/AIFFEL/blob/main/EXPLORATION/Node_17/%5BE-17%5D%20Pix2Pix.ipynb)

---

|-|목차|⏲ 420분|
|:---:|---|:---:|
|17-1| 들어가며 | 5분|
|17-2| 조건 없는 생성모델(Unconditional Generative Model), GAN | 5분|
|17-3| 조건 있는 생성모델(Conditional Generative Model), cGAN | 15분|
|17-4| 내가 원하는 숫자 이미지 만들기 (1) Generator 구성하기 | 20분|
|17-5| 내가 원하는 숫자 이미지 만들기 (2) Discriminator 구성하기 | 20분|
|17-6| 내가 원하는 숫자 이미지 만들기 (3) 학습 및 테스트하기 | 25분|
|17-7| GAN의 입력에 이미지를 넣는다면? Pix2Pix | 20분|
|17-8| 난 스케치를 할 테니 너는 채색을 하거라 (1) 데이터 준비하기 | 20분|
|17-9| 난 스케치를 할 테니 너는 채색을 하거라 (2) Generator 구성하기 | 30분|
|17-10| 난 스케치를 할 테니 너는 채색을 하거라 (3) Generator 재구성하기 | 25분|
|17-11| 난 스케치를 할 테니 너는 채색을 하거라 (4) Discriminator 구성하기 | 25분|
|17-12| 난 스케치를 할 테니 너는 채색을 하거라 (5) 학습 및 테스트하기 | 30분|
|17-13| 프로젝트 : Segmentation map으로 도로 이미지 만들기 | 180분|
|17-14| 프로젝트 제출 |-|

---

## 학습 목표

- 조건을 부여하여 생성 모델을 다루는 방법에 대해 이해합니다.
- cGAN 및 Pix2Pix의 구조와 학습 방법을 이해하고 잘 활용합니다.
- CNN 기반의 모델을 구현하는데 자신감을 갖습니다.

## 학습 전제

- Tensorflow의 Subclassing API로 레이어 및 모델을 구성하는 방법에 대해 대략적으로 알고 있어야 합니다.
- 신경망의 학습 방법에 대한 전반적인 절차를 알고 있어야 합니다.
- CNN, GAN에 대한 기본적인 개념을 알고 있어야 합니다.
- Tensorflow의 GradientTape API를 이용한 학습 코드를 보고 이해할 수 있어야 합니다.
- (중요) Tensorflow 내에서 잘 모르는 함수(또는 메서드)를 발견했을 때, 공식 문서에서 검색하고 이해해 보려는 의지가 필요합니다.

## 목차

1. 조건 없는 생성 모델(Unconditional Generative Model), GAN
2. 조건 있는 생성 모델(Conditional Generative Model), cGAN
3. 내가 원하는 숫자 이미지 만들기 (1) Generator 구성하기
4. 내가 원하는 숫자 이미지 만들기 (2) Discriminator 구성하기
5. 내가 원하는 숫자 이미지 만들기 (3) 학습 및 테스트하기
6. GAN의 입력에 이미지를 넣는다면? Pix2Pix
7. 난 스케치를 할 테니 너는 채색을 하거라 (1) 데이터 준비하기
8. 난 스케치를 할 테니 너는 채색을 하거라 (2) Generator 구성하기
9. 난 스케치를 할 테니 너는 채색을 하거라 (3) Generator 재구성하기
10. 난 스케치를 할 테니 너는 채색을 하거라 (4) Discriminator 구성하기
11. 난 스케치를 할 테니 너는 채색을 하거라 (5) 학습 및 테스트하기
12. 프로젝트 : Segmentation map으로 도로 이미지 만들기

---

## 노드 내용

- GAN
  - 조건 없는 생성모델(Unconditional Generative Model)
  - 기존 노이즈 입력을 이미지로 변환
  - 내가 원하는 종류의 이미지를 바로 생성해 내지 못한다.
  - GAN 구조
    - Generator
    - Discriminator
  - 두 신경망이 minimax game을 통해 서로 경쟁하며 발전
  - GAN의 목적 함수: $min_G max_D V(D,G)= \mathbb{E}_{x\sim p_{data}~(x)}[log D(x)] + \mathbb{E}_{z\sim p_x(z)}[log(1-D(G(z)))]$
    - D는 이 식을 최대화하려 학습
    - G는 이 식을 최소화하려 학습
  - G는 z를 입력받아 생성한 데이터 G(z)를 D가 진짜 데이터라고 예측할 만큼 진짜 같은 가짜 데이터를 만들도록 학습한다.
  - ![GAN](https://d3s0tskafalll9.cloudfront.net/media/images/gan_img.max-800x600.png)
- cGAN(Conditional Generative Adversarial Nets)
  - 조건 있는 생성모델(Conditional Generative Model)
  - 내가 원하는 종류의 이미지를 생성할 수 있도록 고안된 방법
  - cGAN의 목적 함수: $min_G max_D V(D,G)=  E_{x∼p_{data}(x)} [logD(x∣y)]+ \mathbb{E}_{z\sim p_x(z)}[log(1-D(G(z\lvert{y})))]$
    - GAN의 목적 함수와 달라진 점
      - D(x) -> D(x|y)
      - G(x) -> G(x|y)
    - **y**
      - 레이블 정보(=특정 조건을 나타냄)
      - 일반적으로 one-hot 벡터를 입력으로 넣음
  - ![cGAN](https://d3s0tskafalll9.cloudfront.net/media/images/cgan_img.max-800x600.png)
- 이미지 전처리
  - 이미지 픽셀 값 [-1,1] 범위로 변경
  - 레이블 정보를 원-핫 인코딩(one-hot encoding)함
  - label 정보를 사용 안 하면 GAN, 사용하면 cGAN
- GAN 구성
  - GAN Generator
    - Tensorflow2 의 Subclassing 방법 사용 ≒ Pytorch 모델 구성 방법
    - Subclassing 방법은 tensorflow.keras.Model 을 상속받아 클래스 생성
    - **init**() 메서드 안에서 레이어 구성을 정의
    - 구성된 레이어를 call() 메서드에서 사용해 forward propagation을 진행
  - GAN Discriminator
    - 입력: Generator가 생성한 이미지
    - 학습: call() 에서 가장 먼저 layers.Flatten() 적용
    - blocks 리스트에서 for loop 로 레이어를 하나씩 꺼내서 입력 데이터를 통과시킴
    - 마지막 fully-connected 레이어를 통과하면 진짜/가짜 이미지 판별 값 1개 출력
- cGAN 구성
  - fully-connected 레이어를 연속적으로 쌓아 생성
  - cGAN의 입력은 2개(노이즈 및 레이블 정보)
  - cGAN Discriminator
    - [Maxout](https://arxiv.org/pdf/1302.4389.pdf) 레이어 사용
      - 두 레이어 사이를 연결할 때, 여러 개의 fully-connected 레이어를 통과시켜 그 중 가장 큰 값을 가져옴
      - $max(w_1^T x+b_1, w_2^T x+b_2)$
      - units 차원 수를 가진 fully-connected 레이어를 pieces개만큼 만들고 그중 최댓값을 출력
      - Maxout 레이어를 3번만 사용하면 cGAN의 Discriminator를 구성할 수 있음
    - 입력: Generator가 생성한 이미지
    - 학습: call() 에서 가장 먼저 layers.Flatten() 적용
    - 이미지 입력 및 레이블 입력 각각은 Maxout 레이어를 한 번씩 통과한 후 서로 결합되어 Maxout 레이어를 한 번 더 통과
    - 마지막 fully-connected 레이어를 통과하면 진짜/가짜 이미지 판별 값 1개 출력
- loss function: Binary Cross Entropy
- optimizer: Adam

```python
from tensorflow.keras import optimizers, losses

bce = losses.BinaryCrossentropy(from_logits=True)

def generator_loss(fake_output):
    return bce(tf.ones_like(fake_output), fake_output)

def discriminator_loss(real_output, fake_output):
    return bce(tf.ones_like(real_output), real_output) + bce(tf.zeros_like(fake_output), fake_output)

gene_opt = optimizers.Adam(1e-4)
disc_opt = optimizers.Adam(1e-4)
```

- [Pix2Pix](https://arxiv.org/pdf/1611.07004.pdf)
  - L1손실과 GAN 손실을 같이 사용하면 더욱더 좋은 결과를 얻을 수 있다. => L1+cGAN
  - 이미지를 입력으로 하여 원하는 다른 형태의 이미지로 변환시킬 수 있는 GAN 모델
    - Conditional Adversarial Networks => cGAN
    - Image-to-Image Translation => 한 이미지의 픽셀에서 다른 이미지의 픽셀로(pixel to pixel) 변환한다.
  - 이미지 변환이 목적인 Pix2Pix는 이미지를 다루는데 효율적인 convolution 레이어를 활용
  - Pix2Pix Generator
    - ![Encoder-Decoder vs U-Net](https://d3s0tskafalll9.cloudfront.net/media/images/p2p_result_g2.max-800x600.png)
    - Encoder-Decoder
      - Encoder: 입력 이미지(x)를 받으면 단계적으로 이미지를 down-sampling 하면서 입력 이미지의 중요한 representation을 학습
      - Decoder: 다시 이미지를 up-sampling하여 입력 이미지와 동일한 크기의 변환된 이미지(y)를 생성
      - Encoder 최종 출력 => bottelneck => 입력 이미지(x)의 가장 중요한 특징만 담고 있음
    - [U-Net](https://medium.com/@msmapark2/u-net-%EB%85%BC%EB%AC%B8-%EB%A6%AC%EB%B7%B0-u-net-convolutional-networks-for-biomedical-image-segmentation-456d6901b28a)
      - 단순한 Encoder-Decoder 구조에 비해 Encoder와 Decoder 사이를 **skip connection** 으로 연결한 U-Net 구조를 사용한 결과가 훨씬 더 실제 이미지에 가까운 품질을 보임
      - bottelneck: 변환 이미지(y)를 생성하는데 충분한 정보를 제공하지 못함 => 보완하기 위해 U-Net 사용
      - 각 레이어마다 Encoder와 Decoder가 연결(skip connection)되어 있다.
      - 단순한 Encoder-Decoder 구조의 Generator를 사용한 결과에 비해 비교적 선명한 결과를 얻음
      - U-Net Generator에서 사용한 skip-connection으로 인해 Decoder의 각 블록에서 입력받는 채널 수가 늘어났고, 이에 따라 블록 내 convolution 레이어에서 사용하는 필터 크기가 커지면서 학습해야 할 파라미터가 늘어남
  - Pix2Pix Discriminator
    - cf. DCGAN Discriminator
      - ![DCGAN Discriminator](https://d3s0tskafalll9.cloudfront.net/media/original_images/dcgan_d.png)
      - 생성된 가짜 이미지 혹은 진짜 이미지를 하나씩 입력받아 convolution 레이어를 이용해 점점 크기를 줄여나가면서, 최종적으로 하나의 이미지에 대해 하나의 확률 값을 출력
    - ![Pix2Pix Discriminator](https://d3s0tskafalll9.cloudfront.net/media/images/patchgan.max-800x600.png)
    - 하나의 이미지가 Discriminator의 입력으로 들어오면, convolution 레이어를 거쳐 확률 값을 나타내는 최종 결과를 생성하는데, 그 결과는 하나의 값이 아닌 여러 개의 값을 가짐
    - 전체 영역을 다 보는 것이 아닌 일부 영역(파란색 점선)에 대해서만 진짜/가짜를 판별하는 하나의 확률 값을 도출
    - 서로 다른 영역에 대해 진짜/가짜를 나타내는 여러 개의 확률 값을 계산할 수 있으며 이 값을 평균하여 최종 Discriminator의 출력을 생성 => 이미지의 일부 영역(patch)을 이용한다고 하여 PatchGAN 이라고 불림
    - 거리가 먼 두 픽셀은 서로 연관성이 거의 없음 => 일부 영역에 대해서 세부적으로 진짜/가짜를 판별
- Pix2Pix 구현
  - 데이터셋 이미지: 스케치, 실제 이미지가 포함되어 있음 => 모델 학습에 사용할 데이터를 2개 이미지로 분할해서 사용
  - 첫 번째 스케치를 다음 단계에서 구성할 Pix2Pix 모델에 입력하여 두 번째 그림과 같은 채색된 이미지를 생성하는 것이 이번 단계의 목표
  - 학습에 사용하는 데이터의 다양성을 높이기 위해 여러 augmentation 방법을 적용 => 좋은 일반화 결과 기대
  - Generator 구성요소: (256,256,3) 입력 -> (1,1,512)로 변환 -> (256,256,3)의 결과
    - Encoder-Decoder
      - ![Pix2Pix Generator](https://d3s0tskafalll9.cloudfront.net/media/images/refer_g.max-800x600.png)
        - ENCODE: (width, height) 크기가 점점 절반씩 줄어들며 최종적으로 (1,1)이 되고, 채널의 수는 512까지 늘어남 => (1,1,512)
        - DECODE:  (width, height) 크기가 점점 두 배로 늘어나 다시 (256, 256) 크기가 되고, 채널의 수는 점점 줄어들어 처음 입력과 같이 3채널이 됨 => (256,256,3)
      - "Convolution → BatchNorm → LeakyReLU"의 3개 레이어로 구성된 기본적인 블록을 아래와 같이 하나의 레이어로 생성
        - Convolution 레이어에서 필터의 크기(=4) 및 stride(=2)
        - LeakyReLU 활성화의 slope coefficient(=0.2)
      - 블록을 여러 번 반복하여 Encoder, Decoder 생성
      - Encoder와 Decoder를 연결해 Generator를 구성
    - U-Net Generator
      - **init**() 메서드에서 Encoder 및 Decoder에서 사용할 모든 블록들을 정의해 놓고, call()에서 forward propagation 하도록 구현
      - Encoder와 Decoder 사이의 skip connection을 위해 features 라는 리스트 생성하고 Encoder 내에서 사용된 각 블록들의 출력을 담음
      - Encoder의 최종 출력이 Decoder의 입력으로 들어가면서 다시 한번 각각의 Decoder 블록들을 통과
        - features 리스트에 있는 각각의 출력들이 Decoder 블록 연산 후 함께 연결되어 다음 블록의 입력으로 사용
  - Discriminator 구성요소: 두 개의 (256,256,3) 크기 입력 -> 70x70 [PatchGAN](https://medium.com/@sahiltinky94/understanding-patchgan-9f3c8380c207) 사용했으므로 최종 출력 크기 (30,30,1)
    - ![Pix2Pix Discriminator](https://d3s0tskafalll9.cloudfront.net/media/images/refer_d.max-800x600.png)
      - 2개 입력(위 그림의 "in", "unknown")을 받아 연결(CONCAT)한 후, ENCODE 라고 쓰인 5개의 블록을 통과
- 손실 함수에 따른 결과의 차이

![results](https://d3s0tskafalll9.cloudfront.net/media/images/p2p_result_loss2.max-800x600.png)

---

## 17-13. 프로젝트 : Segmentation map으로 도로 이미지 만들기

### 프로젝트 수행

1. 데이터에 한 가지 이상의 **augmentation 방법을 적용** 하여 학습해 주세요. (어떠한 방법을 사용했는지 적어주세요.)
2. 이전에 구현했던 두 개의 Generator 중 Encoder와 Decoder간에 skip connection이 있는 **U-Net Generator를 사용** 해 주세요.
3. 모델 학습 후, 학습된 Generator를 이용해 테스트합니다. 테스트 데이터는 다운로드했던 "val" 폴더 내 이미지를 사용해 주세요.
4. 1개 이상의 이미지에 대해 테스트 과정을 거친 후 그 결과를 **스케치, 생성된 사진, 실제 사진 순서로 나란히 시각화** 해 주세요.
5. 모델을 충분히 학습하기에 시간이 부족할 수 있습니다. 적어도 10 epoch 이상 학습하며 **중간 손실 값에 대한 로그** 를 남겨주세요. 좋은 결과를 얻기 위해선 긴 학습 시간이 필요하므로 테스트 결과는 만족스럽지 않아도 괜찮습니다.

---

>## **루브릭**
>
>|번호|평가문항|상세기준|평가결과|
>|:---:|---|---|:---:|
>|1|pix2pix 모델 학습을 위해 필요한 데이터셋을 적절히 구축하였다.|데이터 분석 과정 및 한 가지 이상의 augmentation을 포함한 데이터셋 구축 과정이 체계적으로 제시되었다.|⭐|
>|2|pix2pix 모델을 구현하여 성공적으로 학습 과정을 진행하였다.|U-Net generator, discriminator 모델 구현이 완료되어 train_step이 안정적으로 진행됨을 확인하였다.|⭐|
>|3|학습 과정 및 테스트에 대한 시각화 결과를 제출하였다.|10 epoch 이상의 학습을 진행한 후 최종 테스트 결과에서 진행한 epoch 수에 걸맞은 정도의 품질을 확인하였다.|⭐|
