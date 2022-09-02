#clustering

Unsupervised segmentation에서 pseudo-label을 어떻게 생성하는가?  
기존 논문 PiCIE에서는 unsupervised feature를 사용하여 label 줬더니, cluttered됨.   
Supervised model의 local feature 써야하는데, global과도 잘 working해야함.  

=> Global semantic을 활용할 수 있는 pyramid global guidance를 사용해야한다.  
=> Image-level, pixel-level 모두에서 clustering 하는 two stage 사용.  
=> Pixel-level clustering에서 CAM module 사용  
