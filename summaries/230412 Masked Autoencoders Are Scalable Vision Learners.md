
# Abstract
- Masked autoencoder (MAE)는 scalable한 self-supervised learner이다. (vision에서)
- Asymmetric한 encoder-decoder 구조.
  - Encoder: visible한 패치에만 작동. mask token 사용 x
  - Decoder: Lightweighted.
- 이미지의 많은 비율 (75%)을 가리는것이 nontrivial하고 meaningful한 self-supervisory task 생산.
=> 두 디자인을 합쳐서 3배 더 빠른 모델 개발


# Introduction

### GPT vs BERT
- GPT: autoregressive language modeling
  - Decoder에서 현재까지 inference output을 다시 input으로 활용하여 inference.  
- BERT: masked autoencoding
  - Masked language modeling을 통해 pretraining. 
  - 몇 단어를 무작위로 마스킹하여 빈칸을 잘 추론하도록 Encoder를 학습하는 기법.
  - 일종의 self-supervised learning (+autoencoder)
- 두 모델 모두 데이터의 한 부분을 제거하고, 제거된 컨텐츠를 예측하는 것을 학습한다.


### NLP에서 autoencoding이 잘 되는 이유?
- 정보의 집약도(information density)가 다르기 때문. 언어는 단어 하나하나가 중요하지만, vision에서는 의미없는 정보가 많음.
- Decoder의 역할이 다름. Output이 NLP에서는 단어로, 높은 semantic을 가지지만 vision에서는 하나의 픽셀 input은 low level semantic을 가짐.
- BERT에서 optimal mask ratio는 15%이지만, vision 에서는 redundancy가 높아 75%에서 optimal을 보임.




#######ref: https://ambitious-posong.tistory.com/166
