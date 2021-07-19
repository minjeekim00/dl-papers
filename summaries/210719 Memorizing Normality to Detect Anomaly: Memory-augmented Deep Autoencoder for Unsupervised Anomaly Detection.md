https://arxiv.org/pdf/1904.02639.pdf

Memorizing Normality to Detect Anomaly: Memory-augmented Deep Autoencoder for Unsupervised Anomaly Detection
===

```
정상 샘플로만 학습됐더라도, autoencoder는 일반화를 잘하기 때문에 학습 샘플에 존재하지 않는 anomaly까지 reconstruction을 해버린다.
이러한 단점을 극복하기 위해 `memory module`을 사용하여 정상 샘플을 encoding하고, query로 이용하여 최대한 비슷한 샘플로 reconstruction 하는 방식을 제안한다.
```
