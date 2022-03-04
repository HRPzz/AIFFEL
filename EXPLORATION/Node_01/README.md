# 1. 인공지능과 가위바위보 하기

**손글씨 이미지(MNIST)를 분류하는 간단한 이미지 분류기를 keras를 활용하여 제작해 보고, 이를 응용하여 가위바위보 이미지를 분류해 보는 프로젝트를 진행한다.**

---

|-|목차|⏲ 360분|
|:---:|---|:---:|
|1-1| 인공지능과 가위바위보 하기 | 30분|
|1-2| 데이터를 준비하자! | 30분|
|1-3| 딥러닝 네트워크 설계하기 | 30분|
|1-4| 딥러닝 네트워크 학습시키기 | 30분|
|1-5| 얼마나 잘 만들었는지 확인하기 | 30분|
|1-6| 더 좋은 네트워크 만들어 보기 | 30분|
|1-7| 미니 프로젝트 : 가위바위보 분류기를 만들자 | 180분|
|1-8| 프로젝트 제출|-|

---

- 간단한 인공지능 이미지 분류기 만들기
  - 숫자는 0~9까지 총 10개의 클래스(class)만 인식, Sequential Model 이용
  - 가위바위보는 총 3개의 클래스만 구분
- 라이브러리
  - 텐서플로우(TensorFlow)
    - 표준 API인 tf.keras의 Sequential API를 이용하여 숫자 손글씨 인식기 제작
    - 구글(Google)에서 오픈소스로 제공
    - 가장 널리 사용되고 있는 머신러닝 라이브러리 중 하나
    - Tensorflow 버전 2.4.0에서 진행
  - Matplotlib
    - 파이썬에서 제공하는 시각화(Visualization) 패키지
    - 차트(chart), 플롯(plot) 등 다양한 형태로 데이터를 시각화
- 케라스에서 모델 만드는 방법
  - Sequential API
  - Functional API
  - 밑바닥부터 직접 코딩
- 교차 검증(cross validation): 쉽게 생각하면 본고사를 치르기 전 모의고사를 여러 번 보는 것
- 딥러닝 제작 과정: 데이터 준비 → 딥러닝 네트워크 설계 → 학습 → 테스트(평가)
  - 데이터 준비
    - 데이터 가져오기
      - mnist = keras.datasets.mnist
      - (x_train, y_train), (x_test, y_test) = mnist.load_data()
        - 학습용 데이터(Train set): 입력(x_train), 정답(y_train)
          - 학습O
        - 검증용 데이터(Validation set)
          - 학습X, 학습이 이미 완료된 모델을 검증
          - Train 데이터 중 일부를 validation 데이터로 이용
        - 시험용 데이터(Test set): 입력(x_test), 정답(y_test)
          - 학습X, 학습과 검증이 완료된 모델의 성능을 평가
        - 데이터 비율
          - Train : Test = 8 : 2로 이용
          - Train : Validation : Test = 6 : 2 : 2로 이용
      - 숫자 손글씨 이미지
        - 28 x 28 크기
        - 총 70,000개 = 60,000(Training set) + 10,000(Test set)
    - 데이터 전처리
      - 이미지 실제 픽셀 값: 0~255 사이의 값
      - 인공지능 모델을 훈련시키고 사용할 때, 일반적으로 입력은 0~1 사이의 값으로 정규화 시켜주는 것이 좋음
      - 각 픽셀의 값이 0~255 사이 범위에 있으므로 데이터들을 255.0 으로 나누어줌
  - 딥러닝 네트워크 설계
    - tf.keras의 Sequential API를 이용하여 LeNet이라는 딥러닝 네트워크를 설계
      - Sequential API는 개발의 자유도는 많이 떨어지지만, 매우 간단하게 딥러닝 모델을 만들어낼 수 있는 방법
      - 미리 정의된 딥러닝 레이어(layer)를 손쉽게 추가
      - 각 레이어의 첫 번째 인자 역할
        - Conv2D: 이미지 특징 수 => 입력 이미지가 디테일하고 복잡한 영상이라면 숫자를 늘려야 함
          - 첫 번째 레이어에 input_shape=(28,28,1)로 지정 => 네트워크의 입력은 (데이터갯수, 이미지 크기 x, 이미지 크기 y, 채널수) 와 같은 형태여야 함
            - x_train_reshaped=x_train_norm.reshape(-1, 28, 28, 1)  # 데이터갯수에 -1을 쓰면 reshape시 자동계산됨
            - x_test_reshaped=x_test_norm.reshape(-1, 28, 28, 1)
          - 채널수
            - 흑백 이미지: 1
            - 컬러 이미지: 3 (R,G,B)
        - Dense: 분류기에 사용되는 뉴런 숫자 => 알파벳(소문자 26 + 대문자 26) 분류기라면 64, 128 설정
        - 마지막 Dense: 최종 분류기 class 수 => 숫자(0~9) 분류기라면 10개
    - model.summary()
      - Conv2D -> MaxPooling2D -> Conv2D -> MaxPooling2D -> Flatten -> Dense -> Dense(softmax)
  - 학습
    - x_train_reshaped=x_train_norm.reshape(-1, 28, 28, 1)  # 데이터갯수에 -1을 쓰면 reshape시 자동계산됨
    - x_test_reshaped=x_test_norm.reshape(-1, 28, 28, 1)
    - model.compile(optimizer='adam', loss='sparse_categorical_crossentropy', metrics=['accuracy'])
    - model.fit(x_train_reshaped, y_train, epochs=10)
  - 테스트(평가)
    - 정확도 측정: model.evaluate()
      - test_loss, test_accuracy = model.evaluate(x_test_reshaped,y_test, verbose=2)
    - model 이 추론한 확률분포: model.predict()
      - predicted_result = model.predict(x_test_reshaped)  # model이 추론한 예측확률분포
      - predicted_labels = np.argmax(predicted_result, axis=1)
- 딥러닝 모델 개선 방법
  - 하이퍼 파라미터 변경
    - Conv2D 레이어에서 입력 이미지의 특징 수
    - Dense 레이어에서 뉴런 수
    - 학습 반복 횟수인 epoch 값

---

## 1-7. 미니 프로젝트 : 가위바위보 분류기를 만들자

- 데이터 준비하기
  - 가위, 바위, 보 이미지 각 100장 만들기(이미지 크기 224x224)
  - [구글의 teachable machine 사이트](https://teachablemachine.withgoogle.com/)
- 데이터 불러오기 + Resize 하기
  - 이미지 크기: 28x28
  - 가위바위보의 경우 가위: 0, 바위: 1, 보: 2 로 라벨링
- 딥러닝 네트워크 설계하기
- 딥러닝 네트워크 학습시키기
- 얼마나 잘 만들었는지 확인하기(테스트)
  - test accuracy 측정
- 더 좋은 네트워크 만들어보기
  - test accuracy 가 train accuracy 에 근접하도록 개선 방법 찾기
- 노드를 마치며...
  - 이미 잘 정제된 10개 클래스의 숫자 손글씨 데이터를 분류하는 classifier 만들기
  - 정제되지 않은 웹캠 사진으로부터 데이터 만들어보기
  - 흑백 사진이 아닌 컬러 사진을 학습하는 classifier 만들기
  - 분류하고자 하는 클래스의 개수를 마음대로 조절하기 (10개에서 3개로)

---

>## **루브릭**

>|번호|평가문항|상세기준|평가결과|
>|:---:|---|---|:---:|
>|1|이미지 분류기 모델이 성공적으로 만들어졌는가?|트레이닝이 정상적으로 수행되었음|⭐|
>|2|오버피팅을 극복하기 위한 적절한 시도가 있었는가?|데이터셋의 다양성, 정규화 등의 시도가 적절하였음|⭐|
>|3|분류모델의 test accuracy가 기준 이상 높게 나왔는가?|60% 이상 도달하였음|☆|
