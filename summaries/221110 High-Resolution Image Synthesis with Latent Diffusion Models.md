# Abstract
- diffusion을 이용한 이미지 생성특징
  - 제약을 두기 보다는 guiding 매커니즘이다.
  - Pixelspace에 바로 작용된다.
- Pretrained auto-encoder의 latent space에 diffusion process를 적용하여 resource를 줄임.
- Representations에 diffusion 적용하면 complexity reduction과 detail preservation을 모두 가져갈 수 있음
- Cross-attention layer 사용

# Introduction
### Departure to Latent Space
- Likelihood-based 모델은, 두 가지 스테이지로 나눠진다.
- 1. perceptual compression stage: 고주파 디테일을 없애고 적은 semantic variation을 배운다.
- 2. semantic compression stage: 실제 생성 모델이 데이터의 semantic과 conceptual composition를 학습.
=> 따라서, 우리는 perceptually equivalent하지만 computationally more suitable한 공간을 찾고난 후 고해상도 이미지를 학습.

- 먼저, 저차원의 representational space를 제공하는 autoencoder를 학습한다. (데이터 공간과 perceptually equivalent)
- "이미 학습된 latent space에서 diffusion model(DM)을 학습하기 때문에 과도한 spatial compression에 의존할 필요 없음"
- 학습된 latent space는 공간 차원과 관련하여 더 나은 스케일링 속성을 나타냄
- Universal한 autoencoding 학습 후에 DM 학습으로 재사용 가능. -> 전혀 다른 task에도 쓸 수 있다.
- Text-to-image 모델에서는 transformer를 DM의 UNet backbone에 연결하는 구조를 디자인할 것 => 임의 유형의 토큰 기반 컨디셔닝 메커니즘을 활성화할 수 있음

### Figure 1.
- Spatial downsampling factor by f.
- 덜 공격적인 downsampling으로 영상 품질의 상한을 높인다.
- DM은 공간 데이터에 대한 훌륭한 inductive biases를 제공한다. -> 지나친 spatial downsampling 필요 x


# Method
- 이미지 공간과 인지적으로 동등한 space를 배우는 autoencoding 모델을 학습한다.
- 하지만 computational complexity는 확연히 감소한다.
1) 샘플링이 저차원에서 이루어지기 때문에 효율적이다.
2) Diffusion 모델의 inductive bias를 Unet으로부터 물려받는다 -> spatial structure
3) Downstream application에 사용 가능한 multiple generative models 학습 가능.

### Perceptual Image Compression
- Taming transformer + Autoencoder(perceptual loss + patch-based adversarial objective)
  - 이는 Reconstruction이 true image manifold에만 국한됨. (??) (local realism을 강요)
  - L2 또는 L1 objective와 같이 pixel space loss에만 의존하면 bluriness가 발생하는데, 이를 방지할 수 있다.
- 임의의 high-variance latent space를 피하기 위해, 두가지 regularization으로 나누어서 실험.
  - 1. KL-reg: 학습된 latent가 standard normal가 되도록 약간의 KL-penalty를 부과.
  - 2. VQ-reg: 디코더에 vector quantization 레이어를 사용. 디코더에 흡수되는 quantization 레이어가 있는 VQGAN으로 해석된다.
- 이후에 학습되는 DM은 학습된 잠재 공간 z = E(x)의 2차원 구조와 함께 작동하도록 설계되었기 때문에 비교적 완만한 압축률을 사용해도 매우 우수한 reconstruction을 달성할 수 있다.
- 기존에는 학습된 공간 z의 임의의 1D 순서에 의존하여 분포를 auto-regressive하게 모델링하여 z의 고유한 구조가 무시됐었음.

### Latent Diffusion Models
#### Diffusion Models
- T길이의 고정된 Markov Chain의 reverse process를 학습.
- 이미지 생성에서 성공한 모델들은 variational lower bound의 reweighted variant에 의존. :denoising score-matching.
  - 이러한 모델들은 denoising autoencoder의 equally weighted sequence로 해석된다.
  - 입력 x_t의 denoised variant를 예측하는 것을 학습하고, x_t는 x의 noisy 버전이다.
#### Generative Modeling of Latent Representations
- Perceptual compression 모델로 이제 high-frequency이면서, 감지할 수 없는 디테일이 추상화되는 효율적이고 저차원의 latent space가 학습됨.
- 고차원의 pixel space에 비교해서, 이 공간은 좀더 likelihood-based 생성 모델에 적합하다.
- 이제 1) 중요하고, semantic한 데이터 bits에 집중 가능. 2) 낮은 차원에 학습으로 계산적으로 매우 효율적인 공간이다.
- Inductive bias이용 가능.
  - 2D Conv로 구성된 기본 UNet을 구축할 수 있음 + reweighted bound를 사용하여 지각적으로 가장 관련성이 높은 bits에 objective를 추가로 집중시킴.
- Backbone은 time-conditional Unet.

#### Conditioning Mechanisms
- 다른 생성 모델들 처럼 conditional training 가능.
- 
