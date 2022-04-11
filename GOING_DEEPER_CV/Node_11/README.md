# 11. OCR 기술의 개요

**이미지 속 글자를 읽어보는 OCR 기술을 구성하는 Text detection과 Text recognition에 대해 알아본다.**

---

|-|목차|⏲ 180분|
|:---:|---|:---:|
|11-1| 들어가며 | 5분|
|11-2| Before Deep Learning | 15분|
|11-3| Text detection | 60분|
|11-4| Text recognition | 60분|
|11-5| Text recognition + Attention | 40분|

---

## 실습목표

- Deep learning 기반의 OCR을 이해합니다.
- Text를 Detection하기 위한 딥러닝 기법을 배웁니다.
- Text를 Recognize하기 위한 딥러닝 기법을 배웁니다.

---

## 노드 내용

- OCR = Text detection + Text recognition
  - ![OCR 진행 과정](https://d3s0tskafalll9.cloudfront.net/media/images/GC-6-L-00.max-800x600.png)
  - Text detection: 문자의 영역 검출, Object Detection + Segmentation
  - Text recognition: 검출된 영역의 문자 인식
- OCR(Optical Character Recognition)
  - 딥러닝 적용 전 OCR: Tesseract OCR
    - e.g. 브라우저에서 동작하는 OCR을 이용하여 웹에서 유저의 행동을 관찰하는 방법
    - 입력 이미지 추출, 전처리(=이진화, 흑백 변환), OCR 처리(=문자 영역 검출, 라인 또는 단어 단위 추출), OCR 출력 텍스트 후처리(단어 단위 이미지를 text로 변환하기 위해 문자를 하나씩 인식하고 다시 결합)
  - 딥러닝 기반의 OCR
    - 원하는 단위로 문자 검출
    - 한 번에 인식하도록 아키텍처를 단순화 => 빠른 인식
    - 검출과 인식을 동시에 해내는 End-to-End OCR 모델
- Text detection
  - 텍스트 위치 찾기
  - 이미지 내에서 문자를 검출해낼 때엔 검출하기 위한 최소 단위를 정해야 함
    - 문장 또는 단어 단위로 찾아낼 경우, 엄청나게 긴 단어나 문장과 함께 짧은 길이도 찾아낼 수 있어야 함
    - 글자 단위로 인식하면 글자를 다시 맥락에 맞게 묶어주는 과정을 거쳐야 함
  - ![Text detection 기법 정리](https://d3s0tskafalll9.cloudfront.net/media/images/GC-6-L-02.max-800x600.png)
    - Text의 바운딩 박스를 구하는 방식
    - 단어 단위의 탐지
      - Object detection의 Regression 기반의 Detection
      - Anchor를 정의하고 단어의 유무와 Bounding box의 크기를 추정해서 단어를 찾아냄
    - 글자 단위의 방식
      - 글자 영역을 Segmentation하는 방법
  - 여러 접근 방식
    - Regression
      - [TextBoxes](https://arxiv.org/pdf/1611.06779.pdf)
      - 딥러닝 기반의 Detection을 이용하여 단어 단위로 인식
      - 네트워크의 기본 구조: SSD => 빠르게 문자 영역 탐지
        - 긴 단어의 Feature를 활용하기 위해서 1x5로 convolution filter를 정의하여 사용
    - Segmentation
      - [PixelLink](https://arxiv.org/pdf/1801.01315.pdf)
        - Text 영역을 찾아내는 segmentation과 함께, 글자가 어느 방향으로 연결되는지를 같이 학습하여 Text 영역 간의 분리 및 연결을 할 수 있는 정보를 추가적으로 활용
        - 구조<br>![PixelLink 구조](https://d3s0tskafalll9.cloudfront.net/media/images/architecture_pixellink.max-800x600.png)
        - 전체적인 구조는 U-Net과 유사
          - conv 1X1, 2(16) 형태의 레이어가 U-Net 구조로 연결
          - 인접 pixel간 연결 구조가 지속적으로 유지되도록 하는 모델 구조
        - output으로 총 9가지의 정보를 얻음
          - 1개: Text/non-text Prediction을 위한 class segmentation map
            - 해당 영역이 Text인지 Non-text인지 예측값을 의미하는 2개의 커널을 가짐
          - 나머지 8가지: Link Prediction map
            - 글자의 Pixel을 중심으로 인접한 8개의 Pixel에 대한 연결 여부를 의미하는 16개의 커널로 이루어짐
        - 인접한 pixel이 중심 pixel과 단어 단위로 연결된 pixel인지, 아니면 분리된 pixel인지 알 수 있으므로, 문자 영역이 단어 단위로 분리된 Instance segmentation이 가능
        - 배경과 글자인 영역으로 분리 => 글자 영역으로 찾아낸 뒤에 이를 분리해내는 작업이나 연결하는 작업을 더 해서 원하는 최소단위로 만들어줘야 함
      - [CRAFT(Character Region Awareness for Text Detection)](https://arxiv.org/abs/1904.01941)
        - 문자(Character) 단위로 문자의 위치를 찾아낸 뒤, 이를 연결하는 방식
        - 문자의 영역을 boundary로 명확히 구분하지 않고, 가우시안 분포를 따르는 원형의 score map을 만들어서 배치시키는 방법으로 문자의 영역을 학습
        - 단어 단위의 정보만 있는 데이터셋에 대해 단어의 영역에 Inference를 한 후, 얻어진 문자 단위의 위치를 다시 학습에 활용하는 Weakly supervised learning을 활용
      - [PMTD(Pyramid Mask Text Detector)](https://arxiv.org/pdf/1903.11800.pdf)
        - Mask-RCNN의 구조를 활용하여 먼저 Text영역을 Region proposal network로 찾아냄 -> Box head에서 더 정확하게 regression 및 classification -> Mask head에서 Instance의 Segmentation을 하는 과정
        - Mask 정보가 부정확한 경우를 반영하기 위해서 Soft-segmentation 활용
          - 단어의 사각형 배치 특성을 반영하여 피라미드 형태의 Score map 활용 => Pyramid 형상의 Mask를 가짐
- Text recognition
  - Unsegmented data
    - 분리에 드는 비용이 많이 들거나 어려워 Segmentation이 되어있지 않은 데이터
    - annotation이 제대로 안 된 음성데이터
    - segment되어 있지 않은 하위 데이터들끼리 시퀀스(sequence)를 이루고 있다.
  - Unsegmented data 다루기 위한 모델: CRNN
    - CNN(Convolutional neural network)과 RNN(Recurrent neural network)을 같이 쓰는 방법
    - 구조<br>![CRNN 구조](https://d3s0tskafalll9.cloudfront.net/media/original_images/crnn_structure.png)
    - 문자 이미지 -> (Feature Extractor(=CNN)) -> 정보(=feature) 추출 -> (Map-To-Sequence) -> Sequence 형태의 feature로 변환 -> (RNN 으로 Bidirectional LSTM 사용) -> step마다 나오는 결과는 Transcription Layer에서 문자로 변환
    - Fully Connected Layer의 logit을 Softmax 함수에 넣어줌으로써 어떤 문자일 확률이 높은지 알 수 있다.
    - 모델의 Output은 24개의 글자로 이루어진 Sequence
  - [CTC(Connectionist Temporal Classification)](http://www.cs.toronto.edu/~graves/icml_2006.pdf)
    - Unsegmented data와 같이 Input과 Output이 서로 다른 Length의 Sequence를 가질 때, 이를 Align 없이 활용하는 방법
    - Label Encode에서 이렇게 같은 문자를 구분하기 위한 Blank를 중복된 라벨 사이를 구분하기 위해 넣음
    - Decode 후에 중복을 제거하고, 인식할 문자가 아닌 값을 지우면 결과를 얻을 수 있다.
  - 실제 정답과 예측한 단어가 얼마나 가까운지 측정할 수 있는 방법: Edit distance
    - 두 문자열 사이의 유사도를 판별하는 방법
    - 예측된 단어에서 삽입, 삭제, 변경을 통해 얼마나 적은 횟수의 편집으로 정답에 도달할 수 있는지 최소 거리 측정
  - [TPS](https://arxiv.org/pdf/1603.03915.pdf)
    - Thin Plate Spline (TPS) Transformation을 적용하여 입력 이미지를 단어 영역에 맞게 변형 시켜 인식이 잘되도록 함
    - Thin plate spline: control point를 정의하고 해당 point들이 특정 위치로 옮겨졌을 때, 축 방향의 변화를 interpolation하여 모든 위치의 변화를 추정해냄 => 전체 이미지 pixel의 변화를 control point로 만들어낼 수 있다.
    - TPS 연산은 미분 가능한 연산이기 때문에 이 모듈([Spatial Transformer Network](https://papers.nips.cc/paper/2015/file/33ceb07bf4eeb3da587e268d663aba1a-Paper.pdf)를 통해서 Control point가 얼마나 움직여야 하는지 예측하는 네트워크)을 Recognition model 앞단에 붙여서 학습이 바로 가능하다.
      - Spatial Transformer Network: 인풋 이미지에 크기, 위치, 회전 등의 변환을 가해 추론을 더욱 용이하게 하는 transform matrix를 찾아 매핑해 주는 네트워크
  - Attention
    - sequence prediction은 문장의 길이를 고정하고, 입력되는 Feature에 대한 Attention을 기반으로 해당 글자의 Label을 prediction
    - RNN으로 Character label을 뽑아낸다.
    - 첫 번째 글자에서 입력 feature에 대한 Attention을 기반으로 label을 추정하고, 추정된 label을 다시 입력으로 사용하여 다음 글자를 추정해내는 방식
    - 미리 정해둔 Token(start, end, 예외처리, 공백 등) 사용
  - Transformer
    - Irregular text를 잘 인식하기 위해서 2d space에 대한 attention을 활용하여 문자 인식
    - Query, Key, Value라는 개념을 통해서 Self-Attention을 입력으로부터 만들어 냄 => 입력에서 중요한 Feature에 대해 Weight를 줌
    - Attention의 핵심: Decoder의 현재 포지션에서 중요한 Encoder의 State에 가중치가 높게 매겨진다.
