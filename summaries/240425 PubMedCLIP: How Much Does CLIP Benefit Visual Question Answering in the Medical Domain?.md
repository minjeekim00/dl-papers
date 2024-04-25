# Abstract
- Medical Visual Question Answering(MVQA) task에 CLIP을 적용
- Pubmed article을 CLIP에 접목시킴.
- pre-trained MAML(Model-Agnostic Meta-Learning)와 비교해서 정확도 3% 우위.

# Introduction
- MedVQA와 달리, 다양한 범위의 body region을 사용하여 몇몇 장기에 국한되지 않음.

# PubMedCLIP
- 먼저, Radiology Objects in COntext (ROCO) dataset을 이용하여 image-text 쌍을 CLIP으로 학습시킨다.
- ROCO는 8만장의 초음파, X-ray, PET, CT, MRI, giography으로 다양한 body region을 포함한다.
- ROCO는 PubMed article에서 나온 데이터이고, 평균 20단어 정도로 짧은 caption을 가지고 있다.
- ViT-B/32 Vision Transformer, ResNet RN-40, RN-50x4 사용
- 맥시멈 텍스트 길이가 76이므로, 더 길면 trim하고, 짧으면 zero-padding 적용
- 그냥 CLIP과 비교했을 때 embedding이 훨씬 더 뭉쳐있음

# PubMedCLIP for MedVQA
- 
