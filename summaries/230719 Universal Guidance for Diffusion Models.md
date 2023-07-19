# Abstract
- text 뿐만 아니라 다른 모달리티를 condition으로 (universal한 condition) 재학습 필요없이 학습할 수 있는 diffusion 모델.

# Introduction
- 모델 결과를 조절할 수 있는 좋은 방법은 guidance를 이용하는 것이다. (Guidance: 어떤 기준에 충족시켰는지 여부를 측정하는 도구)
- e.g. CLIP score가 text description과 생성된 이미지를 비교하여 최소화하도록 guide.
- 이미지 생성의 각 반복 동안 guidance 함수의 기울기를 조금씩 낮추어 최종 생성된 이미지가 사용자 기준을 충족하도록 한다.
- 보통 guidance를 주려면 재학습이 필요한데, 이 논문의 guidance는 universal 해서 많은 목적으로 사용 가능.
- guidance계열의 어려움은 guidance 모델(e.g. CLIP)은 clean 이미지에서 학습된 데에 반해 diffusion sampling process에서 만들어진 noisy 이미지와의 domain shift 때문이다.
- !!!이 논문에서는 노이지 latent states가 아닌, denoised image에만 적용되어 강점이 있다. (domain gap 줄임)


# Background
## Controlled Image Generation
- 미분 가능한 guidance function, e.g. CLIP feature extractor or segmentation 네트워크를 생각해보자.  
  이미지에 적용했을 때, $c = f(x)$ 를 얻을 수 있다.
- 두 백터  $c$와 $c'$사이의 근접성을 나타내는 함수 $l$또한 정의한다.
- 우리가 구하고 싶은 것은 $l(c, f(z)) \approx 0$ : 제약조건을 만족하는 이미지 분포에서의 sample $z$를 생성하는 것이 목표.
- 간단히 말하면, prompt에 맞는 in-distribution image를 생성하고 싶은 것이다.
- 이를 만족시키는 데에는 1. conditional image generation 2. guided image generation이 있다.

### Conditional Image Generation
- additional input을 넣어야함.  
 e.g. classifier-free guidance에서는 class label을 prompt로 넣어서 unconditional과 conditional 사이의 linear interpolation을 학습
- 또 다른 연구에서는, guidance 함수가 알려진 linear degradation 연산자인 경우를 연구하고, linear inverse problem을 해결하기 위해 조건부 모델을 학습.
- classifier-free guidance로 확장됨. (생성된 이미지의 CLIP표현과 텍스트 프롬프트 간의 유사성을 강화하며 학습)
- 이러한 연구들은 성공적이였으나, 재학습이 필요함.
  
### Guided Image Generation
- Frozen pretrained diffusion 모델 사용하지만 sampling 방법만 이미지 생성시 피드백을 주도록 바뀜.
- 초반에는 external guidance 함수를 주어 restriction을 줌.
- 예를 들어, classifier guidance는, 서로 다른 노이즈 스케일을 갖는 이미지로 분류기를 학습해서, 그 그라디언트를 sampling process에 포함시켰다.
- 하지만 non-linear한 guidance 함수에 까진 적용되지 않음.
- 이 논문에서는, object detection이나 segmentation network 등 어떠한 guidance 함수라도 적용 가능한 universal guidance 알고리즘을 연구.


# Universal Guidance
- 기존의 보조 네트워크로써의 guidance를 포함한 이미지 샘플링 방법을 보강하는 알고리즘을 제안.
- "복원된 클린 이미지 ẑ0 불완전하지만 이미지 생성을 안내하는 정보 피드백을 제공하는 generic guidance 함수에는 여전히 적합하다"에서 아이디어를 얻음.
- 3.1에서는 classifier guidance의 연장선인 **forward universal guidance**
- 3.2에서는 보조적인 **backward universal guidance**를 사용하여 생성된 이미지가 guidance 함수에 따른 constraint에 충족하도록 함.
- 3.3에서는 self-recurrence trick을 이용하여 생성된 이미지의 품질을 높임.

## 3.1 Forward Universal Guidance
- External guidance의 정보로 생성을 guide하기 위해서는 classifier guidance를 어떠한 guidance 함수도 허용하게 발전시켜야한다.
- Class 프롬프트 c가 주어졌을 때, classifier guidance는 $ϵ_θ (z_t , t)$는 각 샘플링 스텝에서 $z_t$가 주어졌을 때 $c$가 나올 확률 $\log p(c|z_t)$을 더하여 학습시킴.
- $\log p(c|z_t)$를 $z_t$를 분류기에 넣은 결과인 $f_cl(z_t))$와의 cross-entropy loss로 대체할 수 있음.
- 하지만 $f_cl$을 $l_ce$로 바로 맞바꾸면 실제로 loss 함수가 잘 작동하지 않음. (f는 클린 이미지에서 학습하기 때문에 noisy할때는 의미있는 guidance를 못줌)
- $ϵ_θ (z_t , t)$는 데이터 포인트에 노이즈를 합한 이미지를 예측함. 따라서 predicted clean image $\hat z_0$ 를 얻을 수 있음.
- 따라서 noisy한 이미지가 아닌 predicted clean 데이터 포인트를 사용.
  
## 3.2 Backward Universal Guidance
- 


## 3.3 Per-step Self-recurrence












