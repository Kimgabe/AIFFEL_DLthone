# AIFFEL_DLthone
- 지난 10월부터 아이펠에서 공부한 시간 약 1859시간!
- 그동안 배운내용을 바탕으로 진행하는 첫 팀프로젝트겸 헤커톤입니다.
- Kaggle의 [Jellyfish 데이터셋](https://www.kaggle.com/datasets/anshtanwar/jellyfish-types) 을 사용해서 다중 클래스 분류기 만들기
- [프로젝트 대시보드 구경가기](https://www.notion.so/gabesoon/DL_Thon-Jellyfish-465fe4892d90436b9a1ef64ed3991895?pvs=4)
---
# 📌 작업순서에 따른 노트북 읽는 순서 안내
![process](https://github.com/Kimgabe/AIFFEL_DLthone/assets/74717033/93205692-a703-4838-9ef4-1c3e0eb57f0f)
> 💡 이번 DLthone에서 우리팀의 목표는 소규모의 데이터셋으로도 빠르고 우수한 성능을 구현할 수 있는 **Transfer Learning 모델의 성능을 직접 구축한 모델로 최대한 따라잡는 것이었습니다.**
> 이름 감안하여 프로젝트 수행과정을 살펴봐 주시면 감사드리겠습니다 😀

---

1. [CNN모델링](https://github.com/Kimgabe/AIFFEL_DLthone/tree/main/CNN_Model)
   - 설명
     - 데이터셋에 대한 이해 및 모델링, 성능향상을 위한 튜닝의 1 circle에 대한 이해도를 높이기 위해 각자 CNN모델을 생성하고 최적화 하는 작업 수행(1/10)
     - 3명이 각각 CNN이라는 같은 아키텍처의 모델에 서로 다른 모델 구조를 구성해 버전별로 최적화룰 수행

2. [ConvMixer 1차시도](https://github.com/Kimgabe/AIFFEL_DLthone/blob/main/ConvMixer_Model/%5BSeonjae%20Lee%5D%20Jellyfish_ConvMixer_Base_Model_data_extention.ipynb)
   - 설명
     - CNN 최적화 작업 1 circle을 통해 작업과정에 대한 이해도를 동일한 수준으로 맞춘 뒤, 데이터셋의 한계를 해결하기위해 ConvMixer 모델을 도입
     - 1차 시도에서는 ConvMixer의 모델 특성을 살려 적은 데이터셋으로 성능향상이 이뤄질 것을 기대하였으나, 기대 이하의 성능을 보임
       - 데이셋의 한계
         1) 전체적으로 부족한 데이터 볼륨 (총 979장)
         2) 학습데이터(900장) 🔛 검증(39장) & 테스트(40장) 데이터간의 Imbalance

 3. [ConvMixer 2차 시도](https://github.com/Kimgabe/AIFFEL_DLthone/blob/main/ConvMixer_Model/%5BKimgabe%5D%20Jellyfish_ConvMixer_basic_model.ipynb)
    - 설명
      - ConvMixer의 특성상 적은 데이터셋에도 안정적인 성능을 기대했지만, 너무나 적은 데이터로 인해 과적합에 빠지는 문제 발생
      - 문제 해결을 위해 상대적으로 볼륨이 풍부한 학습데이터만을 사용한 Kfoldout을 적용한 Cross Validation으로 모델의 일반화 능력을 키우면서 성능 향상을 추구
      - 기존 모델(Test Accuracy: 27.459 %) 👉 Kfold 적용한 모델 (Test Accuracy: 73.333%) 로 **Dramatic한 모델성능 향상 달성**
 
4. [데이터 추가 확보](https://github.com/Kimgabe/AIFFEL_DLthone/tree/main/Data_collection)
   - 설명
     - ConvMixer의 성능 개선에는 성공하였지만, 목표 성능(85%이상)에는 부족한 상황
     - 근본적으로 데이터셋이 가진 한계를 극복하기 위해 추가적인 데이터확보를 하기로 내부회의로 결정
     - 데이터 확보를 위해 구글링을 통해 Jellyfish에 대한 데이터를 제공하는 사이트 2곳을 확보하여 추가데이터 확보
       - [링크1](https://zenodo.org/records/3545785), [링크2](https://images.cv/download/jellyfish/2457/CALL_FROM_SEARCH/%22jellyfish%22)
     - 이외에 구글 이미지 검색을 통해 클래스별로 평균 350장의 이미지를 [크롤링](https://github.com/Kimgabe/AIFFEL_DLthone/blob/main/Data_collection/%5BKimgabe%5D%20%EA%B5%AC%EA%B8%80%20%EC%9D%B4%EB%AF%B8%EC%A7%80%20%ED%81%AC%EB%A1%A4%EB%A7%81%20%EC%BD%94%EB%93%9C.ipynb)하여 수집

1. [추가확보한 데이터 EfiicientNet 으로 라벨링](https://github.com/Kimgabe/AIFFEL_DLthone/blob/main/Data_collection/%5BKimgabe%5D_99_acc_%EC%A0%84%EC%9D%B4%EB%AA%A8%EB%8D%B8%EB%A1%9C_%EC%99%B8%EB%B6%80%EB%8D%B0%EC%9D%B4%ED%84%B0_%EB%9D%BC%EB%B2%A8%EB%A7%81%ED%95%98%EA%B8%B0.ipynb)
   - 설명
     - 확보된 데이터의 경우 별도의 라벨링이 되어있지 않아 즉각 사용이 어려운문제 발생
     - 사전 모델 선정 과정에서 조사시 99%성능으로 우수한 성능을 보여준 Pre trained 모델인 EfficientNet_v2 를 사용해추가확보 데이터의 라벨링작업을 수행
     - 모델의 오류를 고려하여 Threshold를 95%로 설정하여 모델이 예측한 결과가 95%이상인 이미지만 해당 클래스로 분류하고 데이터셋으로 확보
       
2. [추가한 데이터셋에 대한 처리 및 데이터셋 구축](https://github.com/Kimgabe/AIFFEL_DLthone/blob/main/Data_collection/%5BKimgabe%5D%20%EC%9B%90%EB%B3%B8%20%EB%8D%B0%EC%9D%B4%ED%84%B0%20%2B%20%EC%B6%94%EA%B0%80%EB%8D%B0%EC%9D%B4%ED%84%B0%20%EB%B3%91%ED%95%A9.ipynb)
   - 설명
     - DLThone 노드에서 강조했던 Train_val_test 폴더의 사전에 분류된 데이터셋을 가급적 유지하고자 함
     - 특히 원본 validation, test 데이터가 Train데이터로 유출되어 과적합되는 상황이 발생하지 않도록 독립적으로 처리
       
3. [추가데이터를 활용한 CNN 성능 업그레이드 시도](https://github.com/Kimgabe/AIFFEL_DLthone/blob/main/CNN_Model/%5BKimgabe%5D%20Jellyfish_CNN_added_data_bestmodel.ipynb)
   - 설명
     - 원본 데이터셋으로 가장 좋은 성능을 보였던 CNN모델에 추가데이터를 적용하여 모델 성능 향상 시도
     - 기존 모델의 경우 W&B 의 Sweeper 를 활용한 하이퍼파라미터 튜닝으로 얻은 최대 성능(Test Accuracy 70%) 을 개선한 성능(Test Accuracy 75.82%) 달성
     - 데이터셋이 가졌던 한계를 추가 데이터로 어느정도 극복하였음을 확인
       
4. [CNN 앙상블을 통한 모델 일반화 및 과적합 해소 시도](https://github.com/Kimgabe/AIFFEL_DLthone/blob/main/Ensembles/%5BEnsemble%5D%20CNN%EB%AA%A8%EB%8D%B8%EC%9D%84%20%ED%99%9C%EC%9A%A9%ED%95%9C%20%EB%B0%B0%EA%B9%85(%EC%95%99%EC%83%81%EB%B8%94).ipynb)
   - 설명
     - 추가된 데이터로 성능이 향상된 기존 3개의 독립된 CNN모델을 앙상블(배깅)하여 모델의 일반화 능력을 키움과 동시에 과적합문제를 완화하고자 함
     - 이는 기존 모델 학습시 학습데이터(900장) 🔛 검증(39장) & 테스트(40장) 데이터간의 Imbalance로 인해 학습데이터에 과적합되는 현상이 지속적으로 관찰되었기 때문
     - 앙상블 모델과 개별모델의 성능지표를 비교한 결과 앙상블 모델의 정확도는 기존 CNN모델과 비슷한 수준을 보였으나, 모델 일반화 능력과 과적합 방지 부분에서 유의미한 평가 결과를 확인할 수 있었음
       
5. [Tranfer Learning을 사용한 모델 구현](https://github.com/Kimgabe/AIFFEL_DLthone/tree/main/Pre-Trained_Model)
   - 설명
      - 목표로 했던 사전학습 모델들을 직접 구현하여 직접 구축한 모델들과의 성능을 비교 평가
      - 이 과정의 목표는 전이학습 모델을 구현하는 과정을 직접 실행해보고 활용하는 방법을 학습하는 것이었음
          

## 일정
### Day 01 / 01.10(수)

- 주제 공개 및 가이드라인 안내
- **팀 빌딩**
- 팀장 선정
- DLthon 진행

### Day 02 / 01.11(목)

- DLthon 진행

### Day 03 / 01.12(금)

- 오전 10:00까지 제출
- 발표
- 회고
---
## 팀원 소개
- 팀장 : [김승순](https://github.com/Kimgabe/) 
- 팀원
  - [이선재](https://github.com/thetjswo)
  - [한현종](https://github.com/hjhan1201)
---

### 평가항목

1. 데이터 EDA와 데이터 전처리가 적절하게 이뤄졌는가?
2. Task에 알맞게 적절한 모델을 찾아보고 선정했는가?
3. 성능향상을 위해 논리적으로 접근했는가?
4. 결과 도출을 위해 여러가지 시도를 진행했는가?
5. 도출된 결론에 충분한 설득력이 있는가?
6. 적절한 metric을 설정하고 그 사용 근거 및 결과를 분석하였는가?
7. 발표가 매끄럽게 진행되었고 발표시간을 준수하였는지?

---

### 평가 루브릭

| 평가문항 | 상세기준 |
| ---- | ---- |
| 1. 적합한 로스와 메트릭을 사용하여 훈련이 이루어졌는가? | 데이터셋 구성, 모델 훈련, 결과물 시각화의 한 사이클이 정상적으로 수행되어 테스트 결과를 출력하였다. |
| 2. 두 가지 이상의 차이점을 두어 비교가 이루어졌는가? | 선택한 모델의 훈련에 필요한 하이퍼 파라메터들의 수치별 성능과 비용의 비교분석을 진행하였다. |
| 3. 훈련 결과 및 제품화 가능성에 대한 탐색이 이루어졌는가? | 데이터와 모델에 대한 구성, 훈련 비용, 성능, 제품화 가능성, 응용분야와 강점, 개선사항 등에 대한 정량적 및 정성적 분석을 진행하였다. |
