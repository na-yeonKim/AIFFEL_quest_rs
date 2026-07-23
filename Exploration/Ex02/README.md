# AIFFEL Campus Online Code Peer Review Templete
- 코더 : 김나연
- 리뷰어 : 박희지


# PRT(Peer Review Template)
- [x]  **1. 주어진 문제를 해결하는 완성된 코드가 제출되었나요?**
    - 아웃포커싱된 인물, 동물, 배경전환 크로마키 사진을 각각 1장씩 성공적으로 제작하였다.
      <img width="498" height="615" alt="image" src="https://github.com/user-attachments/assets/43bce300-cf72-4c13-a969-bc977af2330d" />
      <img width="556" height="547" alt="image" src="https://github.com/user-attachments/assets/10688b3c-9403-4b6f-a711-105b18e3929c" />
      <img width="548" height="545" alt="image" src="https://github.com/user-attachments/assets/635fe076-710f-4017-b777-e53671f1547a" />
    - 인물사진에서 블러 처리 되지 않는 부분을 명확히 표시한 사진을 제출하였다.
      <img width="344" height="451" alt="image" src="https://github.com/user-attachments/assets/4dc20936-ad25-4486-a71c-6a7834fdd8db" />
    - 문제점을 해결하는 방법으로 3가지를 제안하고, 방법 중 하나인 Morphology 적용한 결과물과 평가를 제출하였다.
      <img width="638" height="154" alt="image" src="https://github.com/user-attachments/assets/11350eca-d0b9-454b-a29b-7bbd535dedb0" /><img width="543" height="451" alt="image" src="https://github.com/user-attachments/assets/f543af60-026a-4614-b339-1d4b24b15681" />

    
- [x]  **2. 전체 코드에서 가장 핵심적이거나 가장 복잡하고 이해하기 어려운 부분에 작성된 
주석 또는 doc string을 보고 해당 코드가 잘 이해되었나요?**
    - 해당 코드 블럭을 왜 핵심적이라고 생각하는지 확인
    - 해당 코드 블럭에 doc string/annotation이 달려 있는지 확인
    - 해당 코드의 기능, 존재 이유, 작동 원리 등을 기술했는지 확인
    - 주석을 보고 코드 이해가 잘 되었는지 확인
        - 중요! 잘 작성되었다고 생각되는 부분을 캡쳐해 근거로 첨부
    <img width="543" height="451" alt="스크린샷 2026-07-23 171838" src="https://github.com/user-attachments/assets/73db4d65-496e-4a56-9591-995ca2fb2726" />  
    
    이 코드 블럭은 semantic segmentation mask의 오류를 보완할 수 있는 부분이므로 핵심적이라고 판단하였다. 주석으로 Morpholopy가 어떤 기법인지 설명하였고, 각 처리 단계마다 원리를 설명하는 주석을 달아 이해가 잘 되었다.

        
- [x]  **3. 에러가 난 부분을 디버깅하여 문제를 해결한 기록을 남겼거나
새로운 시도 또는 추가 실험을 수행해봤나요?**
    - 문제 원인 및 해결 과정을 잘 기록하였는지 확인
    - 프로젝트 평가 기준에 더해 추가적으로 수행한 나만의 시도, 
    실험이 기록되어 있는지 확인
        - 중요! 잘 작성되었다고 생각되는 부분을 캡쳐해 근거로 첨부
     
    - Morphology 후처리 기법을 추가로 적용하여 마스크의 품질을 개선하는 실험을 수행하였다. cv2.morphologyEx()를 사용해 객체의 경계를 자연스럽게 연결한 뒤, 블러 처리를 적용하여 기존 결과와 비교하였다
      <img width="543" height="445" alt="image" src="https://github.com/user-attachments/assets/874a44df-1952-4f3b-b10c-2696f46bddcc" />
      
        
- [x]  **4. 회고를 잘 작성했나요?**
    - 주어진 문제를 해결하는 완성된 코드 내지 프로젝트 결과물에 대해
    배운점과 아쉬운점, 느낀점 등이 기록되어 있는지 확인
    - 전체 코드 실행 플로우를 그래프로 그려서 이해를 돕고 있는지 확인
        - 중요! 잘 작성되었다고 생각되는 부분을 캡쳐해 근거로 첨부
     
    - Morphology 기법을 적용하는 데 그치지 않고, 후처리 기법의 한계에 대한 분석을 함께 기록하였다. 실험 결과를 바탕으로 기법의 효과를 평가하고, 성능 향상을 위해 다른 모델을 사용하는 것이 더 효과적일 수 있다는 방향을 제시하였다.
      <img width="818" height="68" alt="image" src="https://github.com/user-attachments/assets/4913c286-6bd2-40e4-b197-abc23c947e09" />

        
- [ ]  **5. 코드가 간결하고 효율적인가요?**
    - 파이썬 스타일 가이드 (PEP8) 를 준수하였는지 확인
    - 코드 중복을 최소화하고 범용적으로 사용할 수 있도록 함수화/모듈화했는지 확인
        - 중요! 잘 작성되었다고 생각되는 부분을 캡쳐해 근거로 첨부

    - 파이프라인이 함수화되지 않고 반복 작성되었다. 전처리 → 추론 → resize로 이어지는 동일한 로직이 변수명만 바뀐 채 여러 셀에 중복 구현되어 있다.

# 회고(참고 링크 및 코드 개선)
```
Semantic Segmentation의 한계를 보완하기 위한 다양한 솔루션을 확인할 수 있어 유익했다. 동일한 문제에 대해 나와 다른 접근 방식을 취한 사례를 살펴보며, 문제 해결에 여러 경로가 존재할 수 있음을 확인했고, 각 해결 방안이 어떤 논리로 문제를 풀어냈는지 파악할 수 있었다. 
```
