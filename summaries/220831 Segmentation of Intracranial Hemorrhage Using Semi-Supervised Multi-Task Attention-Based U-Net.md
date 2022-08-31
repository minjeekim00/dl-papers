#

" Multi-Task Attention-Based Semi-Supervised Learning for Medical Image Segmentation " 을 활용.
적은 량의 Labeled 데이터를 활용한 semi-supervised 방식으로 segmentation 구현.
Supervised set의 데이터를 점점 줄이는 방식의 curriculum learning 방식을 활용.



- Modified U-Net과 curriculum learning 학습 전략.
- Unsupervised segmentation clustering + 패치 학습을 통한 deep feature representation 학습.



- Multi-task reconsturction을 이용하여 제한된 데이터로 학습 가능.
- Inverse sigmoid curriculum learning을 사용하여 supervised > unsupervised 에서 supervised < unsupervised로 portion을 늘림.
- Jaccard loss, Weighted MSE loss 사용 
