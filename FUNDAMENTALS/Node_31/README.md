# 31. 뉴스기사 크롤링 및 분류

**뉴스기사를 주제/섹션별로 모아서 데이터셋을 구축하고, 이를 기반으로 뉴스기사 주제를 분류하는 텍스트 분류기를 구현해 본다.**

---

|-|목차|⏲ 395분|
|:---:|---|:---:|
|31-1| 들어가며 | 10분|
|31-2| 웹 이해하기 (1) HTML과 태그 | 20분|
|31-3| 웹 이해하기 (2) 선택자 | 10분|
|31-4| BeautifulSoup 패키지 | 15분|
|31-5| newspaper3k 패키지 | 10분|
|31-6| 네이버 뉴스 기사 크롤링 (1) 뉴스 URL, 페이지 이해하기 | 10분|
|31-7| 네이버 뉴스 기사 크롤링 (2) BeautifulSoup와 newspaper3k를 통해 크롤러 만들기 | 20분|
|31-8| 네이버 뉴스 기사 크롤링 (3) 데이터 수집 및 전처리 | 60분|
|31-9| 네이버 뉴스 기사 크롤링 (4) 데이터 전처리 | 30분|
|31-10| 머신 러닝 사용하기 | 30분|
|31-11| 프로젝트 - 모델 성능 개선하기 | 180분|

---

## 학습 목표

1. HTML 문서의 개념에 대해서 이해한다.
2. 태그의 형식에 대해서 이해한다.
3. 크롤링을 위한 패키지인 BeautifulSoup4의 사용법을 이해한다.
4. 머신 러닝 분류 방법인 나이브 베이즈 분류기의 사용법을 익힌다.

---

## 31-11. 프로젝트 - 모델 성능 개선하기

- Step 1. 형태소 분석기 변경해 보기
- Step 2. 불용어 추가해 보기
- Step 3. 다른 날짜 데이터 추가해 보기

---

## 내용 정리

- 웹 사이트들은 HTML(HyperText Markup Language) 이라는 마크업 언어로 작성된 문서로 구성됨
  - 웹 페이지에서 마우스 우클릭 후에 '페이지 소스 보기'를 클릭하면 HTML 문서의 소스 코드를 확인할 수 있음
- HTML
  - 태그(Tag): 꺾쇠(<>)로 구성된 코드 e.g. <태그>내용</태그>
    - p 태그, span 태그, a 태그
  - 선택자(Selector): 어떤 특정 태그들에 그룹이나 번호를 주는 기능
    - id
    - class: 비슷한 성격을 가지는 그룹을 정의해서 함께 관리하고 싶을 때 사용
- 크롤링(crawling): 웹 페이지로부터 데이터를 추출하는 행위
  - 크롤러(crawler): 크롤링하는 소프트웨어
  - 파이썬 크롤링 패키지
    - BeautifulSoup: HTML이나 XML 문서로부터 원하는 정보를 추출
    - newspaper3k: 뉴스 데이터(뉴스 기사 제목, 텍스트)를 크롤링하기 위해 만들어진 패키지

```python
# BeautifulSoup

from bs4 import BeautifulSoup

html = '<html>내용</html'
soup = BeautifulSoup(html, 'html.parser')  # HTML 문법을 기반으로 파싱

# 정보 가져오기
print(soup.select('body'))

# h1 자식인 클래스 .name의 자식인 클래스 .menu의 정보 가져오기
print(soup.select('h1 .name .menu'))
```

```python
from newspaper import Article

url = '파싱할 뉴스 기사 주소'
article = Article(url, language='ko')  # 한국어
article.download()
article.parse()

# 기사 제목
print(article.title)

# 기사 내용
print(article.text)
```

- 임의의 뉴스에 대해서 카테고리 예측하기
  - 전처리(텍스트 데이터 -> 벡터 변환): TF-IDF
  - 머신 러닝 모델: 나이브 베이즈 분류기
  - 머신 러닝 성능 측정 방법: F1 Score
  - 형태소 분석기: Mecab
