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


## 학습 방법
### Invariant information clustering
x와 x에 변형을 준 x'의 representation Φ에서 공통 정보만을 뽑는다. mutual information maximization 이용한다.  
Φ(x), Φ(x')의 mutual information을 최대화하므로 instance-specfic 한 정보를 제거할 수 있다. (Φ는 cluster 개수)   
또한, 출력단의 차원을 cluster 개수로 제한하고 입력이 어떤 cluster 에 속하는지를 확률로 나타내기 위해 Φ의 마지막에 softmax 추가  

군집할당 엔트로피 H(z)를 최소화하는 조건을 꼭 추가해야함.  

### Image clustering
10개 클래스에 해당안되는 다른 데이터들을 활용하여 더 좋은 representation을 추출한다.  
Main head에서 정해진 cluster 개수로 학습, 나머지는 auxiliary head에서 over-clustering.
