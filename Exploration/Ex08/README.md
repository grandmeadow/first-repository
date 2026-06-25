# AIFFEL Campus Online Code Peer Review Templete
- 코더 : 강성진
- 리뷰어 : 장주휘


# PRT(Peer Review Template)
- [ ]  **1. 주어진 문제를 해결하는 완성된 코드가 제출되었나요?**
    - Stanford Dogs 데이터 전처리, 모델 로드, CAM/Grad-CAM 생성, IoU 계산 및 시각화까지 실행 완료됨.
    -  4가지 IoU 스코어가 정상 출력된 것을 확인함
        - <img width="1100" height="1192" alt="image" src="https://github.com/user-attachments/assets/65467fd2-cebd-4e1d-bf90-14c19dab0fdb" />

    
- [ ]  **2. 전체 코드에서 가장 핵심적이거나 가장 복잡하고 이해하기 어려운 부분에 작성된 
주석 또는 doc string을 보고 해당 코드가 잘 이해되었나요?**
    - 모델이 예측한 히트맵을 바탕으로 Object Detection 평가가 가능하도록 Bounding Box를 역산하는 cam_to_bbox 함수와, 최종 시각화를 위해 텐서를 이미지로 되돌리는 역정규화 로직이 프로젝트의 핵심 연산이라고 생각함
    - 각 함수 시작 부분에 CAM/Grad-CAM 히트맵에서 ... 추출 구조로 doc string이 명확히 작성되
    - display_original_sample 함수에서 PyTorch 텐서 변환(permute), ImageNet 정규화 역산, np.clip 가 필요한 이유가 주석으로 상세히 기술됨
    - 주석에 번호가 매겨진 설명 덕분에 쉽게 이해할 수 있었음
        - <img width="1170" height="742" alt="image" src="https://github.com/user-attachments/assets/52284e22-89f1-41e8-b5cc-ad4c342ebfed" />
        
    <img width="1012" height="786" alt="image" src="https://github.com/user-attachments/assets/880481f0-56e6-4601-9141-77b48ccd4a72" />


        
- [ ]  **3. 에러가 난 부분을 디버깅하여 문제를 해결한 기록을 남겼거나
새로운 시도 또는 추가 실험을 수행해봤나요?**
    - 에러 디버깅 과정을 적은 직접적인 텍스트 기록은 없으나, XML 파싱 중 예외가 발생할 경우 프로그램이 멈추지 않도록 try-except 문을 배치하여 에러 가드를 쳐둔 흔적이 있음
    - 단일 모델 테스트에 그치지 않고 ResNet-50과 ResNet-101 두 가지 백본, 그리고 CAM과 Grad-CAM 두 가지 알고리즘을 교차 결합한 총 4가지 조합의 크로스 매트릭스 비교 실험을 스스로 설계하고 구현함
        - <img width="690" height="600" alt="image" src="https://github.com/user-attachments/assets/0e530fc7-c3ee-4746-94e1-62cfe59838f8" />
        <img width="696" height="296" alt="image" src="https://github.com/user-attachments/assets/bf26af81-ca5f-4d80-90c1-8af7b29b6d21" />


        
- [ ]  **4. 회고를 잘 작성했나요?**
    - 코드 셀 실행 결과로 시각화와 로그가 잘 적혀있음
        
- [ ]  **5. 코드가 간결하고 효율적인가요?**
    - 변수명, 함수명 규칙(snake_case) 및 들여쓰기가 깔끔하며 전체적인 가독성이 높
    - 핵심 연산들이(IoU 계산, 박스 변환, CAM 추출 등) 독립된 함수 및 클래스로 모듈화되어 있어 다른 데이터셋에도 바로 재사용할 수 있을 만큼 범용성이 뛰어남      
        -  <img width="1086" height="722" alt="image" src="https://github.com/user-attachments/assets/36262493-56f5-4213-8ae4-c49f20cc5c62" />
        <img width="654" height="516" alt="image" src="https://github.com/user-attachments/assets/0e53ace2-259e-4f60-a425-ba91659a9a4a" />




# 회고(참고 링크 및 코드 개선)
```
# 리뷰어의 회고를 작성합니다.
1. 인상깊은 실험 스토리
- 일반적으로는 backbone 모델이 깊어질 수록 성능이 향상되는 것이 일반적임. 모델이 더 깊어지면, 모델의 IoU Score도 더 정교해질 것이다라는 가설을 세우고 크로스 매트릭스 실험을 설계하심
- 하지만 실험 결과에서는 오히려 ResNet-50 조합이 IoU 0.0951로, ResNet-101 조합(IoU 0.0454)보다 더 높은 점수를 기록해버림..!
- 깊은 모델일수록 해상도가 줄어들며 국소적인 특징에 과하게 집중하거나, 레이어가 깊어짐에 따라 그래디언트 소실/왜곡이 발생해 히트맵이 다소 뭉개질 수 있다는 딥러닝의 고유한 특성을 직접 눈으로 확인할 수 있는 뜻깊은 시도였다고 생각함
```
