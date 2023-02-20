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
- 2D network를 temporal dimension으로 확장하기 위해 두가지를 사용한다:
1) Conv layer. 2) Attention layers.
- FC와 같은 다른 레이어들은 structured 공간/시간 정보가 없기 때문에 차원을 더하는 것은 쉽다.
- Temporal 변형: U-Net-based diffusion network에서 대부분 사용, spatio-temporal decoder D가 16 RGB Frame (64 x 64) 생성
- Super resolution에는 hallucination이 포함된다. 아티팩트가 깜박이지 않도록 하려면 hallucination이 프레임 전체에서 일관되어야 합니다. (??)
- 결론적으로, SR 모듈이 spatial, temporal에 걸쳐 작동된다.

#### Pseudo-3D convolutional 레이어
- Pretrained 2D conv 레이어에 새롭게 초기화된 1D conv layer를 잇는다.
- Temporal conv는 scratch부터 학습하면서, 공간적 정보는 유지할 수 있다.
- Conv 2D는 pre-trained T2I model로 초기화되고, Conv 1D 레이어는 identity function으로 초기화.
- 초기화때는 각각 입력 텍스트에는 충실하지만 시간적 일관성이 부족한 형태로 K개의 다른 이미지를 생성할 것이다.

#### Pseudo-3D attention 레이어
- T2I 네트워크의 중요한 구성 요소는 attention 레이어이다.
- 추출된 feature에 self-attending 이외에도 텍스트 정보가 diffusion time-step과 같은 기타 관련 정보와 함께 여러 네트워크 계층에 주입된다.
- Dimension decomposition을 attention 레이어에도 적용한다.
- 각각의 pre-trained spatial attention 레이어 뒤에 temporal attention layer를 쌓는다.
- 컨볼루션 레이어와 마찬가지로 전체 spatiotemporal attention 레이어를 근사한다.
- 특히, 주어진 tensor h에 대해서, 우리는 spatial dimension을 B x C x F x HW로 평탄화하는 `flatten`을 matrix operator로 정의한다.
- `unflatten`은 inverse matrix operator로 정의된다.
- Pseudo 3D와 비슷하게, smooth한 spatio-temporal 초기화를 위해서, attention 2D 레이어는 pre-trained T2I 모델로부터 초기화하고, attention 1D 레이어는 itentity function으로 초기화 된다.
- 분해된 space-time attention layer는 VDM와 CogVideo에서도 쓰인다.
  - CogVideo: temporal layer를 각 frozen spatial layer에 추가하여 jointly 학습한다.
  - VDM: images와 videos가 interchangebly 학습하기 위해서, 2D U-Net을 unflattened 1x3x3 conv filter를 사용하여 3D로 확장.
  - 연속적인 spatial attention은 2D에 남아있으면서, 1D temporal attention은 relative position embedding을 통해 추가됨.
- 반면에, 이 논문에서는 각 1x3x3 이후 3x1x1 conv projection을 추가해서 temporal 정보가 각 conv 레이어에 통과하도록 함.
- 
