# Abstract
- forward/backward가 모두 가능할 때 frame ordering의 애매모호함을 학습
- 각 비디오 시퀀스에 명확한 order를 주어 order-ambiguity 문제를 해결
- 각 비디오를 latent high-dimensional space에 매핑하여 모든 비디오 시퀀스에 hyperplane이 존재하도록 한다.
- 그곳에서 뽑힌 vector와, 그 반대 시퀀스가 hyperplane 상에서 다른 면에 위치하도록 한다.

# Methodology

### HyperCUT order
- 각 target frame index $k$에 대해서, $y^i$로 부터 $x_k^i$를 예측하기 위한 네트워크 $f_k$를 학습한다.
- 개념적으로, $f_k$의 출력은 학습에 사용된 모든 샘플링된 목표의 평균으로 수렴할 것으로 알려져 있다.
- 따라서 $H(x^i_k,x^i_{N-k})$와 $H(x^i_{N-k}, x^i_k)$가 다른 side에 있다는 mapping을 찾고 싶음.
 => 곱했을 때 0보다 작으면 된다.

### Addressing order-ambiguity with HyperCUT
- $H$ function은 다른 loss와 합쳐져 order ambiguity 문제를 해결하는 regularization 역할을 한다.
- <b> 네트워크의 output에 해당하는 vector를 hyperplane의 같은 side에 놓이게 강제한다.</b>  
  => HyperCUT regularization을 추가하여 이루어짐
- $H$는 pretrained network로, deblurring netowrk 학습시에는 frozen됨.
