# Abstract
- 의료 도메인에서의 vision-language pre-training(VLP) 부재.
- General prompt를 사용하여 image-label 쌍을 image-text로 확장하고, 판독문의 섹션을 이용.
- 또한 의료 이미지와 보고서의 study-level 특성을 학습하기 위해 각각 ICL과 TCL이라는 두 contrastive loss 적용

# Introduction
- MedCLIP: rule-based 레이블러를 이용하기 때문에 레이블러의 성능에 의존함.
- DeCLIP: CXR 스터디에서 여러개의 이미지와 텍스트를 이용하는 multi-view supervision (MVS)를 사용하여 효율적인 학습.
- 각각에 image / text - contrastive loss (ICL/TCL) 적용
- 
