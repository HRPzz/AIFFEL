# 14. 멀리 있는 사람도 스티커를 붙여주자

**이미지에 사람 얼굴이 다수 포함된 경우에도 빠르게 이를 인식할 수 있는 SSD 모델을 구현, 학습해 보고 이를 이용해 카메라 스티커 앱을 개선해 본다.**

---

[➡ [CV-14] Camera_Sticker_App_with_SSD.ipynb](https://nbviewer.org/github/HRPzz/AIFFEL/blob/main/GOING_DEEPER_CV/Node_14/%5BCV-14%5D%20Camera_Sticker_App_with_SSD.ipynb)

---

|-|목차|⏲ 450분|
|:---:|---|:---:|
|14-1| 들어가며 | 20분|
|14-2| 데이터셋 전처리(1) 분석 | 20분|
|14-3| 데이터셋 전처리(2) TFRecord 생성 | 30분|
|14-4| 모델 구현(1) Default boxes | 30분|
|14-5| 모델 구현(2) SSD | 30분|
|14-6| 모델 학습(1) Augmentation, jaccard 적용 | 30분|
|14-7| 모델 학습(2) train | 60분|
|14-8| inference(1) NMS | 20분|
|14-9| inference(2) 사진에서 얼굴 찾기 | 30분|
|14-10| 프로젝트 : 스티커를 붙여주자 | 180분|
|14-11| 프로젝트 제출 |-|

---

## 14-10. 프로젝트 : 스티커를 붙여주자

- Step 1. 스티커 구하기 혹은 만들기
- Step 2. SSD 모델을 통해 얼굴 bounding box 찾기
- Step 3. dlib 을 이용한 landmark 찾기 (선택사항)
- Step 4. 스티커 합성 사진 생성하기

---

>## **루브릭**
>
>|번호|평가문항|상세기준|평가결과|
>|:---:|---|---|:---:|
>|1|multiface detection을 위한 widerface 데이터셋의 전처리가 적절히 진행되었다.|tfrecord 생성, augmentation, prior box 생성 등의 과정이 정상적으로 진행되었다.|⭐|
>|2|SSD 모델이 안정적으로 학습되어 multiface detection이 가능해졌다.|inference를 통해 정확한 위치의 face bounding box를 detect한 결과이미지가 제출되었다.|⭐|
>|3|이미지 속 다수의 얼굴에 스티커가 적용되었다.|이미지 속 다수의 얼굴의 적절한 위치에 스티커가 적용된 결과이미지가 제출되었다.|⭐|
