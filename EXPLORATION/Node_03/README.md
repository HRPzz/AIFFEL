# 3. 카메라 스티커앱 만들기 첫걸음

**face detection 기술, 이미지 처리기법 등 Computer Vision 분야의 실용적인 기술 활용법을 알아보고, SNOW 같은 재밌는 얼굴 인식 스티커 앱을 만들어봅시다.**

---

[➡ [E-03] Camera_Sticker_App.ipynb with nbviewer](https://nbviewer.org/github/HRPzz/AIFFEL/blob/main/EXPLORATION/Node_03/%5BE-03%5D%20Camera_Sticker_App.ipynb)

---

|-|목차|⏲ 320분|
|:---:|---|:---:|
|3-1| 카메라 스티커앱 만들기 첫걸음 | 5분|
|3-2| 어떻게 만들까? 사진 준비하기 | 30분|
|3-3| 얼굴 검출 face detection | 30분|
|3-4| 얼굴 랜드마크 face landmark | 30분|
|3-5| 스티커 적용하기 | 45분|
|3-6| 프로젝트: 고양이 수염 스티커 만들기 | 180분|
|3-7| 프로젝트 제출|-|

---

## 학습 목표

- 얼굴인식 카메라의 흐름 이해
- dlib 라이브러리 사용
- 이미지 배열의 인덱싱 예외 처리

## 준비물

![crown.png](https://d3s0tskafalll9.cloudfront.net/media/original_images/E-8-3.png)

- png(Portable Network Graphics) 파일
  - 무손실 압축 => 고품질 이미지
  - 배경이 투명해서 배경 이미지 위에 png 파일을 얹어 두 이미지를 자연스럽게 합성시킬 수 있다.
  - jpeg 등과 같은 다른 이미지 파일을 사용하면 배경을 지우는 등의 추가 처리가 필요

---

## 3-6. 프로젝트: 고양이 수염 스티커 만들기

데이터에 어떤 정보가 담겨있는지, feature는 무엇이고 label은 무엇인지 확인

- Step 1. 스티커 구하기 or 만들기
  - [![cat-whiskers.png](https://cdn-icons-png.flaticon.com/512/24/24674.png)](https://www.flaticon.com/free-icon/cat-whiskers_24674?term=cat%20nose&page=1&position=1)
  - (1) 고양이 수염 이미지를 다운로드
  - (2) 셀카 이미지도 촬영
- Step 2. 얼굴 검출 & 랜드마크 검출 하기
  - 라이브러리 dlib 사용
  - 얼굴의 bounding box 위치와 landmark의 위치 찾기
- Step 3. 스티커 적용 위치 확인하기
  - ![landmark.png](https://d3s0tskafalll9.cloudfront.net/media/original_images/E-8-8.png)
- Step 4. 스티커 적용하기
  - (1) np.where 를 사용해서 스티커 적용
  - (2) 스티커 뒤로 원본 이미지가 같이 보이게 만들기: opencv 의 cv2.addWeighted() 사용
- Step 5. 문제점 찾아보기
  - (1) 셀프 카메라를 다양한 각도에서 촬영하면서 스티커를 반복해서 적용
  - (2) 문제점이 무엇인지 기록
    - 얼굴 각도에 따라 스티커가 어떻게 변해야할까요?
    - 멀리서 촬영하면 왜 안될까요? 옆으로 누워서 촬영하면 왜 안될까요?
    - 실행 속도가 중요할까요?
    - 스티커앱을 만들 때 정확도가 얼마나 중요할까요?

---

>## **루브릭**
>
>|번호|평가문항|상세기준|평가결과|
>|:---:|---|---|:---:|
>|1|자기만의 카메라앱 기능 구현을 완수하였다.|원본에 스티커 사진이 정상적으로 합성되었다.|⭐|
>|2|스티커 이미지를 정확한 원본 위치에 반영하였다.|정확한 좌표계산을 통해 고양이 수염의 위치가 원본 얼굴에 잘 어울리게 출력되었다.|⭐|
>|3|카메라 스티커앱을 다양한 원본이미지에 적용했을 때의 문제점을 체계적으로 분석하였다.|얼굴각도, 이미지 밝기, 촬영거리 등 다양한 변수에 따른 영향도를 보고서에 체계적으로 분석하였다.|⭐|

---

## 노드 내용

- **일단 한번 만들어 보자!**
  - 카메라앱 만들기 => 동영상 처리, 검출, 키포인트 추정, 추적, 카메라 원근의 기술 다루기
  - 각도 변화가 가능하고 거리 변화에 강건한 스티커 제작
- 라이브러리
  - **opencv**
    - 이미지 처리
    - 이미지 채널 BGR(파랑, 녹색, 빨강) 순으로 사용
    - import cv2
  - **matplotlib**
    - 주피터 노트북에 이미지 출력
    - 이미지 채널 RGB(빨강, 녹색, 파랑) 순으로 사용
    - import matplotlib.pyplot as plt
  - **dlib**
    - Object Detection
    - 이미지 채널 RGB(빨강, 녹색, 파랑) 순으로 사용
    - import dlib
- 카메라 스티커 앱 만들기 과정
  - 얼굴이 포함된 사진 준비
    - img_bgr = cv2.imread(my_image_path)
    - img_rgb = cv2.cvtColor(img_bgr, cv2.COLOR_BGR2RGB)
  - **사진으로부터 얼굴 bounding box 찾은 다음에 face landmark 찾기**
    - Face Detection (얼굴 위치 찾기)
      - dlib의 face detector 사용
        - HOG(Histogram of Oriented Gradients)
          - 이미지에서 색상의 변화량을 나타낸 것
          - 딥러닝이 나오기 이전에 다양하게 사용되던 방식
        - SVM(Support Vector Machine)
          - 선형 분류기
          - 한 이미지를 다차원 공간의 한 벡터로 봄
          - 여러 벡터를 잘 구분짓는 방법
          - 이미지가 HOG를 통해 벡터로 만들어진다면 SVM이 잘 작동
      - sliding window
        - 작은 영역(window)을 이동해가며 확인하는 방법
        - 큰 이미지의 작은 영역을 잘라 얼굴이 있는지 확인하고, 다시 작은 영역을 옆으로 옮겨 얼굴이 있는지 확인
        - 이미지가 크면 클수록 오래걸리는 단점 => 딥러닝 필요
    - Bounding Box 찾기
      - dlib을 활용해 hog detector를 선언: detector_hog = dlib.get_frontal_face_detector()  # 
      - 얼굴의 bounding box를 추출: dlib_rects = detector_hog(img_rgb, 1)   # (image, num of image pyramid)
        - image pyramid
          - 이미지를 upsampling 방법을 통해 크기를 키우는 것
            - upsampling: 데이터의 크기를 키우는 것
          - 이미지 피라미드에서 얼굴을 다시 검출하면 작게 촬영된 얼굴을 크게 볼 수 있기 때문에 더 정확한 검출이 가능
        - dlib detector 는 dlib.rectangles 타입의 객체를 반환
          - dlib.rectangles 는 dlib.rectangle 객체의 배열 형태
          - left(), top(), right(), bottom(), height(), width() 등의 멤버 함수를 포함
    - Face Landmark 찾기
      - 이목구비의 위치를 추론 => face landmark localization 기술
      - face landmark는 detection 의 결과물인 bounding box 로 잘라낸(crop) 얼굴 이미지를 이용
      - Object keypoint estimation 알고리즘
        - Face landmark와 같이 객체 내부의 점을 찾는 기술
        - keypoint를 찾는 알고리즘
          - top-down : bounding box를 찾고 box 내부의 keypoint를 예측 (예제에서 사용할 방식)
          - bottom-up : 이미지 전체의 keypoint를 먼저 찾고 point 관계를 이용해 군집화 해서 box 생성
      - Dlib landmark localization
        - 잘라진 얼굴 이미지에서 아래 68개의 이목구비 위치 찾기
          - 점의 개수는 데이터셋과 논문마다 다름
        - Dlib의 제공되는 모델을 사용
          - landmark_predictor = dlib.shape_predictor(model_path)
          - points = landmark_predictor(img_rgb, dlib_rect)
  - **찾아진 영역으로 머리에 왕관 스티커 붙이기**
    - 스티커 위치
      - x
        - $x = x_{nose}$
        - x = landmark[30][0]
        - refined_x = x - w // 2  # 이미지 시작점은 top-left 좌표라서 좌표 조정
        - if refined_x < 0: img_sticker = img_sticker[:, -refined_x:]; refined_x = 0  # 스티커의 시작점이 얼굴 사진의 영역을 벗어나면 음수로 표현되므로 음수 예외 처리 필요
      - y
        - $y = y_{nose} - \frac {height}{2}$
        - y = landmark[30][1] - dlib_rect.height()//2
        - refined_y = y - h  # 이미지 시작점은 top-left 좌표라서 좌표 조정
        - if refined_y < 0: img_sticker = img_sticker[-refined_y:, :]; refined_y = 0  # 스티커의 시작점이 얼굴 사진의 영역을 벗어나면 음수로 표현되므로 음수 예외 처리 필요
    - 스티커 크기
      - $width = height = width_{bbox}$
      - w = h = dlib_rect.width()
      - img_sticker = cv2.resize(img_sticker, (w,h))
