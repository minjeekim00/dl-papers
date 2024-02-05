# Abstract
- 문제: Multi-label 의료 영상-판독문 페어에서는 CLIP이 잘 작동안함
- 해결: Sentence sampling과 positive-pair loss relaxation으로 해결


# Related works
### Text augmentation
- Token-level augmentation (random insertion or deletion)이 주로 이루어짐.
- 의료 판독문은 짧은 정보 집약적 문장으로 구성되어 있기 때문에 정보 손실의 위험이 높음
- 이 논문에서는 sentence-level random selection을 사용.
### Loss relaxation for constrastive learning
- 
