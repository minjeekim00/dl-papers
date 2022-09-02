#clustering

Unsupervised segmentation에서 pseudo-label을 어떻게 생성하는가?
- Supervised pretrained model의 local feature는 object label에 잘 튜닝이 된다.
- 따라서 dataset clustering을 할 때 global guidance와 잘 working 할 수 있다.
- 하지만, Unsupervised feature를 사용했을 때는 그러지 못하다.

=> Global semantic을 활용할 수 있는 pyramid global guidance를 사용해야한다.
=> Image-level, pixel-level 모두에서 clustering 하는 two stage 사용.
=> Pixel-level clustering에서 CAM module 사용
