# 12. 직접 만들어보는 OCR

**Text recognition 모델을 구현, 학습하고 Text detection 모델과 연결하여 OCR을 구현한다.**

---

[➡ [GD-12] Making_OCR.ipynb with nbviewer](https://nbviewer.org/github/HRPzz/AIFFEL/blob/main/GOING_DEEPER_CV/Node_12/%5BCV-12%5D%20Making_OCR.ipynb)

---

|-|목차|⏲ 400분|
|:---:|---|:---:|
|12-1| 들어가며 | 5분|
|12-2| Overall structure of OCR | 15분|
|12-3| Dataset for OCR | 60분|
|12-4| Recognition model (1) | 30분|
|12-5| Recognition model (2) Input Image | 30분|
|12-6| Recognition model (3) Encode | 20분|
|12-7| Recognition model (4) Build CRNN model | 30분|
|12-8| Recognition model (5) Train & Inference | 30분|
|12-9| 프로젝트: End-to-End OCR | 180분|
|12-10| 프로젝트 제출 |-|

---

## 실습목표

1. Text Recognition 모델을 직접 구현해 봅니다.
2. Text Recognition 모델 학습을 진행해 봅니다.
3. Text Detection 모델과 연결하여 전체 OCR 시스템을 구현합니다.

## 목차

1. Overall sturcture of OCR
2. Dataset for OCR
3. Recognition model
    - Input Image
    - Encode
    - Build CRNN model
    - Train & Inference
4. 프로젝트 : End-to-End OCR

---

## 12-9. 프로젝트: End-to-End OCR

- keras-ocr의 Detector 사용
- Recognition model로 인식하는 함수를 직접 작성하고 그 결과를 출력해보기

---

>## **루브릭**
>
>|번호|평가문항|상세기준|평가결과|
>|:---:|---|---|:---:|
>|1|Text recognition을 위해 특화된 데이터셋 구성이 체계적으로 진행되었다.|텍스트 이미지 리사이징, ctc loss 측정을 위한 라벨 인코딩, 배치처리 등이 적절히 수행되었다.|-|
>|2|CRNN 기반의 recognition 모델의 학습이 정상적으로 진행되었다.|학습결과 loss가 안정적으로 감소하고 대부분의 문자인식 추론 결과가 정확하다.|-|
>|3|keras-ocr detector와 CRNN recognizer를 엮어 원본 이미지 입력으로부터 text가 출력되는 OCR이 End-to-End로 구성되었다.|샘플 이미지를 원본으로 받아 OCR 수행 결과를 리턴하는 1개의 함수가 만들어졌다.|-|
