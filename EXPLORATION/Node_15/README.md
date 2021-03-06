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

[➡ [E-15] OCR.ipynb with nbviewer](https://nbviewer.org/github/HRPzz/AIFFEL/blob/main/EXPLORATION/Node_15/%5BE-15%5D%20OCR.ipynb)

---

|-|목차|⏲ 360분|
|:---:|---|:---:|
|15-1| 들어가며 | 15분|
|15-2| 기계가 읽을 수 있나요? | 40분|
|15-3| 어떤 과정으로 읽을까요? | 15분|
|15-4| 딥러닝 문자인식의 시작 | 15분|
|15-5| 사진 속 문자 찾아내기 - detection | 15분|
|15-6| 사진 속 문자 읽어내기 - recognition | 60분|
|15-7| keras-ocr 써보기 | 40분|
|15-8| 테서랙트 써보기 | 40분|
|15-9| 프로젝트 : 다양한 OCR모델 비교하기 | 120분|
|15-10| 프로젝트 제출|-|

---

## 실습목표

- OCR의 과정을 이해합니다.
- 문자인식 결과의 표현방식을 이해합니다.
- 파이썬을 통해 OCR을 사용할 수 있습니다.

## 학습 목차

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
>
>|번호|평가문항|상세기준|평가결과|
>|:---:|---|---|:---:|
>|1|OCR을 활용하여 구현하려는 서비스의 기획이 타당한가?|목표로 하는 서비스가 OCR를 적용 가능하며, OCR을 활용했을 때 더욱 유용해진다.|⭐|
>|2|모델 평가기준이 명확하고 체계적으로 세워졌는가?|평가 기준에 부합하는 테스트 데이터의 특징이 무엇인지 명확하게 제시되었다.|⭐|
>|3|평가기준에 따라 충분한 분량의 테스트가 진행되고 그 결과가 잘 정리되었는가?|최대 20장까지의 테스트 이미지를 사용해 제시된 평가 기준에 따른 테스트 결과가 잘 정리되어 결론이 도출되었다.|⭐|
