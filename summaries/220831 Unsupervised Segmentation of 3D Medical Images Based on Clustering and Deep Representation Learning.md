#

1. CNN에서 생성된 representation을 cluster하여 patch당 deep feature representation을 뽑아낸다. 
2. Cluster label를 supervisory signal로 이용하여 CNN parameter를 업데이트한다.
3. CNN에서 뽑힌 deep representation에 k-means를 적용한다. 
4. 타겟 이미지에 cluster label을 project해서 segmented image를 얻는다.


- JULE와 k-means를 합친데에 의의가 있음. (느리지 않을까?)
- JULE이 다른 clustering 알고리즘과도 잘 working 하므로, 타겟 이미지의 패치에 JULE 적용 후 더 빠른 clustering 알고리즘에 적용가능.
- JULE로 나눠진 clustering을 label에 assign하는 데 k-means clustering 사용.



1. 학습된 CNN으로 patch단위 feature representation을 뽑는다.
2. Feature representation을 k-means로 K cluster로 나눈다.
3. 각 cluster를 label로 assign하고, original image에 붙인다.
4. Euclidean distance 상에서 가까운 cluster representation은 같은 label을 붙인다.
