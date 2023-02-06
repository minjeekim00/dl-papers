# Abstract
- Conditional DPM + attention을 이용하여 MRI sub-volume 생성
- 슬라이스 인덱스에서 attention weight를 가져와 target / conditional 슬라이스를 인코딩하는 mask를 사용하여 long-range dependency 잃지 않고 생성 가능
- 첫번째 subset을 생성하고, 점진적으로 conditionally 이후 슬라이스를 생성


# Methodology
### Diffusion Probabilistic Model
- Reverse diffusion process는 posterior distribution을 근사하는것이 목표기 때문에, random noise로부터 realistic X0를 재생성한다.
- Xt로부터 시작하는 posterior distribution trajectory

### Conditional Generation with DPM
- 학습: X로부터 랜덤샘플한 두 셋트의 슬라이스를 condition 그룹에 배치한다.
- Subset + slice index 셋트 C (condition group)와 P(target group)로 나눈다.
- 현재 관측된 slice를 통해서 target MRI를 reconstruct 해야한다.
- C와 P의 슬라이스 갯수를 제한하는 대신에, max number만 정해둔다. (len(C)+len(P)<=Tmax)
- 학습 중에 생성 대상이 되려면 X_P에 적어도 하나의 슬라이스가 있어야 하지만 X_C는 앞서 언급한 제약 조건을 충족하는 한 임의의 수의 슬라이스를 가질 수 있다.
- len(C)=0이면, 모델은 unconditional generation을 줄일 것이다.
- 연속적인 시퀀셜 샘플링과 비교하여, 더 많은 데이터 슬라이스 조합을 볼 수 있고 다양한 넘버의 관측 슬라이스로 인해 일반화를 증가시키고 long-range dependency를 감지할 수 있다.
- X^C와 X^P를 풀링하고 난 후, 어떤 슬라이스가 condition이고 generation target인지를 encode하기 위해 binary mask를 생성한다.
- Mask위에 index C, P를 모델에 인풋으로 넣어서, 두 슬라이스 index 사이의 distance를 기반으로 한 attention weight를 계산하도록 한다.
