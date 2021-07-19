

Memorizing Normality to Detect Anomaly: Memory-augmented Deep Autoencoder for Unsupervised Anomaly Detection
===
https://arxiv.org/pdf/1904.02639.pdf  
> 정상 샘플로만 학습됐더라도, autoencoder는 일반화를 잘하기 때문에 학습 샘플에 존재하지 않는 anomaly까지 reconstruction을 해버린다.
> 이러한 단점을 극복하기 위해 `memory module`을 사용하여 정상 샘플을 encoding하고, query로 이용하는 방식을 제안한다.
>   학습 과정: 정상 데이터의 전형적인 특징 배우기
>   테스트 과정: 학습된 memory는 고정시킨채 memory records로만 reconstruction 실행
