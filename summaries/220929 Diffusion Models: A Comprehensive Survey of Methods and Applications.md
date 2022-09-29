

## Abstract
Diffusion model은 costly한 샘플링 과정과, sub-optimal likelihood estimation을 수반한다.  

이 논문에서 세 가지로 요약한다. 
1. Sampling-acceleration enhancement 
2. Likelihood-maximization 
3. Data-generalization enhancement 


## Introduction
- Diffusion probabilistic model은 non-equilibrium thermodynamics에 영감을 받은 latent variable generative model에 의해 제안되었다.  
  - 보통 이 모델은 두 과정으로 나누어져 있음
  - 1. 여러 번의 scale로 노이즈를 추가하여 데이터 분포를 점진적으로 방해한다.
  - 2. 그 다음 반대과정을 통해 데이터 구조를 복구한다.
  - => 이 과정에서 deep hierarchical VAE로 볼 수 있다. (encoding - decoding 과정과 비슷) -> variational lower bound를 찾기도함.
  - => Discretization of stochastic differential equations (SDE)로 볼 수도 있음. (forward SDE - reverse SDE)

## Preliminaries of Diffusion Models
- 생성 모델의 가장 큰 trade-off는 flexibility <-> tractability.
- Diffusion model은 크게 Denoising diffusion probabilistic models vs Score-based generative model로 나뉨.

### Denoising Diffusion Probabilistic Models
- DDPM은 두개의 파라미터화된 Markov chain과 variational inference를 사용.
- 가우시안 노이즈를 점진적으로 추가하여 prior인 가우시안 분포에 이를 때 까지 데이터 분포를 교란시킨다.
- 반대 과정은 prior로 부터 파라미터화된 가우시안 transition kernel을 이용하여 교란되기 전 데이터 구조를 학습한다.
- 학습은 negative log-likelihood의 varational upper bound를 최적화 하는 방향.
- 목적 함수는 몬테카를로 알고리즘으로 예측, stochastic optimizer를 사용하여 학습한다.

### Score-based Generative Models
- SDE를 사옹하여 데이터 분포를 교란시킨다. (알려진 prior -> reverse-time SDE 사용하여 prior로 이동)
- w는 standard Wiener process (aka standard Brownian motion)
- DDPM의 forward process를 special forward SDE의 discretization으로 볼 수 있다.
- Reversd SDE와 forward SDE의 솔루션은 같은 marginal density를 가지지만, reverse time으로 진화한다.
- Score-matching 기술에서 빌려와 denoising score-matching 목적을 최적화하는 score function을 예측할 수 있다.
- DDPM의 reverse chain과 reverse SDE using discretization은 닮아있음.


## Diffusion Models with Sampling-Acceleration Enhancement
- Diffusion process에서 많은 수의 discretization step이 필요함.

### Discretization Optimization
1. SDE Solver
  - Score-based Generative Modeling through Stochastic Differential Equations. (LSM)
    - reverse-time SDE를 forward와 맞추려고 노력. (continuous settings)
    - DDPM보다 살짝 더 나은 결과
    - "corrector"를 SDE solver에 추가하여 correct marginal distribution을 샘플링
    - Markov Chain Monte Carlo 방법을 사용한다.
    - Corrector가 step 수를 늘리는 것 보다 나은 결과를 나타냄.  
  - Gotta Go Fast When Generating Data with Score-Based Models.
    - step-size SDE solver (fast high-order SDE + accurate low-order SDE)
    - high, low solver가 previous 샘플로부터 각각 샘플을 생성한다.
    - high, low 샘플이 비슷하다면, 알고리즘은 high-low의 interpolation을 생성하여 새로운 샘플로 받아드린다. 그리고 step size를 높인다.
  - Come-closer-diffuse-faster
    - Better initialization에서 시작한 single forward diffusion이 샘플링 스텝을 줄여준다.
    - Non-expansive linear mapping에 의해 샘플 형성
    - "stochastic contraction" 이론을 사용하여, 더 짧은 샘플링 경로가 존재하고, 이 경로의 추정 오류는 원래 추정의 추정 오류에 의해 bound됨.
  - Diffusion Schrödinger bridge with applications to score-based generative modeling (DSB)
    - the Schrödinger Bridge problem (SB)을 사용하여 LSM의 연구를 연장
    - SB 문제를 iterative proportional fitting procedure의 approximation을 사용.
    - DSB는 생성 모델과 optimal transport 문제의 커넥션을 제공함.
2. ODE Solver
  - 모든 diffusion 모델은 상응하는 ODE를 가지고 있음 (diffusion SDE와 같은 marginal distribution)
  - DPM-Solver: A Fast ODE Solver for Diffusion Probabilistic Model Sampling in Around 10 Steps.
    - Linear part는 계산하고, non-linear part는 신경망을 exponentionally-weighted integration하여 근사화한다.
    - Diffusion ODE는 DDPM을 포함한다.
    - Linear part의 discretization error를 줄여주고, sampling step을 줄여준다.
    - Fitting error도 줄여준다.
  - Fast Sampling of Diffusion Models with Exponential Integrator (DEIS)
    - 비슷한 semi-linear struction를 가짐.
    - Discretizing the diffusion ODE를 quadratic time step.
    - ODE의 solution 계산을 근사화하기 위해 exponential intergrator 사용.
    - 더 나은 score estimation을 위하여 polynomial extrapoliation 사용.
  - Pseudo numerical methods for diffusion models on manifolds (PNDM)
    - Manifold에서 faster higher-order ODE를 디자인 할 수 있음.
    - 솔루션 궤적의 tangent는 discretization 오류를 줄이기 위해 noise schedule을 적절하게 설계하여 잡음이 제거된 출력으로 point 해야한다.


### Non-Markovian Process
- Markov process는 last step만 활용하기 때문에 이전 샘플의 풍부한 정보에 대한 제약이 있음.
- 반대로, Non-Markovian process의 transition kernel은 더 많은 샘플에 의존함. 더 많은 정보 활용 가능.
- Large step size로도 정확한 예측 가능.
- DDIM이 DDPM을 non-Markovian case로 확장시킴.
- DDIM의 생성 전략은 ODE의 special discretization으로 볼 수 있음. 따라서 deterministic하게 샘플할 수 있음.
- gDDIM: Generalized denoising diffusion implicit models.
  - DDIM를 general diffusion model로 확장 (CLD과 같이)
- Pseudo numerical methods for diffusion models on manifolds (PNDM)
  - DDPM을 manifold상에서의 SDE로 봐야한다.
  - Fast sampling을 위해 pseudo numerical solver를 제안
  - DDIM을 special case로 인캡슐화 가능.
- Learning fast samplers for diffusion models by differentiating through sample quality (GGDM) 
  - 생성 과정에서 다음 샘플을 예측하기 위해 모든 이전 샘플을 사용해야한다. (DDIM의 일반화)
  - 또한 DDSS를 활용하여 샘플 품질 점수를 차별화함으로써 몇 단계 내에 빠른 샘플러를 최적화한다.
