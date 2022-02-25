# 13. 인간보다 퀴즈를 잘푸는 인공지능

**BERT 모델을 이용한 Q&A 태스크 해결을 수행하며, 자연어처리 분야의 pretrained model의 효용성에 대해 이해해 본다.**

- **BERT** (**B**idirectional **E**ncoder **R**epresentations from Transformers)
  - 2018년 10월, SQuAD(Stanford Question Answering Dataset) 리더보드에 Human performance를 능가하는 점수로 1위 자리를 갱신한 모델
  - 340MB나 되는 엄청난 사이즈의 파라미터를 가진 모델을 수십 GB나 되는 코퍼스를 학습 시켜 만든 pretrained model
  - SQuAD뿐 아니라 당시 거의 모든 자연어처리 태스크의 SOTA(State-of-the-art)를 갱신
  - 엄청난 규모의 언어 모델(Language Model)을 pretrain하고 이를 어떤 태스크에든 약간의 fine-tuning만을 통해 손쉽게 적용하여 해결
  - 수많은 후속 모델들이 BERT의 아이디어를 더욱 발전, 확장시켜 나감
- 프로젝트 목표
  - BERT 모델 구조 공부
  - Pretrained Model을 활용하여 한국형 SQuAD인 KorQuAD 태스크를 수행하는 모델 학습
  - Contextual Word Embedding 의 개념, 자연어처리 분야의 최근 트렌드인 전이학습(Transfer Learning) 활용 방법까지 함께 숙지

---

### 전제조건

- Keras를 활용한 모델 구성 및 학습 진행 방법을 숙지하고 있다.
- LSTM의 개념을 이해하고 모델 구성에 활용할 수 있다.
- Transformer 모델 구조와 Attention의 개념에 대해 이해하고 있다.

### 학습 목표

- Transformer Encoder로 이루어진 BERT의 모델 구조를 이해한다.
- Pretrained embedding 접근 방식에 대해 이해한다.
- Pretrained BERT를 활용할 수 있다.

### 준비물

- KorQuAD 데이터셋 출처: <https://korquad.github.io/>

---

## 13-6. 프로젝트 : Pretrained model의 활용

- STEP 1. pretrained model 로딩하기
- STEP 2. pretrained model finetune 하기
- STEP 3. Inference 수행하기
- STEP 4. 학습 경과 시각화 비교 분석

---

>## **루브릭**

>|번호|평가문항|상세기준|평가결과|
>|:---:|---|---|:---:|
>|1|BERT pretrained model을 활용한 KorQuAD 모델이 정상적으로 학습이 진행되었다.|KorQuAD 모델의 validation accuracy가 안정적으로 증가하였다.|⭐|
>|2|KorQuAD Inference 결과가 원래의 정답과 비교하여 유사하게 나오는 것을 확인하였다.|평가셋에 대해 모델 추론 결과와 실제 정답의 유사성이 확인되었다.|⭐|
>|3|pretrained model 활용이 효과적임을 실험을 통해 확인하였다.|pretrained model을 사용하지 않았을 때 대비 학습경과의 차이를 시각화를 통해 확인하였다.|⭐|
