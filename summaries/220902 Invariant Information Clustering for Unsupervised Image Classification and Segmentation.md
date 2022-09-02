#

각 pair의 class assignments마다 mutual information을 maximaize하여 학습.


## k-means 문제점
특히 representation learning과 합쳐질 때 하나의 cluster가 다른 cluster를 dominate해버림.  
=> Entrophy maximization에서는 한 class로만 쳐지면 minimize가 안되므로 해결.

## over-clustering 문제
Auxiliary output layer로 test 타임에는 무시.


## Semantic clustering vs intermediate representation learning
GGDR이 intermediate representation learning으로 볼 수 있다.
* Representation learning은 continuous하기 때문에 k-means같은 방법으로 후처리가 필요함.
* Semantic clustering은 바로 discrete output 제공.
