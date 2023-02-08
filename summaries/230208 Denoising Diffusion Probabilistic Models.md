# Abstract
- Non-equilibrium thermo-dynamics에서 영감을 받은 잠재 변수 모델 중 하나인 diffusion probabilistic model을 사용하여 이미지 합성.
- Diffusion probabilistic model과 Langevin 역학을 사용한 denoising score matching 사이의 새로운 연결을 통한 weighted variational bound 사용
- 자연적으로 autoregressive decoding의 일반화로 해석될 수 있는 progressive lossy decompression scheme 사용.



# Discussion on related work
- 학습 후 추가하기 보다는 latent variable model 자체로 sampler를 학습. 
- NCSN과 다른 점들:
1. U-Net + self-attention을 사용.
  - Transformer sinusoidal position embedding을 t 타임에 모든 레이어에 condition했다. 
2. 각 forward process step에 데이터를 scale down해서 noise가 더해질 때 variance가 증가하지 않는다.
  - 따라서 reverse process neural net에서 입력값이 일정하게 scaled된다.
3. Forward process에서 (DKL (q(xT |x0 ) || N (0, I)) ≈ 0)을 제거한다.
  - prior와 aggregate posterior가 close-match임을 확신할 수 있음.
  - βt가 매우 작아서 forward process가 conditional gaussians를 이용한 Markov chain에 의해 reversible 될 수 있음.
4. Langevin-like sampler는 forward process βt에서 오는 coefficients가 있음. (learning rate, noise scale, etc.)
  - 따라서, 학습 과정에서 sampler가 T step 후의 데이터 분포와 match하도록 바로 학습된다. (??)
  - = 즉, variational inference를 이용한 latent variable model로써 샘플러를 학습.
