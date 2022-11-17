# Abstract
- Discriminative pixel-level task를 생성모델을 이용하여 풀어냈다.
- 이 논문의 모델은 limited 데이터를 사용하여 image-label 분포의 joint를 포착한다.
- Label synthesis branch를 사용하여 augmented 된다.
- 테스트 시 이미지 라벨링은 인코더를 통해 joint latent space에 대상 이미지를 embedding한 다음, 
추론된 embedding에서부터 label을 생성하여 달성된다.


# Introduction
- Semi-supervised learning(SSL)의 종류
  - pseudo-labeling
  - consistency regularization
  - data augmentation techniques
  - contrastive learning 
    : powerful image feature extractor + unsupervised contrastive loss on image transformations 학습이 목표

## 방법 요약
1. Joint image-label distribution 모델링을 통해 image와 semantic segmentation mask 모두 합성.
2. Label generation branch를 사용하여 augmentation.
3. Test-time prediction에서는 먼저 input image를 recon하여 latent code 최적화하고, 
   추론된 embedding을 사용하여 G에 적용함으로써 label을 합성한다.
   
# Related Work
- Semi-Supervised Learning + Semantic Segmentation
  - 이 논문에서 최초로 추가 loss나 cross entropy 없이 pixel-wise label을 합성.
  - 학습된 D를 semantic outputs로 test time에 사용.  
- Generator Inversion
<b>  - 보통 Image reconstruction이나 editting을 하려고 inversion을 쓰는데, 이 논문에서는 pixel-wise image labeling을 위한 inferred embedding을 얻기 위해 쓰임. </b>

- Generative Models for Image Understanding
  -  생성 모델로 여러가지 discriminative 하거나 image recognition task를 수행한 경우들.
  - Missing label의 "in-painting"으로 해석할 수도 있음. (joint image-label distribution)
  
  
# Method
## Overview
- image와 label의 joint distribution인 p(x, y) 합성. output은 images x, labels y 둘다 나옴.
- given z, image and labels are conditionally independent
- 먼저 auxiliary encoder와 테스트 시간 최적화를 통해 임베딩 z* 를 유추하여 새 이미지 x* 에 레이블을 지정할 수 있다
- 그 후, 사응하는 pixel-wise label y* 를 얻을 수 있다.


## Model
- Generator: 각 style layer에 segmentation y를 output으로 하는 branch를 더한다.
- Discriminator: Dr, Dm 두가지 사용
  - Dr: real-synthesized image 비교하는 D
  - Dm: real-synthesized image-mask pair 비교하는 D. image랑 mask concat해서 만듦. alignment 담당. Multi-scale patch-based D 사용.
- Encoder and W+-space
  -  학습기간에는 동일한 w가 모든 레이어에 적용되지만, 각 style layer를 독립적으로 모델링하는 w+ space를 사용.
  -  Mapping images x를 곧바로 w+-space에 적용하는 encoder 학습.
  -  Feature pyramid network based로 multi-level feature를 추출한다.
