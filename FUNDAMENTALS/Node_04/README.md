# 4. 개발자를 위한 첫 번째 필수 교양

**개발에 필요한 git, jupyter notebook 등 기본 툴을 익힌다.**

---

|-|목차|⏲ 180분|
|:---:|---|:---:|
|4-1| 학습 목표 및 목차 | 5분|
|4-2| 개발자를 위한 첫 번째 필수 교양 (1) 협업을 더 잘, 더 편리하게 하기 위해 | 10분|
|4-3| 개발자를 위한 첫 번째 필수 교양 (2) 개발자의 가드닝, 잔디 심기 | 10분|
|4-4| 내 코드의 모든 발자취를 기록할 수 있을까? (1) 버전관리와 Git | 10분|
|4-5| 코드의 모든 발자취를 기록할 수 있을까? (2) Git과 GitHub | 10분|
|4-6| 내 코드의 모든 발자취를 기록할 수 있을까? (3) Git, GitHub 셋팅하기 | 10분|
|4-7| GitHub에 첫 번째 잔디 심기, 어렵지 않아요! (1) 로컬 저장소 | 10분|
|4-8| GitHub에 첫 번째 잔디 심기, 어렵지 않아요! (2) 변화 추적하기 | 10분|
|4-9| GitHub에 첫 번째 잔디 심기, 어렵지 않아요! (3) 원격 저장소 | 10분|
|4-10| GitHub에 첫 번째 잔디 심기, 어렵지 않아요! (4) 협업하기 | 10분|
|4-11| GitHub에 첫 번째 잔디 심기, 어렵지 않아요! (5) 받아오기 | 10분|
|4-12| GitHub에 첫 번째 잔디 심기, 어렵지 않아요! (6) 정리 | 10분|
|4-13| 데이터 분석계의 워드, Jupyter Notebook 마스터하기 (1) 노트북 생성 | 15분|
|4-14| 데이터 분석계의 워드, Jupyter Notebook 마스터하기 (2) 노트북 셀 | 15분|
|4-15| 개발자 문서작업의 시작과 끝! 마크다운을 익혀보자 (1) 마크다운 소개 | 10분|
|4-16| 개발자 문서작업의 시작과 끝! 마크다운을 익혀보자 (2) 마크다운 작성하기 | 20분|
|4-17| 마무리 | 5분|

---

## 학습 전제

- 협업 관리 툴을 다뤄본 적이 없다.
- Git과 GitHub에 대한 개념을 공부해본 적이 없다.
- GitHub을 이용해 소스코드의 버전 관리를 해 본 적이 없다.
- pip를 활용해 본 적은 있으나, Jupyter Notebook을 설치해 사용해 본 적이 없다.
- 마크다운 문법을 공부하거나 활용해본 적이 없다.

## 학습 목표

- Git과 GitHub의 차이를 말할 수 있다.
- Git의 add, commit, push, pull 등의 명령어를 알고, 활용할 수 있다.
- GitHub을 활용해 소스코드 버전관리를 할 줄 안다.
- Jupyter Notebook을 활용해 자유자재로 코드/문서 작업을 할 수 있다.
- 마크다운 문법을 활용해 자유롭게 문서를 작성할 수 있다.

## 목차

- Step 1. 개발자를 위한 첫 번째 필수 교양
  - 협업에 필요한 여러 가지 툴들에 대해 알아본다.
  - GitHub의 존재와 그 의미에 대해 알아본다.
- Step 2. 내 코드의 모든 발자취를 기록할 수 있을까?
  - Git과 GitHub의 차이에 대해 알고, 각각을 설치하며 시작해본다.
- Step 3. GitHub에 첫 번째 잔디 심기, 어렵지 않아요!
  - GitHub을 처음 사용하기 위해 로컬 저장소와 원격 저장소를 만들어본다.
  - add, commit, push, pull 등의 명령어를 직접 실습을 통해 사용해 보고, 그 필요성을 안다.
- Step 4. 데이터 분석계의 워드, Jupyter Notebook 마스터하기
  - Jupyter Notebook이 무엇인지, 어디에 쓰이는지 알아본다.
  - Jupyter Notebook의 사용법을 익힌다.
- Step 5. 개발자 문서작업의 시작과 끝! 마크다운을 익혀보자
  - 다양한 마크다운 문법을 알아보며 직접 실습을 통해 익혀본다.

---

## 노드 내용

- 개발자 필요 역량: 협업 능력
- 협업 툴/프로그램
  - Slack: 소통 앱
  - Notion: 문서작성 앱
  - Trello: To-Do 앱
  - HangOut / Zoom / Google Meet: 화상회의 앱
- 깃허브(GitHub)
  - 협업 및 버전관리 툴
  - 코드를 다루는 개발자들이 편리하게 협업하기 위한 도구
  - 가드닝(잔디 심기): 셀이 초록색으로 칠해졌다 => commit 했다
- 버전관리
  - [Git](https://git-scm.com/book/ko/v2)
    - 소스코드 업데이트 버전 기록/관리 => 소스코드 버전 관리 시스템
    - 로컬(Local)에서 작업한 내용을 Git이 저장
    - 명령어
      - init: 로컬저장소 초기화
      - add: 로컬저장소에 변화 기록하기 위한 준비 단계 => staging
      - commit: 로컬저장소에 변화 기록 => snapshot
      - push: 로컬저장소 기록을 원격저장소로 전송
      - clone: GitHub repository 통째로 복제해서 가져오기
      - pull: 원격저장소를 로컬저장소로 당겨오기
  - 웹사이트 기반으로 Git을 관리하는 온라인 서비스
    - GitHub: Git으로 관리하는 프로젝트를 호스팅하고, 시간과 공간의 제약 없이 협업할 수 있는 온라인 서비스
      - Git이 저장한 버전 기록을 다른 사람과 공유/협업
      - 로컬 작업 기록을 온라인 작업공간인 GitHub에 올려 원격(Remote)으로 작업할 수 있게 함
      - 파일을 따로 전송할 필요 없이 GitHub 에 Push or Pull 사용
      - README.md 파일
        - GitHub repository 의 소개 역할
        - Markdown File: 개발자가 가장 많이 사용하는 문서작업용 언어
    - GitLab
  - 정리
    - ![git_github.png](https://d3s0tskafalll9.cloudfront.net/media/images/F-3-12.max-800x600_j9vh5Fd.png)
    - ![using_github.png](https://d3s0tskafalll9.cloudfront.net/media/images/F-3-13.max-800x600_CwM2sTx.png)
- Jupyter Notebook
  - 데이터 클리닝과 변형, 통계 모델링, 머신러닝 등 데이터 분석을 편리하게 할 수 있도록 최적화 되어있는 오픈소스 웹 어플리케이션
  - 문서, 코드 작업을 동시에 진행할 수 있음
  - 셀 단위로 코드 실행
  - 모드
    - 입력 모드
    - 명령 모드
  - 셀
    - Markdown 셀: 제목/설명 작성
    - Code 셀: 실행시킬 코드 작성
- Markdown
  - md 확장자
  - 장점 : 간결하다, 별다른 도구 없이 작성할 수 있다.
  - 단점 : 표준이 없다, 모든 HTML 마크업을 대체하지 못한다.

---

## 요약

- Step 1. 개발자를 위한 첫 번째 필수 교양
  - 협업이 필수적인 개발자가 알아두면 좋을 여러 가지 협업 툴
- Step 2. 내 코드의 모든 발자취를 기록할 수 있을까?
  - 코드의 버전을 관리하는 Git, GitHub
- Step 3. GitHub에 첫 번째 잔디 심기, 어렵지 않아요!
  - 로컬 저장소, 원격 저장소
  - GitHub에 첫 번째 commit하기
- Step 4. 데이터 분석계의 워드, Jupyter Notebook 마스터하기
  - 개발 문서 작업에 가장 많이 쓰이는 주피터 노트북을 설치, 사용
- Step 5. 개발자 문서작업의 시작과 끝! 마크다운을 익혀보자
  - 개발자가 문서작업을 할 때 가장 많이 사용하는 마크다운 언어
