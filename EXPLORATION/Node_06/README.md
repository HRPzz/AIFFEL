# 6. 영화리뷰 텍스트 감성분석하기

**imdb 영화 리뷰 평점 데이터를 토대로 사용자들의 리뷰 감성을 분류해 보는 실용적인 텍스트 분류 모델을 구현해 본다.**

- 자연어 처리
  - 자연어 처리에 주로 활용되는 RNN(Recurrent Neural Network)
  - 컴퓨터 비전에서만 사용되는 줄 알았던 CNN(Convolutional Neural Network)이 자연어 처리에서 사용될 수도 있다.
- 네이버나 다음 영화에서 확인할 수 있는 영화리뷰에 대한 감성분석(sentiment analysis) 를 진행

---

[➡ [E-06] Naver_movie_sentiment_analysis.ipynb with nbviewer](https://nbviewer.org/github/HRPzz/AIFFEL/blob/main/EXPLORATION/Node_06/%5BE-06%5D%20Naver_movie_sentiment_analysis.ipynb)

---

|-|목차|⏲ 410분|
|:---:|---|:---:|
|6-1| 들어가며 | 5분|
|6-2| 텍스트 감정분석의 유용성 | 40분|
|6-3| 텍스트 데이터의 특징 | 5분|
|6-4| 텍스트 데이터의 특징 (1) 텍스트를 숫자로 표현하는 방법 | 30분|
|6-5| 텍스트 데이터의 특징 (2) Embedding 레이어의 등장 | 30분|
|6-6| 시퀀스 데이터를 다루는 RNN | 30분|
|6-7| 꼭 RNN이어야 할까? | 25분|
|6-8| IMDB 영화리뷰 감성분석 (1) IMDB 데이터셋 분석 | 5분|
|6-9| IMDB 영화리뷰 감성분석 (2) 딥러닝 모델 설계와 훈련 | 30분|
|6-10| IMDB 영화리뷰 감성분석 (3) Word2Vec의 적용 | 30분|
|6-11| 프로젝트 : 네이버 영화리뷰 감성분석 도전하기 | 180분|
|6-12| 프로젝트 제출|-|

---

## 학습 목표

- 텍스트 데이터를 머신러닝 입출력용 수치데이터로 변환하는 과정을 이해한다.
- RNN의 특징을 이해하고 시퀀셜한 데이터를 다루는 방법을 이해한다.
- 1-D CNN으로도 텍스트를 처리할 수 있음을 이해한다.
- IMDB와 네이버 영화리뷰 데이터셋을 이용한 영화리뷰 감성 분류 실습을 진행한다.

---

## 6-11. 프로젝트 : 네이버 영화리뷰 감성분석 도전하기

- 1) 데이터 준비와 확인

```python
import pandas as pd

# 데이터를 읽어봅시다. 
train_data = pd.read_table('~/aiffel/sentiment_classification/data/ratings_train.txt')
test_data = pd.read_table('~/aiffel/sentiment_classification/data/ratings_test.txt')

train_data.head()
```

- 2) 데이터로더 구성
  - 데이터의 중복 제거
  - NaN 결측치 제거
  - 한국어 토크나이저로 토큰화
  - 불용어(Stopwords) 제거
  - 사전word_to_index 구성
  - 텍스트 스트링을 사전 인덱스 스트링으로 변환
  - X_train, y_train, X_test, y_test, word_to_index 리턴
- 3) 모델 구성을 위한 데이터 분석 및 가공
  - 데이터셋 내 문장 길이 분포
  - 적절한 최대 문장 길이 지정
  - keras.preprocessing.sequence.pad_sequences 을 활용한 패딩 추가
- 4) 모델 구성 및 validation set 구성
- 5) 모델 훈련 개시
- 6) Loss, Accuracy 그래프 시각화
- 7) 학습된 Embedding 레이어 분석
- 8) 한국어 Word2Vec 임베딩 활용하여 성능 개선

한국어 Word2Vec은 다음 경로에서 구할 수 있습니다.

- [Pre-trained word vectors of 30+ languages](https://github.com/Kyubyong/wordvectors)

위 링크에서 적절한 ko.bin을 찾아 이용하세요. 그리고 gensim 버전을 3.x.x로 낮춰야 오류가 나지 않습니다.

---

>## **루브릭**
>
>|번호|평가문항|상세기준|평가결과|
>|:---:|---|---|:---:|
>|1|다양한 방법으로 Text Classification 태스크를 성공적으로 구현하였다.|3가지 이상의 모델이 성공적으로 시도됨|⭐|
>|2|gensim을 활용하여 자체학습된 혹은 사전학습된 임베딩 레이어를 분석하였다.|gensim의 유사단어 찾기를 활용하여 자체학습한 임베딩과 사전학습 임베딩을 적절히 분석함|⭐|
>|3|한국어 Word2Vec을 활용하여 가시적인 성능향상을 달성했다.|네이버 영화리뷰 데이터 감성분석 정확도를 85% 이상 달성함|⭐|
