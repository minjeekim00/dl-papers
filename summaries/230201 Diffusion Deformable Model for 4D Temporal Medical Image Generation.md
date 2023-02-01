
# Abstract
- Temporal medical image generation (4D cardiac volume data)
- Source - Target volume 중간의 intermediate temporal volume 만들기
- Source - Target volume 사이의 spatial deformation information을 배우고, 중간 프레임을 생성할 수 있는 latent code를 제공
- 학습이 완료되면, latent code가 interpolate되어 deformation module에 넣어짐.
- => DDM이 continuous trajectory를 따라 temporal frame을 생성한다. (이 때 source image의 topology 유지가능)

# Proposed Method
- Diffusion module + Deformable module
- Diffusion module: 소스와 타겟 사이 중간 프레임의 공간 정보를 제공하는 latent code c를 예측.
- Deformable module: Code c에 따른 trajectory에 따라 deformed 이미지 생성.


### Deformation Module
- Diffusion module이 latent code c를 출력하면, deformation module이 c에 따른 deformed 이미지 생성.
- Learning-based image registration 방법 사용.
- 소스 이미지 S로부터 registration field φ 예측.
- Tri-linear interpolation을 이용한 spatial transformation layer(STL)를 사용하여 deformed 소스 이미지 S(φ) 생성
- Source / Target 이미지를 직접 입력으로 넣는 것이 아닌, latent code c를 사용.
