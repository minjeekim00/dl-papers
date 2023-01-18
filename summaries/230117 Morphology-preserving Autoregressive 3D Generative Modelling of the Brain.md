VQ-VAE + Transformer


# Background
1. VQ-VAE로 latent representation 배우기
2. Transformer로 flattened representation의 maximum likelihood 

### VQ-VAE
- Encoder E에 의해 x -> z로 latent embedding이 이루어짐
- 그 다음, 각 spatial code z에 대해 코드북에서 가장 가까운 백터 ek로 element-wise quantization이 이루어진다. (k는 코드북의 어휘 크기)
- Quantized latent space를 기반으로, decoder G는 x를 reconstruction한다.
- 각 코드북 element 벡터 z를 그와 관련된 index k로 대체하여 latent discrete representation을 얻을 수 있다.

### Transformer
- 트랜스포머 모델과 관련된 self-attention 메커니즘은 상대적 위치에 관계없이 입력 간의 상호 작용을 캡처할 수 있다.
- VQ-VAE의 latent discrete representation은 3D이므로 규모가 크므로, 일반적인 transformer는 필요한 시퀀스 길이로 확장되지 못한다.
- 따라서 Performer를 사용하여 latent sequence를 모델링한다. 
- 3D latent discrete representations의 flattened 1D sequence에서 코드북 인덱스 p(s)의 조건부 분포를 최소화함으로써 데이터 로그 우도가 auto-regression 방버으로 최대화된다.
-
# Method
### Descriptive Quantization for Transformer Usage
- Transformer-based 생성 모델을 위해서는 이미지 볼륨이 1D sequence 토큰으로 표현되어야한다.
- 이를 위해, 전체적인 spatial size를 4096까지 줄여 입력 사이즈 X가 1400 토큰 시퀀스로 표현되도록 함.
- 이 1400 사이즈의 토큰 시퀀스가 VQ-VAE의 EMA 알고리즘을 이용하여 온라인으로 학습된다.
- Pixel slice L1 norm + 푸리에 representation L2 norm + LPIPS + Patch-GAN loss 

### Autoregressive Modelling of the Brain
1. T1w MRI 이미지를 z representation이 추출될때 까지 학습 (neurological)
2. ADNI subject의 zq representation이 추출될 때 까지 파인튜닝 (pathological)
3. Top 1% 생성 샘플만 사용
4. VQ-VAE: 모든 피노타입을 배움, Transformer: 각기 다른 sub-population latent representations 을 배움

# Experiments and Results
### Quantitative Image Fidelity Evaluation
- FID로 image fidelity 측정: middle slice of each axis, 3D sample에서는 배치단위 MMD에 내적 사용.
- MS-SSIM으로 diversity 측정: 생성이미지들 끼리 비교 

### Morphologial Evaluation
- Voxel-based morphometry (VBM) 사용하여 focal difference 조사
