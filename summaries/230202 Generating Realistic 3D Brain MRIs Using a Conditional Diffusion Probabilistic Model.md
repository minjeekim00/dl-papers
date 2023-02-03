# Abstract
- Conditional DPM + attention을 이용하여 MRI sub-volume 생성
- 슬라이스 인덱스에서 attention weight를 가져와 target / conditional 슬라이스를 인코딩하는 mask를 사용하여 long-range dependency 잃지 않고 생성 가능
- 첫번째 subset을 생성하고, 점진적으로 conditionally 이후 슬라이스를 생성


# Methodology
### Conditional Generation with DPM
- 학습: X로부터 랜덤샘플한 두 셋트의 슬라이스를 condition 그룹에 배치한다.
- 슬라이스 인덱스 셋트 C (condition group)와 P(target group)로 나눈다.
