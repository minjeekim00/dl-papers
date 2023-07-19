# Abstract
- retraining 없이 text 외에 다른 모달리티를 condition으로 학습할 수 있는 diffusion 모델.

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
- 
### Guided Image Generation
