# AIFFEL Campus Online Code Peer Review Templete
- 코더 : 김나연
- 리뷰어 : 김시온

# PRT(Peer Review Template)
- [x]  **1. 주어진 문제를 해결하는 완성된 코드가 제출되었나요?**
    - 문제에서 요구하는 최종 결과물이 첨부되었는지 확인
        - 중요! 해당 조건을 만족하는 부분을 캡쳐해 근거로 첨부

요구하는 조건은 다음과 같았다.
1) 한글 코퍼스를 가공하여 BERT pretrain용 데이터셋을 잘 생성하였다.
2) 구현한 BERT 모델의 학습이 안정적으로 진행됨을 확인하였다.
3) 1M짜리 mini BERT 모델의 제작과 학습이 정상적으로 진행되었다.

(1) 잘 생성하였다.

<img width="408" height="282" alt="image" src="https://github.com/user-attachments/assets/0f8d78cc-6a4a-41f7-a473-11693d71dedd" />

(2) 안정적으로 학습이 진행됨을 확인했다.

<img width="627" height="161" alt="image" src="https://github.com/user-attachments/assets/c42b6300-baf2-4d81-ab3e-b7f30de4d10f" />

(3) 1M짜리 조건에 부합하면서 학습이 진행됨을 확인하였다.

<img width="257" height="82" alt="image" src="https://github.com/user-attachments/assets/13eb2369-b8fe-489e-9557-35b13577240d" />

    
- [x]  **2. 전체 코드에서 가장 핵심적이거나 가장 복잡하고 이해하기 어려운 부분에 작성된 
주석 또는 doc string을 보고 해당 코드가 잘 이해되었나요?**
    - 해당 코드 블럭을 왜 핵심적이라고 생각하는지 확인
    - 해당 코드 블럭에 doc string/annotation이 달려 있는지 확인
    - 해당 코드의 기능, 존재 이유, 작동 원리 등을 기술했는지 확인
    - 주석을 보고 코드 이해가 잘 되었는지 확인
        - 중요! 잘 작성되었다고 생각되는 부분을 캡쳐해 근거로 첨부
     
-> 가장 복잡하다고 생각이 되는 create_pretain_instances 부분에 설명이 잘 달려있다.

<img width="507" height="158" alt="image" src="https://github.com/user-attachments/assets/cbd0fe86-380f-42ae-be7e-882e1e69aa61" />

        
- [x]  **3. 에러가 난 부분을 디버깅하여 문제를 해결한 기록을 남겼거나
새로운 시도 또는 추가 실험을 수행해봤나요?**
    - 문제 원인 및 해결 과정을 잘 기록하였는지 확인
    - 프로젝트 평가 기준에 더해 추가적으로 수행한 나만의 시도, 
    실험이 기록되어 있는지 확인
        - 중요! 잘 작성되었다고 생각되는 부분을 캡쳐해 근거로 첨부

-> 1차실험 후, 전체 데이터와 early stopping을 도입한 2차실험을 수행하여 추가실험을 하였다.

<img width="605" height="65" alt="image" src="https://github.com/user-attachments/assets/e4ca4b34-3a68-4594-9a48-3fb3201e4a64" />

        
- [x]  **4. 회고를 잘 작성했나요?**
    - 주어진 문제를 해결하는 완성된 코드 내지 프로젝트 결과물에 대해
    배운점과 아쉬운점, 느낀점 등이 기록되어 있는지 확인
    - 전체 코드 실행 플로우를 그래프로 그려서 이해를 돕고 있는지 확인
        - 중요! 잘 작성되었다고 생각되는 부분을 캡쳐해 근거로 첨부

-> 회고는 작성되어있는것 같지는 않지만, 정량적/정성적 결과를 비교하여 결과를 분석한 내용이 있다.

<img width="637" height="435" alt="image" src="https://github.com/user-attachments/assets/7f5813ef-8b1d-4b56-9db8-3d3bdb017f62" />


        
- [x]  **5. 코드가 간결하고 효율적인가요?**
    - 파이썬 스타일 가이드 (PEP8) 를 준수하였는지 확인
    - 코드 중복을 최소화하고 범용적으로 사용할 수 있도록 함수화/모듈화했는지 확인
        - 중요! 잘 작성되었다고 생각되는 부분을 캡쳐해 근거로 첨부

-> 생성과 로딩 함수를 분리해 재사용성을 높였다.
     
<img width="463" height="96" alt="image" src="https://github.com/user-attachments/assets/b73f255c-9637-4fae-b9d8-00743b88729d" />
<img width="397" height="67" alt="image" src="https://github.com/user-attachments/assets/4ca76901-abc6-45c6-9e1b-ef14a3d12f64" />


# 회고(참고 링크 및 코드 개선)
```
# 리뷰어의 회고를 작성합니다.
# 코드 리뷰 시 참고한 링크가 있다면 링크와 간략한 설명을 첨부합니다.
# 코드 리뷰를 통해 개선한 코드가 있다면 코드와 간략한 설명을 첨부합니다.
```
김시온 : 모든 조건에 부합하여 코드가 작성되어있다.
