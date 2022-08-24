Self-supervised learning in medicine and healthcare
===

<aside>
💡 Unbiased data를 해결하기 위한 유망한 self-supervised learning 기법들을 소개하고, 의료 영상에 적용됐을 때 challenge와 opportunities를 소개.
중요한 결론: augmentation이 가장 중요하다!!!

- Contrastive learning vs Geneative learning
- 모달리티: EHR, chest X-rays, electrocardiograms, protein sequence

- Transfer learning도 좋지만, 자연 영상의 특성이 의료 영상에 도움이 안될 수 있음
    - 따라서 self-supervised learning이 더 나은 학습방법일 수 있음.
- Constrastive learning에 쓰일 augmentation 선정이 중요하다.
    
</aside>



- 현재 의료 연구는 retrospective하게 수집한 데이터를 사용하고 있고, 이에 따라 모델이 편향될 확률이 크다. (task에 맞추어 데이터 수집을 했으므로)
- 실제 임상 환경과 비슷하게 multi-modal 데이터를 사용한 self-supervised learning으로 보다 generalizable한 학습이 가능할 수 있다.
 (ex. Featurizer가 underlying disease에 대한 feature를 학습할 수 있음)
