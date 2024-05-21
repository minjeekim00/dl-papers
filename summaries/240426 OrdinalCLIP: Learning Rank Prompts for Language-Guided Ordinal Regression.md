# Abstract
- Rank를 카테고리가 아닌, Ordinal regression으로 해결.
- Contrastive objective를 통한 image-language 매칭으로 해결.
- Ordianl regression에 CLIP을 적용하기 위해 차별화 가능한 프롬프트 방법 개발.

# Introduction
- 원래는 continuous target space를 discretize하거나, rank 레이블을 각자 다른 숫자로 취급하여 해결했었음.
- (이러한 discrete 값을 각기 다른 카테고리로 여긴 다음, 분류 문제를 해결)
- 하지만 이렇게하면
1. 클래스 카테고리에 ordinal property를 못찾음
2. 학습셋에 의존성이 크기 때문에 overfit할 확률이 큼
- Multimodal information을 이용하여 이러한 문제를 완화할 수 있음

- Language에서 "순위" 개념을 동시에 차용하는 것을 고려한다.
- 구체적으로, 각 순위 레이블은 클래스 카테고리로 간주될 뿐만 아니라 "이 사람은 23세입니다"와 같이 해당 순위를 설명하는 문장으로 연결된다.
- 각 rank 레이블을 text input (프롬프트)로 바꾼 후 CLIP을 이용하여 rank를 위한 language prototype을 추출한다.
- 그 다음, ordinal regression을 image-text 매칭 문제로 재구성한다. -> !!image feature를 고정된 language space에 매칭!!
- Language 모델은 고정했기 때문에, 약간의 CLIP 모델 knowledge distillation 효과
- + 이미 잘 학습된 language latent space로 제한되기 때문에, 일종의 regularization 효과 -> 일반화 잘됨.

- 학습 가능한 context token + 학습가능한 rank embeddings로 구성됨.
- 언어 모델은 숫자의 연속성을 모두 배우진 못하기 때문에, rank embedding의 연속 특성을 명시적으로 모델링하는 여러 기본 feature 사이의 interpolation을 통해 학습한다.


- Language prior를 사용하기 위해 CLIP을 사용
- (Text embedding) 각 순위 레이블을 text prompt로 변환하고, CLIP에 사전학습된 text encoder를 사용한다.
- (Image embedding) image encoder는 trainable하게 열어둔다.
- 각각에서 embedding을 얻어 image-language matching ordinal regression을 학습한다.
- Fixed language model에서 학습됐기 때문에, distillation 한다.

# Experiments
## Age Estimation
- MORPH 2 데이터셋: 13,618명의 55,134 portraits, 16~77세 (long-tail 분포)
- Adience 데이터셋: 2,284명의 26,580 Flickr 사진. 0-2, 4-6, 8-13, 15-20, 25-32, 38-42, 48-53, 60+ 의 8개 그룹으로 구성. + 분류

## Comprasion with State-of-the-art methods
1. Gaussian mixture 모델을 deep neural decision forest로 확장 (regression)
2. Meta learning을 사용하여 나이 분포의 adaptive variance 학습
3. General regression + Uncertainty modeling - $성능 좋음$
4. Zero-shot CLIP: MAE 14.45로 성능 낮음
5. CoOp: MAE 2.39. -> 언어 prior의 효율성을 입증
6. OrdinalCLIP: MAE 2.32

## Parameter Analysis
- Base rank embedding의 숫자, interpolation 타입에 대한 분석
