#
Diffusion process를 활용한 D를 사용한 GAN.
(기존 방법) Guassian distribution을 가진 noise를 여러번 주입
(제시된 방법) 여러번의 step을 퉁쳐서 Gaussian mixture distribution로 모델링.
- Noise-to-data ratio마다 image-noise 조합을 달리하여 weighted함.
- Diffusion step도 "adaptive"하게 조정됨.
- Real/Fake의 Support가 겹치지 않아도 학습이 잘됨.
- Real/Fake 이미지 모두에게 noise를 injection하기 때문에 일종의 augmentation. => 모듈처럼 갖다붙일 수 있음.
