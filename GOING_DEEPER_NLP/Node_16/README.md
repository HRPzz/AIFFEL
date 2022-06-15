# 16. HuggingFace 커스텀 프로젝트 만들기

**대표적인 NLP framework인 Huggingface transformers를 활용하여 자기만의 커스텀 프로젝트를 제작하는 실습을 진행해 본다.**

---

|-|목차|⏲ 340분|
|:---:|---|:---:|
|16-1| 들어가며 | 5분|
|16-2| GLUE dataset과 Huggingface | 35분|
|16-3| 커스텀 프로젝트 제작 (1) Processor | 40분|
|16-4| 커스텀 프로젝트 제작 (2) Tokenizer와 Model | 40분|
|16-5| 커스텀 프로젝트 제작 (3) Train/Evaluation과 Test | 40분|
|16-6| 프로젝트 : 커스텀 프로젝트 직접 만들기 | 180분|
|16-7| 프로젝트 제출 |-|

---

## 오늘의 목차

1. GLUE dataset과 Huggingface
2. 커스텀 프로젝트 제작 (1) Processor
3. 커스텀 프로젝트 제작 (2) Tokenizer와 Model
4. 커스텀 프로젝트 제작 (3) Train/Evaluation과 Test
5. 커스텀 프로젝트 직접 만들기

---

## 16-6. 프로젝트 : 커스텀 프로젝트 직접 만들기

- STEP 1. mnli 데이터셋을 분석해 보기
- STEP 2. `MNLIProcessor` 클래스 구현하기
- STEP 3. 위에서 구현한 processor 및 Huggingface에서 제공하는 tokenizer를 활용하여 데이터셋 구성하기
- STEP 4. model을 생성하여 학습 및 테스트를 진행해 보기

---

>## **루브릭**
>
>|번호|평가문항|상세기준|
>|:---:|---|---|
>|1|MNLI 데이터셋을 처리하는 전용 Processor 클래스를 정상적으로 구현하였다.|Processor 클래스에 대해 1개 이상의 example에 대한 단위테스트가 정상 진행되었다.|
>|2|BERT tokenizer와 Processor를 결합하여 데이터셋을 정상적으로 생성하였다.|MNLI 데이터셋의 입력과 라벨의 정의에 잘 맞는 tf.data.Dataset 인스턴스가 얻어졌다.|
>|3|MNLI 데이터셋에 대해 적당한 모델을 fine-tuning하여 학습하였다.|모델 학습이 정상적으로 진행되었다.|
