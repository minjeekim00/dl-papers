# Abstract

- CycleGAN을 이용한 semi-supervised semantic image segmentation.
- Cycle consistency를 Unpaired image와 segmentation mask 사이의 bidirectional mapping에 적용
- Unsupervised regularization이 segmentation performance를 증가.


# Introduction
- GAN 모델을 이용하여 효율적인 domain adaptation이 가능.
- 이 때, source-target 데이터의 분포를 매칭하는데, 주로 input단이나 feature단에서 이루어진다.  

- CycleGAN은 source-target의 매핑을 찾아내는데, 이 때 cycle consistency loss를 사용하여 key attribute 입력과 변환된 이미지 사이의 key attribute만 뽑아서 보존한다.
- 전통적인 semi-supervised 방법처럼 label-unlabel 데이터간의 domain shift는 아님.  

- 이 논문에서는, label을 사용한 전통적인 supervised mapping을 unsupervised regularization loss로 사용하여 제한된 데이터로 학습이 가능하게 한다.
- Cycle consistency loss가 강요되지 않아 기존 방법과 다르다.  

## Main Contribution
<b> cycle consistent mapping between real images-ground truth as unsupervised regularization prior </b>


# Methodology
