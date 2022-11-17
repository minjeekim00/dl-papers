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
- GAN-based 
