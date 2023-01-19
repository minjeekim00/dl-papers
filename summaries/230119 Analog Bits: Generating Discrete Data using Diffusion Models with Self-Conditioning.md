# Related work
### Autoregressive models for discrete data
- Discrete/categorical 이미지 생성에 autoregressive model이 쓰임

### Diffusion models for discrete data
-  현재 diffusion model은 discrete or categorical data를 생성할 수 없음.
-  Discrete data 학습을 위해 discrete data space와 state space로 나누어 학습 (1)
-  Discrete state space에 비해 continuous state space가 훨씬 유연하고 효율적임.
-  Discrete data embedding을 사용 (2)
-  단순 thresholding은 쉽지만, continuous embedding을 quantization하면 multiple modes를 가져 thresholding/quantization하기 어려움

### Normalizing Flows for discrete data
- Discrete flows: discrete space에서 랜덤변수를 invert
- Discrete data를 continuous space에 disjoint support로 transform. : variational inference
- => Strict invertible restriction이 있음

### Other generative models for discrete data
- VAE나 GAN은 autoregressive model처럼 성능이 좋지 않음.
