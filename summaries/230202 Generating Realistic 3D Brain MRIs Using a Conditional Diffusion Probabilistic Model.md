Key points:
- 3D 생성을 위해 condition으로 mask, slice indices, condition sub-volume을 잘 활용한 논문.
- conditon / target을 같이 학습하되 binary mask encoding으로 둘을 구분한다
- slice index 사이의 distance로 attention weight를 계산하여 활용
  => random sampling으로 연속 시퀀셜보다 더 많은 조합 생성가능 + long-range dependency


# Abstract
- Conditional DPM + attention을 이용하여 MRI sub-volume 생성
- 슬라이스 인덱스에서 attention weight를 가져와 target / conditional 슬라이스를 인코딩하는 mask를 사용하여 long-range dependency 잃지 않고 생성 가능
- 첫번째 subset을 생성하고, 점진적으로 conditionally 이후 슬라이스를 생성


# Methodology
### Diffusion Probabilistic Model
- Reverse diffusion process는 posterior distribution을 근사하는것이 목표기 때문에, random noise로부터 realistic X0를 재생성한다.
- 주어진 샘플 Xt와의 noise와의 reconstruction loss를 최소화하는 방향으로 최적화된다.
- $X_t$로부터 시작하는 posterior distribution trajectory

### Conditional Generation with DPM
<학습>
- 3D MRI $X$로부터 랜덤샘플한 두 셋트의 슬라이스를 condition와 target 그룹에 배치한다. (각각 길이는 C, P)
- 현재 가지고 있는 관측 데이터 $X^C$를 토대로, 모델은 target MRI $X_t^P$를 reconstruction 한다.  
- C와 P의 슬라이스 갯수를 제한하지 않고, max number만 정해둔다. $len(C)+len(P)<=T_{max}$  
- 학습 중에 생성 대상이 되려면 $X_P$에 적어도 하나의 슬라이스가 있어야 하지만 $X_C$는 앞서 언급한 제약 조건을 충족하는 한 임의의 수의 슬라이스를 가질 수 있다.  
- $len(C)=0$이면, 모델은 unconditional generation을 줄일 것이다.
- <b>연속적인 시퀀셜 샘플링과 비교하여, 더 많은 데이터 슬라이스 조합을 볼 수 있고 다양한 넘버의 관측 슬라이스로 인해 일반화를 증가시키고 long-range dependency를 감지할 수 있다.</b>
- $X^C$와 $X^P$를 풀링하고 난 후, 어떤 슬라이스가 condition이고 generation target인지를 encode하기 위해 binary mask를 생성한다. (condition - 0, target - 1)
- Encoding한 mask뿐만 아니라, index C, P 자체 또한 모델에 인풋으로 넣어서, 두 슬라이스 index 사이의 distance를 기반으로 한 attention weight를 계산하도록 한다.
