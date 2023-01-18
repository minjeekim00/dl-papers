Video generation: 3D auto-encoder + auto-regressive + latent diffusion

## Long Video Generation
### Autoregressive Video Generation
- Conditional latent diffusion -> 이전 latent code를 condition으로 하여 autoregressive하게 학습.
- Latent clip의 각 비디오 프레임의 채널에 condition 인지 predict할 frame인지에 대한 additional binary map 추가 
- Binary 맵을 0,1로 설정 -> uncondition, condition 모두 가능

### Hierarchical Video Generation
- Autoregressive -> quality degradation의 문제가 있음. (에러가 축적됨)
- Sparse frame 학습하여 뼈대 만들고 interpolation model이 missing frame을 채움.

### Conditional Latent Pertubation
- 이전 생성 step에서 유발된 conditional error를 완화시키기위해 conditional pertubation 사용
- => Conditional frames에 diffusion process를 적용
- 샘플링시, 고정된 노이즈 레벨에 노이즈를 추가해 autoregressive prediction을 이용.
- Conditional noise augmentation에서 아이디어를 받음. (cascaded diffusion models)
