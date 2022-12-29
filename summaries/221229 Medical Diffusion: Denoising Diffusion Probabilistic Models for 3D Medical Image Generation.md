## Abstract
- Diffusion model을 3D medical image에 적용시킨 모델.
- Realistic image appearance / anatomical correctness / consistencey beween slice 세 가지로 나누어 평가
- Self-supervised pre-training에 사용하여 더 나은 결과를 보임.

## Related works
- Latent diffusion model의 연장이라고 볼 수 있음.
- VQ-GAN의 latent space에 diffusion probabilistic model을 추가하여 3D 이미지 생성 가능
- 압축된 latent space에 적용되기 때문에 cost가 적음
- 저차원의 latent space에 샘플링을 적용하여 샘플 타임을 줄임
- Pixel-level 정보 대신 간소화된 image content 정보를 갖기 때문에 다양한 응용 가능

## Materials and Methods
### Architecture
- 먼저 이미지를 저차원의 latent space로 인코딩한 뒤 데이터의 latent representation으로 diffusion probabilistic model을 학습한다.
1. VQ-GAN
