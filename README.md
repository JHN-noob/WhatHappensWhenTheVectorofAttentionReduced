### What happens when the vector of Attention reduced
- Multi Head Attention 잠재 벡터 차원 축소에 따른 Attention 점수와 계산 효율 변화 연구 
### Data Used
- Integer Delayed Copy Data
- seq_len: 0~99
- pad_token: 100
- delay_len: 20(pad_token)
### Experimental Conditions
- delay 20을 포함한 max_len 120의 Dataset 10,000개
- 30 Epochs
- 1e-4 LR
- Base Transformer, Variant Transformer
### Variant Transformer
- 기존 Multi Head Attention에서는 하나의 공유된 선형 변환을 통해 Q, K, V를 생성한 후 이를 8개의 헤드로 분할합니다.
반면 본 변형 모델에서는 입력을 가중치 학습 이전 단계에서 명시적으로 헤드별로 분리하여 각 헤드가 독립적인 가중치 공간에서 Attention을 학습하도록 설계하였습니다.
이러한 설계는 헤드 간 암묵적인 가중치 공유를 줄이고 각 헤드의 역할 분화를 유도하여 더 다양한 헤드별 특성을 보이도록 합니다.
또한 입력을 헤드별로 분할 후 각 헤드에 대해 독립적으로 Attention을 계산하므로, 개별 헤드가 처리하는 표현 차원은 d_model/h로 축소됩니다.
### Conclusion & Limitations
- 실험 결과 Base 모델과 Variant 모델 간의 loss 및 accuracy 차이는 학습 초기값 설정에 따라 큰 폭으로 달라졌으며 전반적으로 유의미한 성능 우위를 보이는 모델은 관찰되지 않았습니다.
이는 현재 학습 데이터에서는 두 모델이 유사한 표현력을 가지며 성능 측면에서는 구조적 차이가 결정적인 영향을 미치지 않음을 의미합니다.
반면 Attention Map을 비교한 결과 Variant 모델에서는 각 Attention Head 간의 패턴이 다양하게 나타났으며 이는 헤드별 기능 분화가 Base 모델에 비해 더 뚜렷하게 이루어졌음을 보여줍니다.
이러한 결과는 Variant 구조가 헤드간의 암묵적인 중복을 줄이고 역할 특화를 유도하는 구조적 효과를 가질 수 있음을 시사합니다.
향후 연구에서는 본 실험이 국소적인 토이 데이터 환경에서 수행되었다는 점을 고려하여 시퀀스 길이와 데이터 규모를 확장하여 두 모델의 분화 양상이 어떻게 변화하는지 분석할 필요가 있을 것으로 생각됩니다.
