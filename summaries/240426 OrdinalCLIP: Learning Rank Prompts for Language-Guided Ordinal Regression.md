# Abstract
- Rank를 카테고리가 아닌, Ordinal regression으로 해결.
- Contrastive objective를 통한 image-language 매칭으로 해결.
- Ordianl regression에 CLIP을 적용하기 위해 차별화 가능한 프롬프트 방법 개발.

# Introduction
- 원래는 continuous target space를 discretize하거나, rank 레이블을 각자 다른 숫자로 취급하여 해결했었음.
- (이러한 discrete 값을 각기 다른 카테고리로 여긴 다음, 분류 문제를 해결)
- 하지만 이렇게하면 1. 클래스 카테고리에 ordinal property를 못찾음
- 2. 학습셋에 의존성이 크기 때문에 overfit할 확률이 큼
- Multimodal information을 이용하여 이러한 문제를 완화할 수 있음

- Language에서 "순위" 개념을 동시에 차용하는 것을 고려한다.
- 구체적으로, 각 순위 레이블은 클래스 카테고리로 간주될 뿐만 아니라 "이 사람은 23세입니다"와 같이 해당 순위를 설명하는 문장으로 연결된다.
  
- Language prior를 사용하기 위해 CLIP을 사용
- (Text embedding) 각 순위 레이블을 text prompt로 변환하고, CLIP에 사전학습된 text encoder를 사용한다.
- (Image embedding) image encder는 trainable하게 열어둔다.
- 각각에서 embedding을 얻어 image-language matching ordinal regression을 학습한다.
- Fixed language model에서 학습됐기 때문에, distillation 한다.
