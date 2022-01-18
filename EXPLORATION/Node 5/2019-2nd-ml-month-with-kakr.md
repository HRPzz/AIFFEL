# [2019 2nd ML month with KaKR 캐글 코리아와 함께하는 2nd ML 대회 - House Price Prediction](https://www.kaggle.com/c/2019-2nd-ml-month-with-kakr/overview)


## Overview
### Description
#### Introduction
본 대회는 구글 코리아가 후원하고, 캐글 코리아(비영리 페이스북 온라인 커뮤니티)가 진행하는 데이터 사이언스 대회입니다. Academic 목적이며, 대한민국 누구나 참여하실 수 있습니다.

#### Competition background
내 집 마련의 꿈은 누구나 가지고 있습니다. 하지만 집의 가격은 누구나 알고 있지는 않죠. 집의 가격은 주거 공간의 면적, 위치, 경관, 건물의 연식 등 여러 가지 복잡한 요인의 조합에 의해 결정됩니다. 이번에 분석하실 데이터는 20개의 변수를 가지고 있으며, 어떤 조건을 가진 집의 가격이 높고 낮은지를 예측하는 모델을 만드는 것을 목표로 합니다. 이번 대회는 리더보드 점수뿐만 아니라 캐글의 공유 정신의 기본인 커널 작성을 장려하는 목표를 가지고 있습니다.

### Evaluation
#### RMSE
이번 대회의 평가 방식은 Root Mean Squared Error 입니다.
$$ {\sqrt{ {1 \over N} \sum{(yt - y{pr})}^2}} $$

### Prize
2019 ML month 2nd의 Prize는 세가지 Prize Track으로 구성되어 있습니다.

- 상위 리더보드 100명 (대회가 마무리된 후 사용한 소스코드를 커널 항목에 공개 의무가 있습니다.)
- 커널 Vote를 가장 많이 받은 상위 10팀
- 튜터들이 뽑은 좋은 분석이 담긴 커널을 만든 10팀
- 총 120팀

### Timeline
대회 일정 UTC time 기준
- Start Date : 2019/03/11 00:00
- Entry Deadline : None
- End Date : 2019/04/22 23:59


## Data
### Data Description
#### File descriptions
- train.csv - 예측 모델을 만들기 위해 사용하는 학습 데이터입니다. 집의 정보와 예측할 변수인 가격(Price) 변수를 가지고 있습니다.
- test.csv - 학습셋으로 만든 모델을 가지고 예측할 가격(Price) 변수를 제외한 집의 정보가 담긴 테스트 데이터 입니다.
- sample_submission.csv - 제출시 사용할 수 있는 예시 submission.csv 파일입니다.

#### Data fields
1. ID : 집을 구분하는 번호
2. date : 집을 구매한 날짜
3. price : 집의 가격(Target variable)
4. bedrooms : 침실의 수
5. bathrooms : 화장실의 수
6. sqft_living : 주거 공간의 평방 피트(면적)
7. sqft_lot : 부지의 평방 피트(면적)
8. floors : 집의 층 수
9. waterfront : 집의 전방에 강이 흐르는지 유무 (a.k.a. 리버뷰)
10. view : 집이 얼마나 좋아 보이는지의 정도
11. condition : 집의 전반적인 상태
12. grade : King County grading 시스템 기준으로 매긴 집의 등급
13. sqft_above : 지하실을 제외한 평방 피트(면적)
14. sqft_basement : 지하실의 평방 피트(면적)
15. yr_built : 지어진 년도
16. yr_renovated : 집을 재건축한 년도
17. zipcode : 우편번호
18. lat : 위도
19. long : 경도
20. sqft_living15 : 2015년 기준 주거 공간의 평방 피트(면적, 집을 재건축했다면, 변화가 있을 수 있음)
21. sqft_lot15 : 2015년 기준 부지의 평방 피트(면적, 집을 재건축했다면, 변화가 있을 수 있음)

## Rulse
### 유의 사항
본 컴퍼티션은 ML을 공부하고, 즐기기 위한 시작을 알리는 컴퍼티션 입니다.

정정당당하게 자신의 실력을 가늠하며 같이 발전하는 컴퍼티션이 될 수 있도록 동참 부탁드립니다.

이를 위해 아래 규칙을 잘 숙지하여 따라주시길 바랍니다.

### 컴퍼티션 규칙
1. 한 참가자당 한 개의 캐글 계정을 가지고 참여하셔야 합니다.
2. 본 대회에서는 외부 데이터의 사용을 금합니다.
3. Team merger
본 대회는 team merger가 허용되지 않습니다.

하루 최대 제출 횟수만큼인 5번 제출할 수 있습니다.

4. Competition Timeline
Start Date: 2019/03/11 00:00 AM UTC Time

Entry Deadline: None

End Date: 2019/04/22 23:59 PM UTC