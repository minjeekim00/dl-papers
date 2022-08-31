


<br> Better representation <-> Clustering </br>
Representation을 잘 배우면 Clustering을 잘 할 수 있고, 더 잘 된 Clustering으로 더 나은 Representation을 학습할 수 있다.


## Agglomerative clustering

## Experiments
- 모든 test 알고리즘에서는 image intensity에 L2-norm 사용.
- OURS-SF (straight-forward). recurrent로 clustering 끝냈을 때
- OURS-RC (re-running clustering). final representation으로 적용.

1. MNIST-full 에서만 poor 하게 동작. 70k 이미지에서 image intensity를 representation으로 사용하면 안되는 듯.
2. 학습된 representation을 사용하면 모든 데이터셋에서 더 나은 clustering을 하는 것을 확인. (over-fitting x)
3. 한 데이터셋에서 학습한 clustering이 관련 있는 다른 데이터셋에도 적응되는 것을 확인.
4. 분류 문제에서도 학습된 representation을 사용하면 성능이 향상됨. (conv1 + conv2 합산이 가장 성능 좋음)
