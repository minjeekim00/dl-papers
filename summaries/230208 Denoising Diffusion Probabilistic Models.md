# Abstract
- Non-equilibrium thermo-dynamics에서 영감을 받은 잠재 변수 모델 중 하나인 diffusion probabilistic model을 사용하여 이미지 합성.
- Diffusion probabilistic model과 Langevin 역학을 사용한 denoising score matching 사이의 새로운 연결을 통한 weighted variational bound 사용
- 자연적으로 autoregressive decoding의 일반화로 해석될 수 있는 progressive lossy decompression scheme 사용.

# Diffusion models and denoising autoencoders
- 디퓨전 모델이 latent variable 모델의 제한적인 종류이긴 하지만, 구현시 degrees of freedom이 많은편.
- Forward process에서는 variance β_t, Reverse process에서는 가우시안 분포 parameterization을 선택해야한다.
- Diffusion model과 denoising score matching의 새로운 explicit한 관계를 설립하여, 간소화되고, weighted된 variational bound 목적함수를 만듦.
### Forward process and L_T
- Forward process variance β_t가 학습 가능한 파라미라는 것을 무시하고 상수로 고정시킴.
- 예측된 posterior q는 학습가능한 파라미터가 없고, L_T는 학습동안 상수이다.
### Reverse process and L_1:T-1
- 먼저 Σθ(x_t,t) = σ_t^2를 학습되지 않은 시간 종속 상수로 설정한다.                                                                                                                                                             

# Experiments
- Forward process variances는 β1 = 10−4 부터 βT = 0.02까지 linearly 증가
    - Reverse와 forward 과정이 x_T에서의 SNR을 최대한 작게 유지하면서도 대략 같은 functional form을 가지도록
- Reverse process에는 unmaksed PixelCNN++와 비슷한 U-Net backbone을 사용 + Group normalization
- 파라미터는 Transformer sinusoidal position embedding 네트워크를 사용하여 시간을 가로질러 공유된다.
- 16 x 16 feature map resolution의 self-attention 이용했을 때 µ̃를 더 잘 구했다.

### Sample quality
- True variational bound가 simplified objective에서 더 나은 codelength를 만들었다.
### Reverse process parameterization and training objective ablation
- Reverse process parameterization과 학습 목적 함수의 sample quality effect를 살펴보자.
- Unweighted mean squared error보다 true variational bound에서 학습했을 때 


# Discussion on related work
- 학습 후 추가하기 보다는 latent variable model 자체로 sampler를 학습. 
- NCSN과 다른 점들:
1. U-Net + self-attention을 사용.
    - Transformer sinusoidal position embedding을 t 타임에 모든 레이어에 condition했다. 
2. 각 forward process step에 데이터를 scale down해서 noise가 더해질 때 variance가 증가하지 않는다.
    - 따라서 reverse process neural net에서 입력값이 일정하게 scaled된다.
3. Forward process에서 (D_KL(q(x_T|x_0)|| N (0, I)) ≈ 0)을 제거한다.
    - prior와 aggregate posterior가 close-match임을 확신할 수 있음.
    - βt가 매우 작아서 forward process가 conditional gaussians를 이용한 Markov chain에 의해 reversible 될 수 있음.
4. Langevin-like sampler는 forward process βt에서 오는 coefficients가 있음. (learning rate, noise scale, etc.)
    - 따라서, 학습 과정에서 sampler가 T step 후의 데이터 분포와 match하도록 바로 학습된다. (??)
    - = 즉, variational inference를 이용한 latent variable model로써 샘플러를 학습.
