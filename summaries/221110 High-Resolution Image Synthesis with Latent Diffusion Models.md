# Abstract
- diffusion을 이용한 이미지 생성특징
  - 제약을 두기 보다는 guiding 매커니즘이다.
  - Pixelspace에 바로 작용된다.
- Pretrained auto-encoder의 latent space에 diffusion process를 적용하여 resource를 줄임.
- Representations에 diffusion 적용하면 complexity reduction과 detail preservation을 모두 가져갈 수 있음
- Cross-attention layer 사용


# Method
- 이미지 공간과 인지적으로 동등한 space를 배우는 autoencoding 모델을 학습한다.
- 하지만 computational complexity는 확연히 감소한다.
1) 샘플링이 저차원에서 이루어지기 때문에 효율적이다.
2) Diffusion 모델의 inductive bias를 Unet으로부터 물려받는다 -> spatial structure
3) Downstream application에 사용 가능한 multiple generative models 학습 가능.

### Perceptual Image Compression
