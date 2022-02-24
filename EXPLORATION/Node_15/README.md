# 15. 문자를 읽을 수 있는 딥러닝

**문자를 읽는 OCR 모델의 구조를 배우고, keras-ocr과 테서랙트 엔진을 써본다.**

- OCR(Optical Character Recognition, 광학 문자 인식)
  - 인식(Detection) -> 해독(Recognition)
  - 딥러닝 기반 Object Detection: Regression(회귀), Segmentation(세그멘테이션)
  - 딥러닝 기반 Recognition: CRNN (=CNN + RNN)
    - CNN: 이미지 내 텍스트와 연관된 특징 추출
    - RNN: 스텝 단위 문자 정보 인식
- OCR 사용하기
  - 구글 클라우드 기반 OCR API 사용 가능
  - 인식 모델 keras-ocr CRNN 사용, 검출 모델 네이버 데뷰 2018 영상에서 소개한 CRAFT(Character Region Awareness for Text Detection)
  - 테서랙트(Tesseract) 라이브러리

---

### 실습목표

- OCR의 과정을 이해합니다.
- 문자인식 결과의 표현방식을 이해합니다.
- 파이썬을 통해 OCR을 사용할 수 있습니다.

### 학습 목차

1. 들어가며
2. 기계가 읽을 수 있나요?
3. 어떤 과정으로 읽을까요?
4. 딥러닝 문자인식의 시작
5. 사진 속 문자 찾아내기 - detection
6. 사진 속 문자 읽어내기 - recognition
7. keras-ocr 써보기
8. 테서랙트 써보기
9. 프로젝트 : 다양한 OCR 모델 비교하기

---

## 15-9. 프로젝트 : 다양한 OCR모델 비교하기

- Step1. 검증용 데이터셋 준비
- Step2. keras-ocr, Tesseract로 테스트 진행(Google OCR API는 선택 사항)
- Step3. 테스트 결과 정리
- Step4. 결과 분석과 결론 제시

---

>## **루브릭**

>|번호|평가문항|상세기준|평가결과|
>|:---:|---|---|:---:|
>|1|BERT pretrained model을 활용한 KorQuAD 모델이 정상적으로 학습이 진행되었다.|KorQuAD 모델의 validation accuracy가 안정적으로 증가하였다.|-|
>|2|KorQuAD Inference 결과가 원래의 정답과 비교하여 유사하게 나오는 것을 확인하였다.|평가셋에 대해 모델 추론 결과와 실제 정답의 유사성이 확인되었다.|-|
>|3|pretrained model 활용이 효과적임을 실험을 통해 확인하였다.|pretrained model을 사용하지 않았을 때 대비 학습경과의 차이를 시각화를 통해 확인하였다.|-|
