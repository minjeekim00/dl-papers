# Abstract
- Diffusion model -> iterative refinement를 활용한 생성 모델
- -> 물리적 현상이 "연속적(continous)"이라는 사실에 의거함
- -> discrete 하거나 caterofical한 데이터는 어떻게 학습할것인가? 
- Time과 input space 모두가 continuous한 diffusion model을 이용하여 categorical data 모델링. 
- <b>!!!Discrete token을 Euclidean space에 embedding하여 continous 모델을 사용 가능하게함 !!!</b>


#  Introduction
- Diffusion-based language model은 language의 textual representation이 discrete categorical하기 때문에 힘들다.

### CDCD Framework
- Transformer: noise embedding으로부터 token 예측 (attention masking x) -> Cross-entropy
- Input noise: time warping을 통해 non-uniformly sampled t에 time-dependent
- Cross-entropy logit을 사용하여 ODE solver로 샘플링하는 데 사용되는 interpolation을 통해 score function estimate을 얻을 수 있다.

### Contributions
- Score interpolation 사용 -> cross-entropy 사용가능.  
  -> diffusion model + Euclidean embedding을 single loss로 합치는 end-to-end 학습 가능
- Time warping: 효율성을 극대화하기 위해 학습 중에 샘플링된 noise level distribution을 자동으로 조정하는 active learning


# Deffusion models
- # Diffusion for discrete data
1. 비슷한 iterative refinement 과정을 정의한다.
2. 연속 space에 embedding하여 embedding에 continous diffusion을 적용한다. => 후자 이용
- 이점 1: classifier-free guidance 등 특성 이용가능
- 이점 2: 샘플링 프로세스의 중간 단계에서 가능한 결과의 중첩(superposition)을 나타낼 수 있음
  => 각 token 레벨의 uncertainty를 나타낼 수 있음을 의미. 샘플링 절차는 마지막에 특정 토큰에만 커밋됨(?)
  => Discrete input space로 바로 작동하면 이런 능력이 없음. 특정 token만을 표현하거나 decision의 존재만 알 수 있음
  토큰의 일부에만 조기에 커밋해야 하는 이 요구 사항은 결과 샘플의 불일치로 이어질 수 있으며, 이는 소급하여 수정하기 어려움 (??)
  
- Input representation을 연속적으로 바꾸는 것은 어려울 수 있음.
- 실제로 가장 일반적인 모델링 설정은 likelihood 모델에 비해 고주파 콘텐츠의 손실 가중치를 줄여 인간의 인식과 잘 일치하는 모델 용량을 보다 효율적으로 사용.
- Diffusion model에서는 '고빈도 콘텐츠'의 개념이 의미가 없기 때문에 이미지 모델에서 용이함(?). 
- 마지막으로 생성적 모델링 성능에 대한 임베딩 절차 선택의 영향도 고려해야함.

- # Diffusion and autoregression
- Token sequence를 sequential condition으로 인수분해: 샘플링은 항상 sequence 방향으로 이루어짐.
- 여기서 "Sampling"은 "Iterative refinement"로 볼 수 있음
- 1D squence를 표현하기에 가장 적합
- <b>같은 parameter가 모든 sequential conditionals를 모델링할 수 있기 때문에, 각 iterative refinement step이 중요한 gradient signal을 만듦.</b>
(diffusion은 보통 single noise level로 모든 training example을 학습하기 때문에)
- 이점 1: Sampling time에서 이전 모델의 activation을 caching할 수 있음
- 이점 2: Diffusion model은 각 step에서 전체 forward pass가 필요하므로 비용이 더 많이 든다.

