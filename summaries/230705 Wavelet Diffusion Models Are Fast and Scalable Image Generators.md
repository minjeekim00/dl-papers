- 요약: wavelet-based diffusion을 이용하여 sampling 시간을 개선.
- Low-freq에서 뽑히는 intermediate feature representations, High-freq의 detail을 모두 잡았음.


# Related Work
## Diffusion GAN
- 원래 DDPM과 같은 기존 diffusion은 가우시안 노이즈 미세주입을 반복하기 때문에 step수가 매우 크다. (수천번~)
- 가우시안 노이즈 스텝 여러개를 multimodal distribution으로 모델링하여 large step으로 퉁칠 수 있어짐 (2~4번) -> Diffusion GAN
- 그래도 StyleGAN2보다는 이미지 생성에 시간이 오래걸림

## Wavelet-based approaches
- 고차원공간에서 저차원 매니폴드에 이동에 frequency sparsity와 wavelet transform 차원 축소를 사용함.
- image, feature 레벨 모두에 frequency 정보를 넣어줌. -> 성공의 핵심!

# Background
## Diffusion Models
- 노이즈를 주입하는 diffusion process는 매우 소량의 노이즈를 추가하므로 이 반대인 reverse process는 가우시안 프로세스로 근사화할 수 있음.
- 따라서 $x_t$ 일 때 $x_{t-1}$ ... 를 chain rule을 사용하여 파라미터화할 수 있음
- 최종 손실함수는 true denoising distribution과 파라미터화된 distribution사이의 KL divergence.
- Diffusion GAN에서는 large step을 multimodal 분포로 모델링하는데, 이 때 GAN을 사용한다 (분포학습하는것에 GAN을 사용)
- Diffusion GAN는 conditional generator ($x_t$가 주어졌을 때 $x_{t-1}$)을 사용하여 가짜샘플을 생성한다.

-----------------------------------------------------------------------------------------------------------------------------------------------------------------여기서부턴 네트워크 설명
# Method
## Wavelet-based diffusion scheme
- 이미지를 4개의 wavelet subband로 분해한 후 denoising process을 위해 하나로 concat한다. => wavelet spectrum에서 학습됨
=> 이렇게하면 high-frequency 정보를 가지고 디테일 향상을 할 수 있다.
- 한편, wavelet subband의 공간 영역은 원본 이미지보다 4배 작기 때문에 샘플링 프로세스의 계산 복잡성이 크게 줄어든다.
- 입력 사이즈는 3 X H X W 에서 12 X H/2 X W/2가 됨.
- Diffusion GAN의 판별기는 $y_t$ -> $y_{t-1}$로 갈 때 real pair($y_{t-1}, y_t$)와 fake pair($y_{t_1}^{'}, y_t$)를 구별한다.

### Reconstruction term
- 주파수 정보의 손실을 방지 + Wavelet subband의 일관성 유지를 위해 adversarial loss에 recon term을 추가한다.   
   (Generated image와 ground-truth 사이의 l1 loss 추가, 디폴트는 1:1)
- 최종 이미지는 wavelet inverse transform (IWT)를 통하여 찾아진다.

## Wavelet-embedded networks
- 다음으로 고주파 구성 요소에 대한 인식을 강화하기 위해 생성기를 통해 wavelet 정보를 feature 공간에 추가로 통합한다 => 최종 이미지의 선명도와 품질에 도움
- UNet 구조의 up/down sampling operator에 frequency-aware block을 추가
- 가장 낮은 해상도에서는, frequency-bottleneck을 만들어 low/high frequency 구성요소에 더 나은 attention을 준다.
- 원본 신호 Y를 인코더의 다른 feature pyramid에 집어넣는다. (frequency residual connections)

### Frequency-aware downsamling and upsampling blocks
- 원래 downsampling block은 input features, latent z, time embedding t 튜플을 받는다.
- 그 다음 몇 레이어를 거쳐 다운샘플된 feature와 high-frequency subband로 처리된다.
- 이렇게 리턴된 서브밴드는 그 다음에 이어지는 업샘플링 블록의 주파수 큐를 기반으로 업샘플링하기 위한 추가 입력으로 사용된다.
  
### Frequency bottleneck block
- 먼저 feature map $F_i$를 저주파 $F_{i,ll}$와 고주파 서브밴드 $F_{i,H}$의 concat으로 나눈다.
- $F_{i,ll}$은 더 깊은 처리를 위해 resnet 블록에 대한 입력으로 전달된다.
- 처리된 low-freq feature map과 원본 high-freq 서브밴드는 IWT를 통해 원래 공간으로 돌아온다.
- => low-freq 서브밴드의 intermediate feature representations에 집중하며 high-freq 디테일 보존 가능

### Frequency residual connection
- 원래는 원래 신호 Y가 인코더의 feature 피라미드로 들어갈 때 strided-convolution 다운샘플링 사용.
- 하지만 wavelet downsampling 레이어를 입력 Y의 residual shortcut을 해당 feature 차원에 매핑한 다음 각 피라미드에 추가하는 방법을 사용한다.
- (Y의 residual shortcut은 4개의 서브밴드로 분해되고 concat된 다음 feature projection을 위한 conv layer로 주어진다)
