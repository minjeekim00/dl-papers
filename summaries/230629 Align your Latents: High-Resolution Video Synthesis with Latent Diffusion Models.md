<b> Temporal 하게 consistent함을 강제  
 Alignment layer 
 3D로 학습됨, AE에서 decoder에만 temporal layer 추가 </b>


# Abstract
- image로 LDM pretrain -> diffusion 모델에 temporal 차원 도입.
- 인코딩된 이미지 시퀀스, 즉 비디오를 미세 조정하여 image -> video generator로 전환

# Related work
### Video synthesis
- 이전 모델들은 transformer에 기반한 auto-regressive 구조를 가지고, discrete latent space에서 학습됨.
- 이 논문에서는 continuous DM이며 parameter가 적고 CogVideo의 성능을 능가한다.
- 또한 alignment layer에 auto-regressive masking대신 condition on the text prompt 사용.

# Latent Video Diffusion Models

#### Temporal Autoencoder Finetuning
- LDM의 오토인코더는 이미지에만 학습되므로, 나중에 시간축으로 이어질 때 flickering artifacts를 유발함.
- 오토인코더의 decoder에 temporal layer를 추가하여, 3D conv로 구축된 패치 방식 temporal 판별기로 비디오 데이터를 fine-tune한다.


# Conclusions
- 자율주행 연구 시뮬레이터나 고품질 비디오 콘텐츠 제작에 사용 가능
