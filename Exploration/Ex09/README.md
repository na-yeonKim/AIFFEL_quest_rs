# AIFFEL Campus Online Code Peer Review Templete
- 코더 : 김나연
- 리뷰어 : 박희지


# PRT(Peer Review Template)
- [x]  **1. 주어진 문제를 해결하는 완성된 코드가 제출되었나요?**
    - 데이터 로드 → 전처리 → SentencePiece 토크나이징 → Dataset/DataLoader → Transformer 구현 → 학습 → 추론까지 전 과정이 끊김 없이 실행된 상태로 제출되었다.
    1) 한국어 전처리를 통해 학습 데이터셋을 구축하였다.
       - `Data Preprocessing` 셀에서 `str.strip()`으로 앞뒤 공백을 제거하고 `str.replace(r'\s+', ' ', regex=True)`로 연속 공백을 단일 공백으로 정규화, `drop_duplicates()`로 중복 쌍 제거 후 인덱스를 재설정하였다. 원본 11,823행 → 11,750행으로 감소한 것이 출력으로 확인되었다.
       - `max_length`를 임의로 정하지 않고 토큰 길이 분포(질문 평균 7.42 / 99% 15, 답변 평균 7.80 / 99% 16)를 먼저 측정한 뒤, `[16, 20, 24, 32]` 각각의 데이터 손실률(1.26% / 0.29% / 0.07% / 0.00%)을 비교하여 **20**을 선택하였다.
        <img width="500" alt="image" src="https://github.com/user-attachments/assets/620b6cda-e13a-4c2c-bd36-f13976aa6ce0" />
        <img width="500" alt="image" src="https://github.com/user-attachments/assets/91019f24-4c09-4594-b10c-14202b59fda6" />
    2) 트랜스포머 모델을 구현하여 한국어 챗봇 모델 학습을 정상적으로 진행하였다.
       - `PositionalEncoding`, `scaled_dot_product_attention`, `MultiHeadAttention`, `create_padding_mask`, `create_look_ahead_mask`, `EncoderLayer/Encoder`, `DecoderLayer/Decoder`, `Transformer`를 전부 직접 구현하였다. Residual + LayerNorm(eps=1e-6), 임베딩 `* sqrt(d_model)` 스케일링, look-ahead 마스크와 패딩 마스크의 `torch.max` 합성까지 논문 구조를 그대로 따랐다.
       - 25 epoch 학습 로그 전체가 노트북에 남아 있으며 발산·정체 없이 단조 수렴했다. Loss 7.0509 → 0.2089, Accuracy 0.2397 → 0.9534. Loss/Accuracy 곡선도 시각화되어 있다.
        <img width="500" alt="image" src="https://github.com/user-attachments/assets/cc7fddf5-0c55-4e52-a552-a4f6ffc4ad1d" />
        <img width="500" alt="image" src="https://github.com/user-attachments/assets/5b8237de-dbe8-4154-9a45-62b9dc9b5e30" />
    3) 한국어 입력문장에 대해 한국어로 답변하는 함수를 구현하였다.
       - `decoder_inference()`가 학습과 동일한 전처리(strip + 공백 정규화)를 재적용한 뒤 BOS부터 시작해 greedy decoding으로 토큰을 하나씩 이어붙이고, EOS를 만나면 중단하도록 올바르게 함수를 구현하였다.
       - 한국어 문장에 대해 모두 한국어 답변이 생성되었다.
        <img width="438" height="1128" alt="image" src="https://github.com/user-attachments/assets/167f621c-0581-4415-94fc-620e20223d55" />
        <img width="500" alt="image" src="https://github.com/user-attachments/assets/6a6f0368-569e-474d-ab80-2a7d81f944fc" />

    
- [x]  **2. 전체 코드에서 가장 핵심적이거나 가장 복잡하고 이해하기 어려운 부분에 작성된 
주석 또는 doc string을 보고 해당 코드가 잘 이해되었나요?**
    - 가장 핵심적인 부분은 마스크의 shape이 어떻게 만들어져서 어텐션 행렬에 적용되는가라고 생각한다. 헤드 분할이 들어간 텐서에서 마스크 차원이 하나만 어긋나도 에러 없이 잘못 학습되기 때문이다. 이 블럭에 shape 변화가 단계마다 주석으로 작성되어 있어서 이해하기 쉬웠다.
      <img width="500" alt="image" src="https://github.com/user-attachments/assets/e63c4ec9-f209-4fc7-9a49-7052a274bfc3" />
    - `ChatbotDataset.__init__()`의 번호 매긴 주석(1. 토크나이즈 → 2. Encoder input → 3. Decoder 전체 sequence → 4. 길이 제한 → 5. Teacher Forcing을 위해 먼저 shift → 6. Padding). 함수의 흐름을 주석으로 작성하여 코드를 따라가며 이해할 수 있었다.
      <img width="500" alt="image" src="https://github.com/user-attachments/assets/defae1e1-9313-4d4c-83bc-c27b485368b8" />
    - `Optimizer` 셀의 주석은 스케줄러 구현에서 실제로 틀리기 쉬운 함정을 짚어주어 집중해서 확인할 수 있었다.
      <img width="500" alt="image" src="https://github.com/user-attachments/assets/ca4de8a8-f15d-40fd-b7cc-14a4decdb636" />


        
- [x]  **3. 에러가 난 부분을 디버깅하여 문제를 해결한 기록을 남겼거나
새로운 시도 또는 추가 실험을 수행해봤나요?**
    - `max_length` 결정을 위한 정량 실험을 진행하였다. 값을 임의로 정하지 않고 토큰 길이 분포를 먼저 측정한 뒤, 후보값별 데이터 손실률을 비교하는 실험을 하였다. 손실률 0.29%를 감수하고 연산량이 작은 20을 선택했으며, 이후 Dataset size가 11,716(= 11,750 - 34)으로 나와 사전 계산과 실제 결과가 정확히 맞아떨어짐을 확인할 수 있다.
    
      <img width="469" height="333" alt="image" src="https://github.com/user-attachments/assets/e61ec94a-eef3-4b8f-8b67-32e1ee7018a8" />
    - 학습 종료 후 **미학습 문장 5개**와 **학습 데이터 질문 10개**를 나누어 추론하는 실험을 설계하였다. 후자에서 10개 전부 정답과 완전히 일치한다는 사실을 근거로, `Analysis` 섹션에서 "높은 Training Accuracy가 데이터 암기의 결과일 가능성이 있다"고 스스로 한계를 지적하였다. 지표를 좋게 포장하지 않고 검증 설계로 반증을 시도한 점이 좋았다.
     
      <img width="500" alt="image" src="https://github.com/user-attachments/assets/1cb7e079-f4f8-4626-b0e2-2aa3ca3d2452" />


        
- [x]  **4. 회고를 잘 작성했나요?**
    - 노트북 마지막 `# Analysis` 섹션에 실험 결과에 대한 분석과 자기 평가가 상세히 작성되어 있다.
    - 실험 개요에 Tokenizer, vocab size, MAX_LENGTH, batch size, layer 수, D_MODEL, head 수, FF dim, dropout, optimizer, LR 스케줄, epoch, 학습 시간(2분 59초)까지 재현에 필요한 설정을 넣어 다른 사람이 이 목록만 보고 동일 실험을 재현할 수 있도록 기록했다.
    - Loss와 Accuracy를 구간별로 나누어 해석했고(1~10 epoch 급감 구간, 10 epoch 이후 Accuracy 급상승), 수치를 표와 텍스트로 함께 제시하였다.
    - 학습 Accuracy가 높게 나왔는데 결과를 미화하지 않고 "개선이 필요한 부분" 4가지를 스스로 명시한 점이 좋았다.
     
      <img width="600" alt="image" src="https://github.com/user-attachments/assets/86ecb715-3c48-4706-bd5d-a0e4bc8feafb" />
      <img width="600" alt="image" src="https://github.com/user-attachments/assets/118b44c7-333b-4d83-9cdf-9787be9168a4" />


        
- [x]  **5. 코드가 간결하고 효율적인가요?**
    - Transformer 구성 요소를 `PositionalEncoding` / `scaled_dot_product_attention` / `MultiHeadAttention` / `create_padding_mask` / `create_look_ahead_mask` / `EncoderLayer` / `Encoder` / `DecoderLayer` / `Decoder` / `Transformer`로 계층적으로 분리하였다. 각 클래스가 `vocab_size`, `num_layers`, `d_model`, `num_heads`, `ff_dim`, `dropout`을 인자로 받아 하드코딩된 값이 없으므로 다른 하이퍼파라미터·다른 데이터셋에 그대로 재사용 가능하다.
    - `EncoderLayer`/`DecoderLayer`를 `nn.ModuleList` 컴프리헨션으로 쌓아 층 수 변경이 파라미터 하나로 끝났다. 층을 복붙하지 않아 코드가 간결하다.
     
      <img width="500" alt="image" src="https://github.com/user-attachments/assets/7c8aa608-acf9-44d3-b387-7aa8daafc9a8" />



# 회고(참고 링크 및 코드 개선)
```
테스트를 학습 데이터에 있는 문장과 없는 문장으로 나눠서 진행한 점이 인상 깊었습니다. 생성 결과를 그냥 나열하는 데 그치지 않고 "자연스러운 답변", "다소 어색하지만 관련성 있음", "문맥상 부자연스러움"처럼 등급을 매겨 정성 평가까지 진행하신 점도 배웠습니다. 나아가 학습 데이터 질문 10개가 모두 정답과 완전히 일치한 것을 근거로 높은 Accuracy가 과적합의 결과일 수 있다고 스스로 지적한 점도 좋았습니다.
```
