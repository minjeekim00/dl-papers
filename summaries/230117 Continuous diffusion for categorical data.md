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
### Diffusion for discrete data
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

### Diffusion and autoregression
- Token sequence를 sequential condition으로 인수분해: 샘플링은 항상 sequence 방향으로 이루어짐.
- 여기서 "Sampling"은 "Iterative refinement"로 볼 수 있음
- 1D squence를 표현하기에 가장 적합
- <b>같은 parameter가 모든 sequential conditionals를 모델링할 수 있기 때문에, 각 iterative refinement step이 중요한 gradient signal을 만듦.</b>
(diffusion은 보통 single noise level로 모든 training example을 학습하기 때문에)
- 이점 1: Sampling time에서 이전 모델의 activation을 caching할 수 있음
- 이점 2: Diffusion model은 각 step에서 전체 forward pass가 필요하므로 비용이 더 많이 든다.

# The CDCD framework
- Categorical cross entropy loss를 사용한 diffusion model training 소개 (w/ score interpolation)
- Noise level의 분포를 자동 적용하는 active learining strategy - time warping 소개

### Score interpolation
- Diffusion model은 보통 score matching objective를 최소화한다
- 데이터가 discrete하거나 categorical하면 (e.g. 사이즈 V인 단어에서 추출된 token) conditional score function s(x,𝑡|x0)만이 추정 가능하다.
- 따라서, x0의 확률적 예측이 있는 경우, 이를 사용하여 𝑉 가능한 값을 interpolate하여 score function estimates을 얻을 수 있음
- V logits을 예측하기 위해 softmax nonlinearity 사용 가능. 
- Auto-regressive language model을 학습하는 설정과 비슷.

### Diffusion on embeddings
- Continuous space에 input embedding하기.
1. 임의의 embedding을 다른 token에 assign한다.
2. Representation learning을 이용하여 embedding을 얻는다.
=> <b> 이 논문에서는 embedding과 diffusion 모델을 jointly 학습한다. (diffusion model에서 reparameterization trick 사용)</b>
- Score matching을 사용한 diffusion model은 embedding space가 collapse 됐을 것.
- Why? noise 예측때문에 embedding 예측은 사소해짐. Collapse 방지를 위해서는 추가적인 loss condition이 필요함.
- Score interpolation를 사용하여 diffusion model을 cross-entropy loss로 사용 가능.
- 목적 함수는 이제 실제 임베딩을 다른 모든 임베딩과 구별하는 것이므로 노이즈 임베딩이 입력으로 주어지면 모델은 임베딩을 가능한 한 멀리 밀어내도록 권장됨
(이렇게 하면 노이즈의 혼란스러운 영향이 최소화됨).
- embedding이 너무 많아지는 것을 제한해야함. -> embedding vector를 normalize하기. (L2)
- embedding estimate도 score estimate하기 L2-normalization함 (renoramlization)
- 다른 방법으로는 nearest embedding vector를 clamp할 수 있음.

### Time warping
- Diffusion model은 shared parameter에서 single set의 다양한 노이즈 레벨에서 작동하는 denoiser이다.
- 따라서 모델이 다양한 노이즈 레벨에 대한 capacity를 할당하는 정도는 결과 샘플의 인지 품질에 상당한 영향을 미친다.
- => 학습시 노이즈 레벨의 가중치를 대략화하여 컨트롤할 수 있다.
- Cross-entropy loss를 사용하면 각 timestep에서 노이즈레벨 가중치가 상대적으로 바뀐다.
- *Importance sample* 으로 구현가능. - 원하는 상대적 가중치에 상응하는 density를 가진 non-uniform 분포에서 샘플하면됨.
- => *Inverse transform sampling*. 구현을 위해 다음을 사용

***The entropy of the model predictions should increase linearly as a function of F(t).
- 모델 예측의 불확실성은 uniform time function에 상수 비율로 증가한다.
- 따라서 모델의 용량은 입력 시퀀스의 정보 내용 전체에 고르게 분포되어야 합니다.(??)
- 또한, 모델에서 샘플링할 때 균일한 시간에 동일한 간격의 시간 단계를 사용하면 거의 일정한 속도로 불확실성이 점진적 감소한다.

- Active learning 전략처럼 초기에는 노이즈 레벨이 균등하게 샘플되지만, 학습이 진행되며 노이즈 레벨 분포가 학습에 가장 유용한 방향의 레벨로 attention 된다.
