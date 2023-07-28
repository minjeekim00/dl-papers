# Abstract
- CNN의 inductive bias(local interaction 잘배움) + Transformer의 표현력을 합친 모델.

# Introduction
- Transformer는 CNN과 달리 로컬한 지역의 interaction을 전제로하는 inductive bias가 없다.
- 모든 복잡한 relationship 학습 가능하여 표현력이 좋지만 local correlation을 사전지식으로 잘 쓰는 CNN에 비해 모든 relationship을 배워야 함. (비용이 비쌈 => 고해상도로 가는데 문제가 있음.)

- Transformer가 CNN구조를 배우는 경향이 있다는 연구가 있음.
- 비전 모델을 훈련할 때마다 처음부터 로컬 구조와 이미지의 규칙성에 대해 알고 있는데 다시 배워야 할까? 아니면 Transformer의 유연성을 유지하면서 inductive bias를 인코딩하는 방법을 택할까?
- CNN은 low-level 이미지 구조에서 local connectivity에 의해 잘 표현되는 반면에 high-level로 갈수록 이러한 구조적 가정은 의미론적 수준에서는 유효하지 않음. (high-level갈수록 local correlation 중요x)
- 더욱이 CNN은 강한 지역성 편향을 나타낼 뿐만 아니라 모든 위치에서 공유 가중치를 사용하여 공간적 불변성 (spatial invariance)에 대한 편향도 나타냅니다. 
(spatial invariance(이미지가 변환되어도 그 이미지로 인식하는 것. 고양이를 90도 회전시켜놓아도 고양이로 인식하는 능력)이 부족함.)
=> 비전 테스크에 둘 다 사용해보자!

- 컨텍스트가 풍부한 시각적 부분 부분들을 코드북으로 만들어 컨볼루션으로 효율적으로 학습한 다음 전체 구성 모델을 학습한다. (local한부분은 conv + global한 부분은 transformer.)
- Adversarial 방법으로 local part의 dictionary가 low-level statistics를 잘 학습하여 transformer에 넘길 수 있는지 확인. (transformer는 long-range relations에 집중할 수 있도록)

# Related Work
## The Transformer Family
