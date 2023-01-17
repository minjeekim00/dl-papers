# Abstract
- Diffusion model -> iterative refinement를 활용한 생성 모델
- -> 물리적 현상이 "연속적(continous)"이라는 사실에 의거함
- -> discrete 하거나 caterofical한 데이터는 어떻게 학습할것인가? 
- Time과 input space 모두가 continuous한 diffusion model을 이용하여 categorical data 모델링.


#  Introduction
- Diffusion-based language model은 language의 textual representation이 discrete categorical하기 때문에 힘들다.

## CDCD Framework
- Transformer: noise embedding으로부터 token 예측 (attention masking x) -> Cross-entropy
- Input noise: time warping을 통해 non-uniformly sampled t에 time-dependent
- Cross-entropy logit을 사용하여 ODE solver로 샘플링하는 데 사용되는 interpolation을 통해 score function estimate을 얻을 수 있다.
