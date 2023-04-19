#bidirectional #transformer


# Abstract
- 3D tokenizer: 를 이용하여 video를 spatial-temporal visual token으로 quantize함.
- Embedding method: 를 이용하여 multi-task learning을 가능하게하는 masked video token modeling 제안
- 추론 시간에서 diffusion model보다 2배, auto-regressive model보다 60배 더 빠르다.
- Single model로 10가지의 생성 테스크 가능.


# Introduction
### Autoregressive 모델을 사용하여 이미지를 생성하는 방법
1. 이미지를 discrete tokens의 시퀀스로 quantize한다.
2. 이전 생성 결과에 따라 이미지 토큰을 sequentially 생성한다. #autoregressive-decoding


### self-attention in Transformer
https://www.notion.so/Transformer-3a7b8f2d2e7e46e1b2cbd4d404cc783a?pvs=4


# Masked Generative Video Transformer
## Spatial-Temporal Tokenization
## Multi-Task Masked Token Modeling
