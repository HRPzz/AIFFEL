# 8.뉴스 요약봇 만들기

**텍스트 요약을 구현하는 Extractive/Abstractive 접근법에 대해 알아보고, Attentional seq2seq 구조를 활용하여 뉴스 기사를 모델로 구현해 본다.**

- 텍스트 요약(Text Summarization)
  - 끊임없이 늘어나는 정보를 쉽고 빠르게 소화하기 위해서 굉장히 중요한 기술
  - 큰 정보들을 소화할 수 있도록 도와줌

- 추출적 요약(Extractive Summarization)
  - 원문에서 문장들을 추출해서 요약하는 방식
  - 결과로 나온 문장들 간의 호응이 자연스럽지 않을 수 있다.
  - 요약문에 들어갈 핵심문장인지 판별한다는 점에서 문장 분류(Text Classification) 문제

- 추상적 요약(Abstractive Summarization)
  - 원문으로부터 내용이 요약된 새로운 문장을 생성
    - 결과로 나온 문장이 원문에 원래 없던 문장일 수도 있다.
  - 자연어 처리 분야 중 자연어 생성(Natural Language Generation, NLG)의 영역
    - 자연어 생성 기본 신경망: RNN

- 추출적 요약 vs 추상적 요약
  - 추출적 요약: 문장 분류(Text Classification) 문제
  - 추상적 요약: 자연어 생성(Natural Language Generation, NLG)의 영역

---

[➡ [E-08] news_summary_bot.ipynb with nbviewer](https://nbviewer.org/github/HRPzz/AIFFEL/blob/main/EXPLORATION/Node_08/%5BE-08%5D%20news_summary_bot.ipynb)

---

|-|목차|⏲ 420분|
|:---:|---|:---:|
|8-1| 들어가며 | 5분|
|8-2| 텍스트 요약(Text Summarization)이란? | 20분|
|8-3| 인공 신경망으로 텍스트 요약 훈련시키기 | 20분|
|8-4| 데이터 준비하기 | 15분|
|8-5| 데이터 전처리하기 (1) 데이터 정리하기 | 35분|
|8-6| 데이터 전처리하기 (2) 훈련데이터와 테스트데이터 나누기 | 30분|
|8-7| 데이터 전처리하기 (3) 정수 인코딩 | 20분|
|8-8| 모델 설계하기 | 30분|
|8-9| 모델 훈련하기 | 30분|
|8-10| 인퍼런스 모델 구현하기 | 30분|
|8-11| 모델 테스트하기 | 15분|
|8-12| 추출적 요약 해보기 | 20분|
|8-13| 프로젝트: 뉴스기사 요약해보기 | 150분|
|8-14| 프로젝트 제출|-|

---

## 학습 목표

- Extractive/Abstractive summarization 이해하기
- 단어장 크기를 줄이는 다양한 text normalization 적용해보기
- seq2seq의 성능을 Up시키는 Attention Mechanism 적용하기

---

## 8-13. 프로젝트: 뉴스기사 요약해보기

- Step 1. 데이터 수집하기
  - 뉴스 기사 데이터 사용
  - [sunnysai12345/News_Summary](https://github.com/sunnysai12345/News_Summary)
- Step 2. 데이터 전처리하기 (추상적 요약)
- Step 3. 어텐션 메커니즘 사용하기 (추상적 요약)
- Step 4. 실제 결과와 요약문 비교하기 (추상적 요약)
- Step 5. Summa을 이용해서 추출적 요약해보기

---

>## **루브릭**
>
>|번호|평가문항|상세기준|평가결과|
>|:---:|---|---|:---:|
>|1|Abstractive 모델 구성을 위한 텍스트 전처리 단계가 체계적으로 진행되었다.|분석단계, 정제단계, 정규화와 불용어 제거, 데이터셋 분리, 인코딩 과정이 빠짐없이 체계적으로 진행되었다.|⭐|
>|2|텍스트 요약모델이 성공적으로 학습되었음을 확인하였다.|모델학습이 안정적으로 수렴되었음을 그래프를 통해 확인하였으며, 실제 요약문과 유사한 요약문장을 얻을 수 있었다.|⭐|
>|3|Extractive 요약을 시도해 보고 Abstractive 요약 결과과 함께 비교해 보았다.|두 요약 결과를 문법완성도 측면과 핵심단어 포함 측면으로 나누어 비교분석 결과를 제시하였다.|⭐|
