Basic building block으로 Swin Transformer 사용
- window-based local attention
- Conv-based GAN에서는 zero-padding으로 절대 위치를 추론하지만 , window-based transformers에는 그런 feature가 소실됨.
- Block단위로 attention을 계산하면 공간적 긴밀성을 깨트림.
- 고해상도 단계에서 MLP를 줄이면 고화질에서 세부정보를 합성할 수 없음 (e.g. HiT)


<Style Injection>
- AdaNorm: normalization후의 feature map의 mean과 std
- Modulated MLP: feature map을 조절하는 대신, linear layer의 weight를 바꿀 수 있다.
                 network의 channel-wise weight 크기를 바꿈 -> 더 빨라짐
- Cross-attention: transformer-specific style injection
결과: stylegan information을 두번 사용하는 효과이기때문에 AdaNorm이 가장 좋음 ---- why?
  
  
<Double Attention>
<Local-global positional encoding>
- Swin Transformer에서 가져온 relative positional encoding (RPE) - 픽셀의 상대위치를 인코딩함.
!!! 절대 위치는 Conv에게 중요함. 왜? 입과 같은 특정 구성요소가 공간 좌표에 매우 의존하기 때문.
이 절대 위치를 사용하지 않는 relative positional encoding -> 이러한 정보를 잃어버리게 됨.
따라서, sinusoidal position encoding (SPE)를 각 스케일에 적용. (horizontal, vertical feature maps가 encoding에 추가됨)
absolute positional encoding보다 SPE를 선호하는데, translation invariance를 허용하기 때문.
=> RPE는 각 transformer block에 적용되어 local context에,
   SPE는 각 scale에 적용되어  global position에 영향을 줌.
