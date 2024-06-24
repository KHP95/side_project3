# 고기의 선도를 분류하기 위한 CNN기반 경량 분류모델 개발
- 고기의 사진 약 4700장을 학습하여 신선, 주의, 상함 3가지 클래스로 분류하는 모델
- tensorflow를 기반으로 진행
- 학습은 코랩에서 V100으로 진행 => 학습된 모델을 불러와서 검증 및 평가 
- task가 간단하고 데이터 갯수가 적기 때문에, ResNet, EfiNet같은 기존 backbone 사용 x
    - 파라미터가 매우 작은 커스텀 CNN 구성
- 처음 모델은 pre-activation과 bottleneck을 적용한 커스텀 Residual Block을 쌓아서 구성
    - 해당 블록을 5단 쌓아서 backbone 구성. 이후 풀링하고 FC 통과시켜 분류
    - tensorflow 콜백을 활용, 성능개선 없을시 1차로 학습률 감소, 2차로 조기종료
    - 파라미터 수 102K로 ResNet, EfiNet의 backbone에 비해 매우 가벼움
    - 검증 accuracy 0.985 확보
- 성능개선을 위해 Inverted bottleneck로 변경, depthwise-conv로 변경, SEblock 추가
⇒ MBconv 블록과 유사
    - 해당 블록을 7단 쌓아서 backbone구성 (Top은 처음 모델과 동일)
    - 파라미터는 109K로 약 3% 증가
    - 검증 accuracy 0.996 으로 1.1%p 증가
      
- **첫 번째 모델 블록 및 backbone 구성**  
  ![customv1_1](https://github.com/KHP95/side_project3/assets/124794057/691baabd-71e4-44f9-8620-a906c00216d3)
  ![customv1_2](https://github.com/KHP95/side_project3/assets/124794057/f01e37fc-2235-46f9-972e-176cf9a625e8)
  <br>
- **두 번째 모델 블록 및 backbone 구성**  
  ![customv2_1](https://github.com/KHP95/side_project3/assets/124794057/01e8b9cb-b079-4fa4-9541-87577d326269)
  ![customv2_2](https://github.com/KHP95/side_project3/assets/124794057/4800a306-97d7-4aa1-a615-1ba3a0df1ad7)
  <br>
- **첫 번째 모델(V1) 및 두 번째 모델(V2) 훈련 기록**  
  ![train_res](https://github.com/KHP95/side_project3/assets/124794057/15275f07-f699-447d-a41d-a9d17e05a2ba)
  <br>
- **모델 스펙**  
  ![model_spec](https://github.com/KHP95/side_project3/assets/124794057/af2c064b-ae45-4254-9c9d-21b695b2e48b)
