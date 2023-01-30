

- Spatio-temporally factorized diffusion model
- Space와 Time에 Super-Resolution


### 비디오 생성을 위해 Image prior 사용하기
- MoCoGAN-HD: 사전학습 및 고정된 이미지 모델의 latent space에서 trajectory 찾기 (unconditional video generation)
- CogVideo: 사전학습 및 고정된 T2I 모델의 작은 부분만 trainable parameter로 만들어 메모리 감소 (T2V generation)
  - 고정된 오터인코더와 T2I 모델은 T2V 생성이 제한적일 수 있음


# Method
1. T2I 모델
2. Spatio-temporal convolution + attention layer
3. Frame interpoliation 네트워크

### Text-To-Image 모델
- P: 주어진 텍스트 임베딩을 받아 이미지 임베딩을 생성, BPE로 text token 인코드
- D: 이미지 임베딩에서 저해상도의 64 x 64 이미지를 만들어냄.
- SR/SR: 두 개의 super-resolution 네트워크. 256 x 256 => 768 x 768

### Spatio-Temporal 레이어
- 1) Conv layer. 2) Attention layers 사용
- Temporal 변형: U-Net-based diffusion network에서 대부분 사용, spatio-temporal decoder D가 16 RGB Frame (64 x 64) 생성
- 
