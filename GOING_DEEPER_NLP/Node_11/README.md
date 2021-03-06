# 11. 기계 번역이 걸어온 길

**번역 모델이 발전해 온 과정을 살펴보고, 번역을 생성하는 여러 가지 방법을 소개합니다. 자연어 처리에서 DATA Augmentation을 어떻게 할 수 있는지, 자연어 처리 성능은 어떻게 측정할 수 있는지 알아봅니다. 기계 번역과 뗄 수 없는 챗봇도 간단히 공부합니다.**

---

|-|목차|⏲ 220분|
|:---:|---|:---:|
|11-1.| 들어가며 | 5분|
|11-2.| 번역의 흐름 | 25분|
|11-3.| 지적 생성을 위한 넓고 얕은 탐색 (1) Greedy Decoding | 15분|
|11-4.| 지적 생성을 위한 넓고 얕은 탐색 (2) Beam Search | 15분|
|11-5.| 지적 생성을 위한 넓고 얕은 탐색 (3) Sampling | 10분|
|11-6.| 방과 후 번역 수업 (1) Data Augmentation | 15분|
|11-7.| 방과 후 번역 수업 (2) Lexical Substitution| 20분|
|11-7.| 방과 후 번역 수업 (3) Back Translation | 20분|
|11-7.| 방과 후 번역 수업 (4) Random Noise Injection | 15분|
|11-7.| 채점은 어떻게? | 25분|
|11-7.| 실례지만, 어디 챗씨입니까? (1) 챗봇과 번역기 | 15분|
|11-7.| 실례지만, 어디 챗씨입니까? (2) 좋은 챗봇이 되려면 | 20분|
|11-7.| 실례지만, 어디 챗씨입니까? (3) 대표적인 챗봇 | 15분|
|11-8.| 마무리 |5분|

---

## 학습 목표

1. 번역 모델이 발전해 온 과정을 살펴본다.
2. 번역을 생성하는 여러 가지 방법을 이해한다.
3. 주어진 데이터로 더 높은 성능을 만들어 내는 법을 배운다.
4. 자연어 처리의 성능을 측정하기 위한 지표를 배운다.
