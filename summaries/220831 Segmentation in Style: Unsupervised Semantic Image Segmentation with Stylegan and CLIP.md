#

#CLIP #clustering #knowledge-distilation


- Pretrained StyleGAN2를 사용하여, G의 feature space를 clustering하여 semantic class를 발견한다.
- Class가 발견되면, 합성 이미지와 segmentation mask가 생성된다.
- 또한 CLIP를 사용하여 자연어로 정의된 프롬프트를 사용하여 원하는 클래스를 찾을 수 있다.



- Annotation 사용 x
- 잘 찾아지지 않는 rare한 class의 경우는 image manipulation 사용. (CLIP의 text prompts 사용)
- Knowledge distillation 사용하여 synthetic image에 segmentation network 학습.



### Clustering
- StyleGAN의 7~9 Layer를 활용.
- N개의 sample을 뽑아서 64 * 64 * 512 or 128 * 128 * 256 레이어 활용.
- 512 size의 feature map N * 64 * 64 or 256 size의 feature map N * 128 * 128 사용
- 이 feature map을 k-means clustering 한다. => Class 부여
- !!! 한번 cluster를 찾으면, 새로운 샘플의 feature에도 closer cluster를 붙이면 같은 segmentic region이 발견됨. !!!
- 이 cluster로 segmentation mask를 얻어 학습 가능. 


### Class Discovery wih Image manipulation
- 원하는 이미지 속성을 찾기 어려운 경우, 그 속성을 갖도록 w를 업데이트함.
- CLIP을 사용하여 latent space에서 원하는 속성의 direction을 찾음. (????)

### Cluster Classification
- Semantic segmentation에 class 부여하기.
- 모든 이미지의 임베딩을 각 클러스터당 평균을 낸다. => Cluster의 방향 파악가능
