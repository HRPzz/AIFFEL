# 13. modern NLP의 흐름에 올라타보자

**Transformer를 바탕으로 한 최근 NLP의 모델에 대해서 알아보겠습니다.**

---

|-|목차|⏲ 300분|
|:---:|---|:---:|
|13-1| 들어가며 | 10분|
|13-2| Transfer Learning과 Language Modeling | 10분|
|13-3| ELMO(Embedding from Language Models) | 30분|
|13-4| GPT(Generative Pre-Training Transformer) | 30분|
|13-5| BERT(Bidirectional Encoder Representations from Transformers) | 40분|
|13-6| Transformer-XL(Transformer Extra Long) | 30분|
|13-7| XLNet, BART | 30분|
|13-8| ALBERT(A Lite BERT for Self-supervised Learning of Language Representations) | 30분|
|13-9| T5(Text-to-Text Transfer Transformer) | 30분|
|13-10| Switch Transformer | 30분|
|13-11| ERNIE | 30분|

---

## 12-7. Project: 멋진 챗봇 만들기

- Step 1. 데이터 다운로드
- Step 2. 데이터 정제
- Step 3. 데이터 토큰화
- Step 4. Augmentation
- Step 5. 데이터 벡터화
- Step 6. 훈련하기
- Step 7. 성능 측정하기

---

>## **루브릭**
>
>|번호|평가문항|상세기준|
>|:---:|---|---|
>|1|챗봇 훈련데이터 전처리 과정이 체계적으로 진행되었는가?|챗봇 훈련데이터를 위한 전처리와 augmentation이 적절히 수행되어 3만개 가량의 훈련데이터셋이 구축되었다.|
>|2|transformer 모델을 활용한 챗봇 모델이 과적합을 피해 안정적으로 훈련되었는가?|과적합을 피할 수 있는 하이퍼파라미터 셋이 적절히 제시되었다.|
>|3|챗봇이 사용자의 질문에 그럴듯한 형태로 답하는 사례가 있는가?|주어진 예문을 포함하여 챗봇에 던진 질문에 적절히 답하는 사례가 제출되었다.|
