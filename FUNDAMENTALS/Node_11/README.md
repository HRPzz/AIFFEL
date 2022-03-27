# 11. 데이터를 한눈에! Visualization

**Pandas, Matplotlib, Seaborn을 활용해 다양한 형태의 히스토그램이나 그래프로 시각화하는 기법을 익히고, 시계열 데이터를 시각화하는 실습을 진행한다.**

---

|-|목차|⏲ 165분|
|:---:|---|:---:|
|11-1| 들어가며 | 5분|
|11-2| 파이썬으로 그래프를 그린다는건? | 10분|
|11-3| 간단한 그래프 그리기 (1) 막대그래프 그려보기 | 15분|
|11-4| 간단한 그래프 그리기 (2) 선 그래프 그려보기 | 20분|
|11-5| 간단한 그래프 그리기 (3) plot 사용법 상세 | 15분|
|11-6| 간단한 그래프 그리기 (4) 정리해 보자 | 10분|
|11-7| 그래프 4대 천왕 (1) 데이터 준비 | 15분|
|11-8| 그래프 4대 천왕 (2) 범주형 데이터 | 20분|
|11-9| 그래프 4대 천왕 (3) 수치형 데이터 | 20분|
|11-10| 시계열 데이터 시각화하기 | 20분|
|11-11| Heatmap | 15분|

---

## 학습 목표

- 파이썬 라이브러리(Pandas, Matplotlib, Seaborn)을 이용해서 여러 가지 그래프를 그리는 법을 학습합니다.
- 실전 데이터셋으로 직접 시각화해보며 데이터 분석에 필요한 탐색적 데이터 분석(EDA)을 하고 인사이트를 도출해 봅니다.

## 학습 목차

1. 파이썬으로 그래프를 그린다는 건?
2. 간단한 그래프 그리기
3. 그래프 4대 천왕: 막대그래프, 선그래프, 산점도, 히스토그램
4. 시계열 데이터 시각화하기
5. Heatmap

---

## 노드 내용

- 시각화
  - 라이브러리: Pandas, Matplotlib, Seaborn
  - 그래프
    - 도화지를 펼치고 축을 그리고 그 안에 데이터를 그리는 작업
    - 그래프 종류
      - 막대 그래프(bar graph): 범주형 데이터
      - 선 그래프(line graph): 수치형 데이터
      - 산점도 그래프(scatter plot): 수치형 데이터
      - 히스토그램(histogram): 도수분포표 그래프화
      - 밀도 그래프: 연속된 확률분포
      - Heatmap: 방대한 양의 데이터와 현상을 수치에 따른 색상으로 나타내는 것

- 그래프 그리기 1

```python
# 그래프 데이터 
subject = ['English', 'Math', 'Korean', 'Science', 'Computer']
points = [40, 90, 50, 60, 100]

# 축 그리기
fig = plt.figure()
ax1 = fig.add_subplot(1,1,1)
# plt.ylim()  # y 좌표축 범위 설정
# plt.xlim()  # x 좌표축 범위 설정

# 그래프 그리기
ax1.bar(subject, points)
# ax1.annotate()  # 그래프 안에 주석 달기

# 라벨, 타이틀 달기
plt.xlabel('Subject')
plt.ylabel('Points')
plt.title("Yuna's Test Result")

# 그리드(격자눈금) 추가
plt.grid()

# 보여주기
plt.show()
```

- 그래프 그리기 2

```python
x = np.linspace(0, 10, 100) 

plt.subplot(2,1,1)
plt.plot(x, np.sin(x),'orange','o')

plt.subplot(2,1,2)
plt.plot(x, np.cos(x), 'orange') 
plt.show()
```

- pandas.plot 메서드 인자
  - label: 그래프의 범례 이름.
  - ax: 그래프를 그릴 matplotlib의 서브플롯 객체.
  - style: matplotlib에 전달할 'ko--'같은 스타일의 문자열
  - alpha: 투명도 (0 ~1)
  - kind: 그래프의 종류: line, bar, barh, kde
  - logy: Y축에 대한 로그 스케일
  - use_index: 객체의 색인을 눈금 이름으로 사용할지의 여부
  - rot: 눈금 이름을 로테이션(0 ~ 360)
  - xticks, yticks: x축, y축으로 사용할 값
  - xlim, ylim: x축, y축 한계
  - grid: 축의 그리드 표시할지 여부
- pandas의 data가 DataFrame 일 때 plot 메서드 인자
  - subplots: 각 DataFrame의 칼럼을 독립된 서브플롯에 그린다.
  - sharex: subplots=True 면 같은 X 축을 공유하고 눈금과 한계를 연결한다.
  - sharey: subplots=True 면 같은 Y 축을 공유한다.
  - figsize: 그래프의 크기, 튜플로 지정
  - title: 그래프의 제목을 문자열로 지정
  - sort_columns: 칼럼을 알파벳 순서로 그린다.

- 그래프 그리는 과정
    1. fig = plt.figure(): figure 객체를 선언해 '도화지를 펼쳐' 줍니다.
    2. ax1 = fig.add_subplot(1,1,1) : 축을 그립니다.
    3. ax1.bar(x, y) 축안에 어떤 그래프를 그릴지 메서드를 선택한 다음, 인자로 데이터를 넣어줍니다.
    4. 그래프 타이틀 축의 레이블 등을 plt의 여러 메서드 grid, xlabel, ylabel 을 이용해서 추가해 주고
    5. plt.savefig 메서드를 이용해 저장해 줍니다.
- 그래프 요소별 명칭<br>![Graph's Elements](https://d3s0tskafalll9.cloudfront.net/media/images/F-15-2.max-800x600.png)
