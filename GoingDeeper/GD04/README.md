# AIFFEL Campus Online Code Peer Review Templete

- 코더 : 김나연
- 리뷰어 : 강지수


# PRT(Peer Review Template)

- [x] **1. 주어진 문제를 해결하는 완성된 코드가 제출되었나요?**
    - MPII 데이터 준비와 전처리, Stacked Hourglass 및 SimpleBaseline 구현, 동일한 학습 설정에서의 3 epoch 학습, validation loss 비교, 테스트 이미지 pose 예측, PCK 기반 관절별 정량 비교까지 프로젝트의 전체 흐름이 구현되어 있습니다.
    - 특히 단순히 두 모델의 loss만 비교하지 않고, `PCK@0.05`를 추가하여 Hourglass 0.5255 / SimpleBaseline 0.4245의 결과와 관절별 차이까지 확인한 점이 좋았습니다.
    - 근거: `pose_estimation.ipynb`의 **simplebaseline → 모델 비교 → PCK@0.05 → 결과 해석** 부분

- [x] **2. 전체 코드에서 가장 핵심적이거나 가장 복잡하고 이해하기 어려운 부분에 작성된 주석 또는 doc string을 보고 해당 코드가 잘 이해되었나요?**
    - 네. Hourglass와 SimpleBaseline 모두 tensor shape 변화와 각 블록의 역할이 주석으로 상세히 설명되어 있어 구조를 따라가기 좋았습니다.
    - 특히 SimpleBaseline에서 `ResNet50 → (B, 2048, 8, 8) → Deconv 3회 → (B, 256, 64, 64) → 16개 heatmap`으로 이어지는 과정이 코드 옆에 명시되어 있어, 백본에서 압축한 공간 정보를 다시 관절 heatmap 해상도로 복원하는 흐름을 이해하는 데 도움이 되었습니다.
    - `extract_keypoints_from_heatmap()`에서도 단순 argmax 후 3×3 주변 값을 이용해 ±0.25 단위로 위치를 보정하는 이유를 quantization 오차와 연결해서 설명한 점이 인상적이었습니다.

- [x] **3. 에러가 난 부분을 디버깅하여 문제를 해결한 기록을 남겼거나 새로운 시도 또는 추가 실험을 수행해봤나요?**
    - 추가 실험이 분명하게 확인됩니다. 기본 요구사항인 loss와 pose 시각화 비교를 넘어서 `compute_pck()`를 직접 구성하여 전체 PCK와 16개 관절별 PCK를 비교했습니다.
    - 또한 결과 해석에서 Hourglass는 4개 stack loss를 합산하고 SimpleBaseline은 단일 output loss를 사용하므로 **두 loss의 절대값을 직접 비교하면 안 된다**고 짚은 점이 좋았습니다. 단순히 낮은 loss를 더 좋은 모델이라고 결론내리지 않고 지표 정의를 먼저 살펴본 점이 연구적으로 좋았습니다.

- [ ] **4. 회고를 잘 작성했나요?**
    - 마지막에 Validation Loss, PCK, 정성적 결과, 한계까지 정리한 **결과 해석**은 매우 잘 작성되어 있습니다.
    - 실행 흐름은 notebook 중간의 텍스트 다이어그램으로 잘 정리되어 있으므로, 마지막에 짧은 회고만 추가되면 이 항목도 충분히 충족될 것 같습니다.

- [ ] **5. 코드가 간결하고 효율적인가요?**
    - `MPIIDataset`, `Preprocessor`, `Trainer`, `SimpleBaseline`, `compute_pck()` 등 주요 책임을 클래스와 함수로 분리한 점은 좋았고, 모델 교체만으로 같은 학습 엔진을 재사용할 수 있도록 만든 점도 깔끔했습니다.


# 회고(참고 링크 및 코드 개선)

나연님의 GD04를 보면서 제가 진행한 프로젝트와 같은 Hourglass vs SimpleBaseline 비교라도 평가를 어떻게 설계하느냐에 따라 배울 수 있는 내용이 달라진다는 점이 흥미로웠습니다.

특히 저는 heatmap loss 외에 pixel-distance 기반 위치 오차를 추가했는데, 나연님은 PCK를 직접 구현해 관절별 성능까지 비교했습니다. 두 방식 모두 결국 **“heatmap loss가 낮은가?”에서 끝나지 않고 “실제 관절 위치를 얼마나 잘 찾았는가?”를 확인하려는 시도**라는 점에서 연결되어 보여 복습에 도움이 되었습니다.

가장 좋았던 부분은 SimpleBaseline의 validation loss가 훨씬 낮게 보여도 Hourglass와 loss 계산 방식 자체가 다르기 때문에 직접 비교하면 안 된다고 해석한 부분입니다. 결과 숫자를 그대로 받아들이기보다 **지표가 어떻게 계산됐는지를 먼저 확인해야 한다**는 점을 다시 생각하게 되었습니다.

전체적으로 모델 구조와 추론 과정의 주석이 자세해서 코드를 따라가며 Pose Estimation 흐름을 복습하기 좋았습니다.
