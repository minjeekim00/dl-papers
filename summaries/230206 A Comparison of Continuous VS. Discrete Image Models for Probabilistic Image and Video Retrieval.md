# Abstract
- "continuous" GMM 모델에 의해 visual content가 capture될 수 있음.
- "discrete" multinomial model (full-text retrieval에 쓰임)로 visual content 모델링이 가능한지 관측
- continuous vs. discrete keyframe 모델을 비교하여 retrievel effectiveness를 판단. 


# Experiments
- GMM의 단점: indexing time: iterative 알고리즘이 모델 파라미터, 리트리벌 타임을 결정해야해서 오래걸림
- MNM: 프로세싱을 제한하기 때문에 빠르다. 하지만, feature space를 discrete하게 partitioning하기 때문에, indexing time에 grid cell 수가 고정된다.


# Conclusion
- Image retrieval 문제에서 language model을 이용하여 information retrieval을 적용한 논문.
- Discrete model (multinomials) vs Continuous model (GMM).
- Continuous model이 퍼포먼스가 상당히 더 좋았음
- 하지만, 몇몇 queries는 discrete 모델이 더 효율적이었다.
- Grid cell의 정의를 다시 정하여 추가 연구 예정.
- 샘플링 과정을 향상하는 것도 유익할 것으로 보임.
- 또 다른 방법으로는 다양한 크기의 overlapped 이미지 패치에서 특징 벡터를 그려서 이미지의 multi scale representation을 만드는 것.
- 하나의 keyframe 이미지보다 Spatio-temporal information를 넣은 것이 유망했음.
