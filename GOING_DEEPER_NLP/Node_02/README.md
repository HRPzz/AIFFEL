# 2. 멋진 단어사전 만들기

**소설책 1권 분량의 텍스트를 lecture에서 다룬 다양한 기법으로 전처리하여 단어가전을 만들어 본다. 각각의 단어사전을 토대로 language model을 생성하여 perplexity를 측정해 보고 어떤 기법의 성능이 우수한지 평가해 본다.**

---

|-|목차|⏲ 300분|
|:---:|---|:---:|
|2-1| 들어가며 | 5분|
|2-2| 데이터 다운로드 및 분석 | 25분|
|2-3| 공백 기반 토큰화 | 30분|
|2-4| 형태소 기반 토큰화 | 30분|
|2-5| 프로젝트: SentencePiece 사용하기 | 210분|
|2-6| 프로젝트 제출 |

---

## 2-5. 프로젝트: SentencePiece 사용하기

- Step 1. SentencePiece 설치하기
- Step 2. SentencePiece 모델 학습
- Step 3. Tokenizer 함수 작성
- Step 4. 네이버 영화리뷰 감정 분석 문제에 SentencePiece 적용해 보기

---

>## **루브릭**
>
>|번호|평가문항|상세기준|
>|:---:|---|---|
>|1|SentencePiece를 이용하여 모델을 만들기까지의 과정이 정상적으로 진행되었는가?|코퍼스 분석, 전처리, SentencePiece 적용, 토크나이저 구현 및 동작이 빠짐없이 진행되었는가?|
>|2|SentencePiece를 통해 만든 Tokenizer가 자연어처리 모델과 결합하여 동작하는가?|SentencePiece 토크나이저가 적용된 Text Classifier 모델이 정상적으로 수렴하여 80% 이상의 test accuracy가 확인되었다.|
>|3|SentencePiece의 성능을 다각도로 비교분석하였는가?|SentencePiece 토크나이저를 활용했을 때의 성능을 다른 토크나이저 혹은 SentencePiece의 다른 옵션의 경우와 비교하여 분석을 체계적으로 진행하였다.|
