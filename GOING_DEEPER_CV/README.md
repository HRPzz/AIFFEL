# AIFFEL DAEGU 1 [![Hits](https://hits.seeyoufarm.com/api/count/incr/badge.svg?url=https%3A%2F%2Fgithub.com%2FHRPzz%2FAIFFEL%2Ftree%2Fmain%2FGOING_DEEPER_CV&count_bg=%2379C83D&title_bg=%23555555&icon=&icon_color=%23E7E7E7&title=hits&edge_flat=false)](https://hits.seeyoufarm.com)

## GOING DEEPER CV

|N|Node Title|Author|Type|Evaluation|Link|W|Open|Done|
|:---:|---|:---:|:---:|:---:|:---:|:---:|:---:|:---:|
|1|백본 네트워크 구조 상세분석<br><br>#Tags: DL, CV, Paper, ResNet, DenseNet, SENet, NasNet, EfficientNet|강상권|Lecture|✖️|[📋](Node_01/README.md)|12|03.15|✖️|
|2|없다면 어떻게 될까? (ResNet Ablation Study)<br><br>#Tags: DL, CV, ResNet, Ablation Study|강상권|Project|-|[📋](Node_02/README.md)|12|03.16|-|
|3|잘 만든 Augmentation, 이미지 100장 안 부럽다<br><br>#Tags: Data Augmentation, Tensorflow API, albumentation, GAN|강상권|Lecture|✖️|[📋](Node_03/README.md)|12|03.18|✖️|
|4|이미지 어디까지 우려볼까?<br><br>#Tags: |-|Project|-|[📋](Node_04/README.md)|13|03.21|-|
|5|너의 속이 궁금해 - Class Activation Map 살펴보기<br><br>#Tags: |-|Lecture|✖️|[📋](Node_05/README.md)|13|03.23|✖️|
|6|나를 찾아줘 - Class Activation Map 만들기<br><br>#Tags: |-|Project|-|[📋](Node_06/README.md)|13|03.24|-|
|7|Object Detection<br><br>#Tags: |-|Lecture|✖️|[📋](Node_07/README.md)|14|03.28|✖️|
|8|GO/STOP! - Object Detection 시스템 만들기<br><br>#Tags: |-|Project|-|[📋](Node_08/README.md)|14|03.29|-|
|9|물체를 분리하자! - 세그멘테이션 살펴보기<br><br>#Tags: |-|Lecture|✖️|[📋](Node_09/README.md)|14|03.31|✖️|
|10|도로 영역을 찾자! - 세그멘테이션 모델 만들기<br><br>#Tags: |-|Project|-|[📋](Node_10/README.md)|15|04.04|-|
|11|OCR 기술의 개요<br><br>#Tags: |-|Lecture|✖️|[📋](Node_11/README.md)|15|04.05|✖️|
|12|직접 만들어보는 OCR<br><br>#Tags: |-|Project|-|[📋](Node_12/README.md)|15|04.07|-|
|13|멀리 있지만 괜찮아<br><br>#Tags: |-|Lecture|✖️|[📋](Node_13/README.md)|16|04.11|✖️|
|14|멀리 있는 사람도 스티커를 붙여주자<br><br>#Tags: |-|Project|-|[📋](Node_14/README.md)|17|04.18|-|
|15|사람의 몸짓을 읽어보자<br><br>#Tags: |-|Lecture|✖️|[📋](Node_15/README.md)|17|04.19|✖️|
|16|행동 스티커 만들기<br><br>#Tags: |-|Project|-|[📋](Node_16/README.md)|17|04.21|-|

## GOING DEEPER CV CONTENTS SUMMARY

|N|Node Title|Author|Contents Summary|
|:---:|---|:---:|---|
|1|백본 네트워크 구조 상세분석|강상권|컴퓨터 비전 분야에 실전적으로 사용되는 주요 백본 네트워크(VGG, ResNet, SENet, EfficientNet) 구조에 대해 논문에 정리된 이론을 토대로 심화 원리를 학습한다.|
|2|없다면 어떻게 될까? (ResNet Ablation Study)|강상권|핵심적인 기법들을 하나씩 제거했을 때의 효과를 각각 정량적으로 측정하는 ablation study 기법을 배운다. ResNet을 대상으로 실습해 보면서 이론적으로 익힌 기법의 효과를 체감하고 백본을 직접 다뤄보는 실전적 감각을 익힌다.|
|3|잘 만든 Augmentation, 이미지 100장 안 부럽다|강상권|데이터셋이 부족한 상황을 해결하기 위한 Data Augmentation의 다양한 기법에 대해 알아보고, 활용 가능한 라이브러리, 실전상황에서 주의해야 할 팁 등을 정리해 본다.|
|4|이미지 어디까지 우려볼까?|-|텐서플로우의 random augmentation 기법을 적용 및 비교 실험|
|5|너의 속이 궁금해 - Class Activation Map 살펴보기|-|모델 작동 원리를 가늠할 수 있는 CAM, Grad-CAM, ACoL 모델 학습|
|6|나를 찾아줘 - Class Activation Map 만들기|-|CAM, Grad-CAM 모델 구현 및 CAM 추출 시각화|
|7|Object Detection|-|Object Detection 문제와 이를 해결하기 위한 다양한 모델 학습|
|8|GO/STOP! - Object Detection 시스템 만들기|-|Object Detection 모델을 활용하여 자동차, 사람 탐지(가까이 있는지 여부도 체크)|
|9|물체를 분리하자! - 세그멘테이션 살펴보기|-|세그멘테이션의 종류, 주요 모델, 평가 기준에 대해 학습|
|10|도로 영역을 찾자! - 세그멘테이션 모델 만들기|-|시맨틱 세그멘테이션을 이용하여 도로영역을 찾는 프로젝트 노드|
|11|OCR 기술의 개요|-|글자를 인식하는 기술인 OCR 구성요소 학습(Text detection & Text recognition)|
|12|직접 만들어보는 OCR|-|Text Recognition 모델을 구현 및 학습 → OCR 구현 프로젝트|
|13|멀리 있지만 괜찮아|-|Face Detection을 더욱 가볍고, 빠르고, 정확하개 개선할 수 있는 딥러닝 모델에 대한 학습|
|14|멀리 있는 사람도 스티커를 붙여주자|-|SSD 모델을 이용하여 이전 카메라 스티커 앱을 개선해보는 프로젝트|
|15|사람의 몸짓을 읽어보자|-|Human Pose Estimation 관련 논문 학습|
|16|행동 스티커 만들기|-|Simplebaseline 모델을 활용하여 Human Pose Estimation 구현|
